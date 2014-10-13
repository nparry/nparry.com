task :default => :build

desc 'Build site with Jekyll'
task :build do
  jekyll 'build'
end

desc 'Serve site locally with Jekyll'
task :serve do
  jekyll 'serve --watch'
end

def jekyll(opts = '')
  sh 'jekyll ' + opts
end
