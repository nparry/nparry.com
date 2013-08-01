# Stolen from https://github.com/tatey/tatey.com/

task :default => :server
 
desc 'Build site with Jekyll'
task :build do
  jekyll 'build'
end
 
desc 'Build and start server with --auto'
task :server do
  jekyll 'serve --watch'
end

desc 'Push repository to github'
task :gitpush do
  sh 'git push'
end

desc 'Build and deploy'
task :deploy => [:build, :gitpush] do
  sh 'rsync -rtzh --progress --delete _site/ nparry.com:~/nparry.com/'
end

def jekyll(opts = '')
  sh 'rm -rf _site'
  sh 'jekyll ' + opts
end
