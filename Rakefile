# frozen_string_literal: true

require 'pp'
require 'open3'
require 'pathname'
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
  rm_r(packages_path.join('greybird'))
end

task default: [:install]
