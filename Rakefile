#!/usr/bin/env ruby
# -*- ruby -*-

require 'rake/clean'
require 'yaml'
require 'json'

DATA_DIR  = '_data'
API_DIR = 'v1'

DATA_FILES = FileList["#{DATA_DIR}/*"]

module API 
  # Generate an API endpoint from YAML data file
  def self.generate(source, destination)
    data = YAML.load_file(source)
    File.write(destination, data.to_json)
  end
end

task :default => :build

task :build do
  DATA_FILES.each do |file|
    api_name = File.basename(file, '.yml')
    destination = "#{API_DIR}/#{api_name}.json"
    API.generate(file, destination)
  end
end

task :clean do
  sh "rm #{API_DIR}/*"
end

task :publish do
  # TODO(Avi Itskovich): Ensure that each step succeeded
  sh 'git checkout gh-pages'
  rake :clean
  rake :build  
  sh 'git commit -am "Publish API'
  sh 'git push origin gh-pages'
end
