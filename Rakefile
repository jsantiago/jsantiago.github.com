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
        require 'erb'
        require 'jekyll'

        puts "Generating tags..."

        # Read Jekyll site
        options = Jekyll.configuration({})
        site = Jekyll::Site.new(options)
        site.read_posts('')

        # Generate page for each tag
        site.tags.each do |tag, posts|
            # Fill out template
            template = ERB.new( File.open("_templates/tag.erb", "r") { |f| f.read } )
            html = template.result(binding)

            # Write tagged post to tags dir
            posts.each do |post|
                post.write("tags/"+tag)
            end

            # Write template
            File.open("tags/#{tag}/index.html", "w+") do |file|
                file.puts(html)
            end
        end
    end
end

namespace :post do
    desc "Create a new post. Post directory defaults to '_posts'"
    task :new, [:title, :dir] do |t, args|
        require 'erb'

        # Default args
        args.with_defaults(:dir => "_posts")

        puts "Creating new post..."

        # Generate slug
        slug = args.title.downcase.gsub(/[^a-z0-9]+/, '-').chomp('-')

        # Filename must be in format YYYY-MM-DD-title.<extension>
        now = Time.new.strftime("%F")

        # Count number of posts for today, append to title to sort properly
        count = Dir["#{args.dir}/#{now}*.md"].length

        # Open default post template
        template = ERB.new( File.open("_templates/post.erb", "r") { |f| f.read } )
        post = template.result(binding)

        # Write default post
        filename = "#{now}-%02d-#{slug}.md" % count
        File.open("#{args.dir}/#{filename}", "w+") do |file|
            file.puts(post)
        end

        # Edit post
        system "vim #{args.dir}/#{filename}"
    end
end
