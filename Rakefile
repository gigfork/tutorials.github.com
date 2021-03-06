require 'rubygems'
require 'rake'

task :default => :update_toc

desc "Regenerate the table of contents to include links to newest tutorials"
task :update_toc do
  require 'jekyll'
  require 'tempfile'
  require 'fileutils'

  site = Jekyll::Site.new(Jekyll.configuration({})) and site.read

  pages = site.pages.reject do |page|
    page.data["ignore"]
  end.sort_by! do |page|
    File.mtime(File.join(FileUtils.pwd, "pages", page.name)).to_i
  end.reverse

  toc_path = File.join(FileUtils.pwd, "pages", "table-of-contents.md")
  temp = Tempfile.new("table_of_contents.md")

  begin
    File.readlines(toc_path).each do |line|
      line =~ /^<!--- BEGIN TOC -->$/ ? break : temp.puts(line)
    end
    temp.puts "<!--- BEGIN TOC -->"
    pages.each_with_index do |page, index|
      temp.puts "* [#{page.data["title"]}](#{page.destination('')}?ts=#{File.mtime(File.join(FileUtils.pwd, "pages", page.name)).to_i})"
    end
    temp.puts "<!--- END TOC -->"
    FileUtils.mv(temp.path, toc_path)
  ensure
    temp.delete
  end
end
