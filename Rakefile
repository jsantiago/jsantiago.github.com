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

namespace :post do
    desc "Create a new post. Post directory defaults to '_posts'"
    task :new, [:title, :dir] do |t, args|
        args.with_defaults(:dir => "_posts")
        puts "Creating new post..."
        slug = args.title.downcase.gsub(/[^a-z0-9]+/, '-').chomp('-')
        now = Time.new.strftime("%F")
        count = Dir["#{args.dir}/#{now}*.md"].length
        post = <<-POST
---
layout: post
title: #{args.title}
published: true
---

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nullam dolor mi, scelerisque a iaculis sed, venenatis eu orci. Donec viverra turpis est. Fusce pharetra porttitor nisi nec dignissim. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam vel nulla magna, id gravida libero. Aliquam erat volutpat. Sed accumsan risus id sem rhoncus in placerat lacus luctus. Maecenas eu enim ipsum. Integer vel lectus vitae lacus pellentesque bibendum. Pellentesque eros erat, volutpat at consequat ac, commodo ut orci. Nulla facilisi. Integer dictum, nibh eu ultricies laoreet, nunc nisi vestibulum nisi, ut bibendum tortor purus id mauris. Nulla sed turpis eleifend magna malesuada luctus id eget arcu. Suspendisse vehicula augue pulvinar nunc lacinia ac tempor augue rutrum. Praesent ac nulla eros, id pretium nisl.
        POST
        filename = "#{now}-%02d-#{slug}.md" % count
        File.open("#{args.dir}/#{filename}", "w+") do |file|
            file.puts(post)
        end
        system "vim #{args.dir}/#{filename}"
    end
end
