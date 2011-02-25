task :default => 'jekyll:compile'

namespace :jekyll do
    desc "Delete generated Jekyll files"
    task :clean do
        system "rm -rf _site"
    end

    desc "Start the Jekyll server"
    task :server => [:clean, 'compass:compile'] do
        system "jekyll --auto --server"
    end

    desc "Clean and compile Jekyll + Compass"
    task :compile => [:clean, 'compass:compile'] do
        system "jekyll"
    end
end

namespace :compass do
    desc "Run the Compass watch script"
    task :watch do
        system "compass watch"
    end

    desc "Compile SCSS files"
    task :compile do
        system "compass compile --force"
    end
end
