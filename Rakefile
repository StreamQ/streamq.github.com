# Originally adopted from Raimonds Simanovskis Rakefile, 
#   http://github.com/rsim/blog.rayapps.com/blob/master/Rakefile
#   which was in turn adopted from Tate Johnson's Rakefile
#   http://github.com/tatey/tatey.com/blob/master/Rakefile

desc 'Generate tags pages'
task :tags  => :tag_cloud do
  puts "Generating tags..."
  require 'rubygems'
  require 'jekyll'
  include Jekyll::Filters
  
  options = Jekyll.configuration({})
  site = Jekyll::Site.new(options)
  site.read_posts('')

  # Remove tags directory before regenerating
  FileUtils.rm_rf("tags")

  site.tags.sort.each do |tag, posts|
    html = <<-HTML
---
layout: default
title: "artikels met tag: #{tag}"
syntax-highlighting: yes
---
  <h3>artikels met tag: #{tag}</h3>
  {% for ps in site.posts %}
		{% for tag in ps.tags %}
			{%if tag == "#{tag}" %}
			<article class="post">
				<header class="post-header">
					<h3><a class="postlink" href="{{ ps.url }}">{{ ps.title }}</a></h3>
					<span class="post-info">Geplaatst op <time datetime="{{ ps.date }}" pubdate>{{ ps.date | date: "%d/%m/%y" }}</time> door {{ ps.author }}</span>
				</header>
				<p>
				{% if ps.image != null %}
					<img class="post-img" src="/img/posts/{{ ps.image }}" />
				{%endif%}
				{{ps.content}}
				<br /><a href="{{ ps.url }}">Lees het volledige artikel</a>
				</p>
			</article>
			{%endif%}
		{%endfor%}
  {% endfor %}
HTML

    FileUtils.mkdir_p("tags/#{tag}")
    File.open("tags/#{tag}/index.html", 'w+') do |file|
      file.puts html
    end
  end
  puts 'Done.'
end

desc 'Generate tags pages'
task :tag_cloud do
  puts 'Generating tag cloud...'
  require 'rubygems'
  require 'jekyll'
  include Jekyll::Filters

  options = Jekyll.configuration({})
  site = Jekyll::Site.new(options)
  site.read_posts('')

  html = '<h3>tags</h3>'
  max_count = site.tags.map{|t,p| p.count}.max
  site.tags.sort.each do |tag, posts|
    s = posts.count
    font_size = ((20 - 10.0*(max_count-s)/max_count)*2).to_i/2.0
    html << "<a href=\"/tags/#{tag.gsub(/ /,"%20")}\" title=\"Postings tagged #{tag}\" style=\"font-size: #{font_size}px; line-height:#{font_size}px\">#{tag}</a> "
  end
  File.open('_includes/tag_cloud.html', 'w+') do |file|
    file.puts html
  end
  puts 'Done.'
end
