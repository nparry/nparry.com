---
layout: post
date: 2015-11-14
title: SSL setup with Let's Encrypt on AWS CloudFront and S3
---

With the [Let’s Encrypt](https://letsencrypt.org/) project entering public
beta, I thought I should figure out how to make SSL certificates issued by
Let's Encrypt work with my sites hosted on AWS S3.

A bit of searching uncovered this [very helpful
guide](https://www.ikusalic.com/blog/2015/01/31/adding-https-support-to-static-site-hosted-in-s3/)
on setting up CloudFront and S3 with your own SSL certificate. All that
remained was to adapt the process outlined there to work with how Let's Encrypt
issues certificates.

The "easy" mode of the Let's Encrypt client assumes you are running it on the
same machine that is hosting your site. Obviously this is not true if your site
is hosted from S3, so some extra manual steps are necessary. I've outlined the
steps below - at a high level, the process is:

* Setup HTTP based CloudFront access for your site.
* Use the Let's Encrypt client and the initial CloudFront configuration to get a SSL certificate.
* Update the CloudFront configuration to use that certificate.

In full detail, the process is:

### Step 0: Create an S3 bucket for your site

Create your S3 bucket and upload your files by whatever method makes sense for
you. For this site I use [the Travis CI S3
deployer](http://docs.travis-ci.com/user/deployment/s3/). Your S3 bucket does
not need to be publicly readable - we'll set up access
specifically for CloudFront later.

### Step 1: Set up a CloudFront distribution for your S3 bucket

From [the CloudFront main page](https://console.aws.amazon.com/cloudfront/), pick *Create Distribution* and
choose a Web Distribution. This brings up a page with many options - you'll
need to pay attention to the following:

<table class="table">
<tr><td>Origin Domain Name</td><td>Pick your S3 bucket from the dropdown.</td></tr>
<tr><td>Restrict bucket access</td><td>Yes.</td></tr>
<tr><td>Origin Access Identity</td><td>Create a New Identity.</td></tr>
<tr><td>Grant Read Permissions on Bucket</td><td>Yes, Update Bucket Policy.</td></tr>
<tr><td>Viewer Protocol Policy</td><td>HTTP and HTTPS - we'll change this later once we have our certificate.</td></tr>
<tr><td>Alternate Domain Names</td><td>Enter the domain name for your site.</td></tr>
<tr><td>SSL cert</td><td>Default CloudFront Certificate - we'll also be changing this later.</td></tr>
<tr><td>Default root object</td><td>Probably <code>index.html</code> or whatever makes sense for your site.</td></tr>
</table>

I left all other options with their default value. Once you confirm your options,
wait for the *Status* of your distribution to change to *Deployed* - seems to
take 5 or 10 minutes.

### Step 2: Update DNS to point your domain to CloudFront

This is highly dependent on who you are using for DNS - I'll assume you are
using [Route 53](https://console.aws.amazon.com/route53/). Create two records
for your domain of type `A` and `AAAA`. Both should have *Alias* set to *Yes*
and *Alias Target* set to the *Domain Name* of your CloudFront distribution.

At this point you should be able to `curl -D - http://your-site.whatever` and
retrieve your site. You should also see CloudFront headers in the HTTP
response.

### Step 3: Create the SSL certificate

You are now ready to get your SSL certificate from Let's Encrypt. As of this
writing, the [letsencrypt-auto client](https://letsencrypt.readthedocs.org/en/latest/using.html#letsencrypt-auto)
issues dire warnings when run on OS X, so I used the
[Docker](https://letsencrypt.readthedocs.org/en/latest/using.html#running-with-docker)
method of running it.

In addition to Docker, you'll need the [AWS CLI](https://aws.amazon.com/cli/) -
I installed it with [Homebrew](http://brew.sh/). [Configure the CLI with your
AWS credentials](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-quick-configuration)
and ensure whatever credentials you use have access to manage
[IAM](https://console.aws.amazon.com/iam/) - you probably want to attach the
*IAMFullAccess* policy to your user.

With all of that setup out of the way, run the following commands:

    # Create a place to store the client config and output
    mkdir -p ~/lets_encrypt/{etc,lib}

    # Run the Let's Encrypt client via Docker
    docker run -it --rm --name letsencrypt \
      -v "~/lets_encrypt/etc:/etc/letsencrypt" \
      -v "~/lets_encrypt/lib:/var/lib/letsencrypt" \
      quay.io/letsencrypt/letsencrypt:latest \
      --agree-dev-preview \
      --server  https://acme-v01.api.letsencrypt.org/directory \
      -a manual \
      auth

After answering several questions about email addresses and domain names,
you'll be given a prompt like:

    Make sure your web server displays the following content at
    http://your-site.whatever/.well-known/acme-challenge/some_long_path before continuing:

    some_long_string

    Content-Type header MUST be set to text/plain.

    ... <snip> ...
    Press ENTER to continue

You need to upload a file to your S3 bucket with the specified content - using
the AWS CLI you can do that thusly (replacing *some_long_string* and
*some_long_path* with the values from the prompt):

    # Upload the verification file to your S3 bucket
    printf "%s" some_long_string > /tmp/acme-challenge
    aws s3 cp \
      /tmp/acme-challenge \
      s3://your_s3_bucket_name/.well-known/acme-challenge/some_long_path \
      --content-type text/plain

    # Sanity check that the file is there
    curl -D - http://your-site.whatever/.well-known/acme-challenge/some_long_path

Once the file is in place, press *Enter* to continue the Let's Encrypt client. If
all goes well, you'll see something like `Congratulations! Your certificate and
chain have been saved at /etc/letsencrypt/live/your-site.whatever/fullchain.pem`.

You can now upload the SSL certificate to AWS for use in CloudFront:

    aws iam upload-server-certificate \
      --server-certificate-name your-site.whatever \
      --certificate-body file://~/lets_encrypt/etc/live/your-site.whatever/cert.pem \
      --private-key file://~/lets_encrypt/etc/live/your-site.whatever/privkey.pem \
      --certificate-chain file://~/lets_encrypt/etc/live/your-site.whatever/chain.pem \
      --path /cloudfront/

### Step 4: Update CloudFront config to use the SSL certificate

Return to [CloudFront](https://console.aws.amazon.com/cloudfront/), pick your
distribution and select *Edit* from the *General* tab. Change *SSL Certificate*
to *Custom SSL Certificate* and pick the certificate you just uploaded.

Next, pick the *Behaviors* tab and edit the existing behavior, changing *Viewer
Protocol Policy* to *Redirect HTTP to HTTPS*.

At this point, you should have a valid HTTPS only website. It seems to take a
little while for the CloudFront SSL settings to propagate, but eventually a
test like the following should work:

    # Verify that a request for the non-encrypted site redirects to HTTPS
    curl -D - http://your-site.whatever

    # Verify that a request using HTTPS returns your page as expected
    curl -D - https://your-site.whatever

Overall this is a lot of manual steps, but much of this is entirely scriptable.
Ideally this would be supported directly within the `letsencrypt-auto` client.
We'll see how much free time I have. :-)

