task :default => :serve

namespace :site do
  task :build do
    puts "Building..."
    sh "bundle exec jekyll build"
    puts "Done."
  end

  task :serve do
    puts "Serving..."
    sh "bundle exec jekyll serve"
    puts "Done."
  end
end

namespace :blog do
  task :new_post, [:title] do |t, args|
    title = args[:title]

    unless title
      puts "Usage: rake blog:new_post[\"Title of the post\"]"
      exit
    end

    time = Time.now
    filename = "_posts/#{time.strftime("%Y-%m-%d")}-#{title.downcase.gsub(" ", "-")}.markdown"

    File.open(filename, "w") do |file|
      file.write("---\n")
      file.write("layout: post\n")
      file.write("title: \"#{title}\"\n")
      file.write("date: #{time}\n")
      file.write("categories:\n")
      file.write("tags:\n")
      file.write("keywords:\n")
      file.write("---\n")
      file.write("\n")
    end

    puts "Created new post: #{filename}"
  end
end
