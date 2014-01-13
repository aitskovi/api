#!/usr/bin/env ruby
# -*- ruby -*-

require 'rake/clean'
require 'yaml'
require 'json'

DATA_DIR  = '_data'
API_DIR = 'v1'

DATA_FILES = FileList["#{DATA_DIR}/*"]

# TODO(Avi Itskovich): Verify rakefile was run from root

module API 
  # Generate an API endpoint from YAML data file
  def self.generate(source, destination)
    data = YAML.load_file(source)
    File.write(destination, data.to_json)
  end
end

module GIT
  def self.reset
    sh 'git reset --hard'
    sh 'git checkout master'
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
  sh 'git checkout gh-pages'
  sh 'git fetch'
  sh 'git rebase origin/master'

  Rake::Task["clean"].invoke
  Rake::Task["build"].invoke

  sh "git add #{API_DIR}" do |success, res|
    # Attempt to git add, if it doesn't work fall back.
    if !ok
      GIT.reset
      fail "Git add failed on publish with result: #{res}"
    end
  end

  sh 'git commit -m "API Update"'
  sh 'git push -f origin gh-pages'
  sh 'git checkout master'
end
