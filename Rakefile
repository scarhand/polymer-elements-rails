require "bundler/gem_tasks"
require 'open-uri'

task :list do
  Dir['app/assets/components/**/*.html'].each do |dir|
    parts = dir.gsub(/app\/assets\/components\//, '').split('/', 2)
    puts "https://raw.githubusercontent.com/PolymerElements/#{parts.first}/master/#{parts.last}"
    # https://raw.githubusercontent.com/PolymerElements/paper-button/master/paper-button.html
  end
end

task :update do
  Dir['app/assets/components/**/*.html'].each do |dir|
    file_name = dir.gsub(/app\/assets\/components\//, '')
    parts = file_name.split('/', 2)
    remote = "https://raw.githubusercontent.com/PolymerElements/#{parts.first}/master/#{parts.last}"
    begin
      contents = open(remote).read
      if contents
        File.open("app/assets/components/#{file_name}", 'wb') do |fo|
          fo.write contents
        end
      end
    rescue => e
      puts "File not found: #{remote}"
    end
  end
end

task :update_imports do
  Dir['app/assets/components/**/*.html'].each do |dir|
    file_name = dir.gsub(/app\/assets\/components\//, '')
    parts = file_name.rpartition '/'
    File.open("app/assets/components/#{file_name}", 'r') do |fo|
      imports = fo.read.scan(/^<link rel="import" href="(.+)">/).flatten
      imports.reject{ |n| n.end_with? '/polymer/polymer.html' }.each do |import|
        unless File.exists? "app/assets/components/#{parts.first}/#{import}"
          unless import.start_with? '../'
            remote = "https://raw.githubusercontent.com/PolymerElements/#{parts.first}/master/#{import}"
            puts "Trying to download: #{remote}"
            contents = open(remote).read
            if contents
              File.open("app/assets/components/#{parts.first}/#{import}", 'wb') do |fo|
                fo.write contents
              end
            end
          else
            puts "!!!!! try to download this yourself: #{parts.first}/#{import}"
          end
        end
      end
    end
  end
end

task :foo do
  file_name = 'paper-dialog/paper-dialog.html'
  parts = file_name.rpartition '/'
  File.open("app/assets/components/#{file_name}", 'r') do |fo|
    imports = fo.read.scan(/<link rel="import" href="(.+)">/).flatten
    puts importsputs ""
    imports.reject{ |n| n.end_with? '/polymer/polymer.html' }.each do |import|
      unless File.exists? "app/assets/components/#{parts.first}/#{import}"
        puts "Does not exist: app/assets/components/#{parts.first}/#{import}"
        puts "\t referenced by: #{file_name}"
      end
    end
  end
end