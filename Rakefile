#!/usr/bin/env rake
require 'bundler/gem_tasks'
require 'json'

task :update do
  Dir['*.gem'].each{ |f| FileUtils.rm(f) }

	js_dir  = 'vendor/assets/javascripts'
	css_dir = 'vendor/assets/stylesheets'
	img_dir = 'vendor/assets/images'
	[js_dir, css_dir, img_dir].each do |dir|
		FileUtils.rm_r(dir)
		FileUtils.mkdir(dir)
		FileUtils.touch(File.join(dir, '.gitkeep'))
	end

	puts 'Updating source files...'
	`git submodule update --recursive --remote`

	puts 'Copying source js files...'
  FileUtils.cp('mustache.js/mustache.js', js_dir)
  FileUtils.cp('jquery.mustache.js/jquery.mustache.js', js_dir)

  puts 'Updating version...'
  mustache_js_version = JSON.parse(File.read('mustache.js/package.json')).fetch('version')
  jquery_mustache_js_version = File.read('jquery.mustache.js/jquery.mustache.js').match(/jQuery Mustache - v([\.\d]+)/)[1]
  puts "Current mustache.js version is: #{mustache_js_version}"
  puts "Current jquery.mustache.js version is: #{jquery_mustache_js_version}"
  readme = File.read('README.md')
  readme = readme.gsub(/(?<=<b id="mustache-js-version">)[\d\.]+(?=<\/b>)/, mustache_js_version)
  readme = readme.gsub(/(?<=<b id="jquery-mustache-js-version">)[\d\.]+(?=<\/b>)/, jquery_mustache_js_version)
  File.open('README.md','w') { |f| f.write(readme) }
end

task default: %i[update]

