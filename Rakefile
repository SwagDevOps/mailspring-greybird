# frozen_string_literal: true

require 'open3'
require 'pathname'
require 'pp'
require 'yaml'

def packages_path
  Pathname.new(Dir.home).join('.config', 'Mailspring', 'packages')
end

# install ------------------------------------------------------------

desc 'Install theme'
task :install, [:path] => [:uninstall] do |task, args|
  cp_r('src', packages_path.join('greybird'))
end

# uninstall ----------------------------------------------------------

desc 'Uninstall theme'
task :uninstall, [:path] do |task, args|
  rm_rf(packages_path.join('greybird'))
end
