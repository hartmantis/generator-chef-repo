require 'rubygems'
require 'bundler/setup'
require 'appraisal'
require 'tailor/rake_task'
require 'cane/rake_task'
require 'rspec/core/rake_task'
require 'foodcritic'

desc 'Run Cane to check code quality metrics'
Cane::RakeTask.new

desc 'Run Tailor to lint check Ruby code'
Tailor::RakeTask.new do |task|
  task.file_set('**/*.rb')
end

desc 'Display LOC stats'
task :loc do
  puts "\n## LOC Stats"
  sh 'countloc -r .'
end

desc 'Run knife cookbook syntax test'
task :cookbook_test do
  puts 'Running cookbook syntax checks...'
  path = File.expand_path('../..', __FILE__)
  cb = File.basename(File.expand_path('..', __FILE__))
  puts %x{knife cookbook test -c test/knife.rb -o #{path} #{cb}}
  $?.exitstatus == 0 or fail 'Cookbook syntax check failed!'
end

desc 'Run Foodcritic lint tests'
FoodCritic::Rake::LintTask.new do |f|
  f.options = { :fail_tags => %w{all} }
end

desc 'Run ChefSpec unit tests'
RSpec::Core::RakeTask.new(:spec)

desc 'Run a full converge test'
task :converge do
  fail 'Convergence tests not yet implemented'
end

task :default => [ :cane, :tailor, :loc, :cookbook_test, :foodcritic, :spec ]

# vim: ai et ts=2 sts=2 sw=2 ft=ruby
