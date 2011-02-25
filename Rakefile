task :default => 'jekyll:compile'

namespace :jekyll do
    desc "Delete generated Jekyll files"
    task :clean do
        puts "Deleting generated Jekyll files..."
        system "rm -rf _site"
        system "rm -rf tags/*"
        puts
    end

    desc "Start the Jekyll server"
    task :server => [:clean, 'compass:compile'] do
        system "jekyll --auto --server"
    end

    desc "Clean and compile Jekyll + Compass"
    task :compile => [:clean, 'compass:compile', 'tags:generate'] do
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
        puts "Compiling SCSS files..."
        system "compass compile --force"
        puts
    end
end

namespace :tags do
    desc "Generate tags"
    task :generate do
        puts "Generating tags..."
        require 'jekyll'
        options = Jekyll.configuration({})
        site = Jekyll::Site.new(options)
        site.read_posts('')
        site.tags.sort.each do |tag, posts|
            html = <<-HTML
---
layout: archive
title: #{tag}
---

<ul>
<li>Posts tagged as '#{tag}':</li>
<ul>
            HTML
            posts.each do |post|
                post.write("tags/"+tag)
                html << <<-HTML
                    <li><a href="#{post.url}">#{post.data['title']}</a></li>
                HTML
            end
            html << <<-HTML
                </ul>
                </ul>
            HTML
            File.open("tags/#{tag}/index.html", "w+") do |file|
                file.puts html
            end
        end
    end
end
