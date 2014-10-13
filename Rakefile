task :default => :build

desc 'Build site with Jekyll'
task :build do
  jekyll 'build'
end

desc 'Build and start server with --auto'
task :server do
  jekyll 'serve --watch'
end

def jekyll(opts = '')
  sh 'rm -rf _site'
  sh 'jekyll ' + opts
end
