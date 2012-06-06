require 'rake'
require 'rake/testtask'
require 'rake/rdoctask'
require 'rake/packagetask'
require 'rake/gempackagetask'

require File.join(File.dirname(__FILE__), 'lib/rails_sql_views', 'version')

PKG_BUILD       = ENV['PKG_BUILD'] ? '.' + ENV['PKG_BUILD'] : ''
PKG_NAME        = 'rails_sql_views'
PKG_VERSION     = RailsSqlViews::VERSION::STRING + PKG_BUILD
PKG_FILE_NAME   = "#{PKG_NAME}-#{PKG_VERSION}"
PKG_DESTINATION = ENV["PKG_DESTINATION"] || "../#{PKG_NAME}"

RELEASE_NAME  = "REL #{PKG_VERSION}"

RUBY_FORGE_PROJECT = "activewarehouse"
RUBY_FORGE_USER    = "aeden"

desc 'Default: run unit tests.'
task :default => :test

desc 'Test the library.'
Rake::TestTask.new(:test) do |t|
  t.libs << 'lib'
  t.pattern = 'test/**/*_test.rb'
  t.verbose = true
end

namespace :rcov do
  desc 'Measures test coverage'
  task :test do
    rm_f 'coverage.data'
    mkdir 'coverage' unless File.exist?('coverage')
    rcov = "rcov --aggregate coverage.data --text-summary -Ilib"
    system("#{rcov} test/*_test.rb test/**/*_test.rb")
    system("open coverage/index.html") if PLATFORM['darwin']
  end
end

desc 'Generate documentation library.'
Rake::RDocTask.new(:rdoc) do |rdoc|
  rdoc.rdoc_dir = 'rdoc'
  rdoc.title    = 'Rails SQL Views'
  rdoc.options << '--line-numbers' << '--inline-source'
  rdoc.rdoc_files.include('README')
  rdoc.rdoc_files.include('lib/**/*.rb')
end

desc "Publish the API documentation"
task :pdoc => [:rdoc] do 
  Rake::SshDirPublisher.new("aeden@rubyforge.org", "/var/www/gforge-projects/activewarehouse/rails_sql_views/rdoc", "rdoc").upload
end
