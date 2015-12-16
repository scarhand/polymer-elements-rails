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
  # loop through all the html files in the subdirs
  Dir['app/assets/components/**/*.html'].each do |dir|
    # strip the dir from the name
    file_name = dir.gsub(/app\/assets\/components\//, '')
    # split the file name in the part before and the parts after the first /
    parts = file_name.split('/', 2)
    # do we have a .rake_ignore file? If so, ignore this dir
    unless File.exists? "app/assets/components/#{parts.first}/.rake_ignore"
      # construct the raw github uri
      remote = "https://raw.githubusercontent.com/PolymerElements/#{parts.first}/master/#{parts.last}"
      begin
        contents = open(remote).read
        # did we get any content for the file?
        if contents
          # we apparentily did, so write it to the file
          File.open("app/assets/components/#{file_name}", 'wb') do |fo|
            fo.write contents
          end
        end
      rescue => e
        puts "File not found: #{remote}"
      end
    end
  end
end

task :update_imports do
  # loop through all the html files in the subdirs
  Dir['app/assets/components/**/*.html'].each do |dir|
    # strip the dir from the name
    file_name = dir.gsub(/app\/assets\/components\//, '')
    # split in parts before and after the last /
    parts = file_name.rpartition '/'
    # open the file for reading
    File.open("app/assets/components/#{file_name}", 'r') do |fo|
      # scan the file for import declarations
      imports = fo.read.scan(/^<link rel="import" href="(.+)">/).flatten
      # ignore the polymer import
      imports.reject{ |n| n.end_with? '/polymer/polymer.html' }.each do |import|
        # if the file does not exist we need to do something
        unless File.exists? "app/assets/components/#{parts.first}/#{import}"
          # files from other directories/repositories are a special case
          # fix this yourself :)
          unless import.start_with? '../'
            # construct the raw github uri
            remote = "https://raw.githubusercontent.com/PolymerElements/#{parts.first}/master/#{import}"
            puts "Trying to download: #{remote}"
            contents = open(remote).read
            # did we get any content for the new file?
            if contents
              # we apparentily did, so write it to the file
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
