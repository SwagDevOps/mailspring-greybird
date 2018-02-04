# frozen_string_literal: true

require 'json'
require 'open3'
require 'pathname'
require 'pp'
require 'rake'
require 'yaml'

module Mailspring
  class Package
    include Rake::FileUtilsExt

    def initialize
      JSON.parse(manifest.read).each do |k, v|
        self.singleton_class.class_eval do
          attr_accessor k.underscore
          protected "#{k.underscore}="
        end

        self.__send__("#{k.underscore}=", v)
      end
    end

    # @return [Pathname]
    def install_path
      Pathname.new(Dir.home)
        .join('.config') # SHOULD use XDG specification
        .join('Mailspring', 'packages', name)
    end

    def install(origin_dir, target_path = install_path)
      mkdir_p(target_path)
      cp_r(origin_dir, target_path)

      self
    end

    def uninstall(target_path = install_path)
      rm_rf(target_path)

      self
    end

    # @return [Pathname]
    def manifest
      Pathname.new(Dir.pwd).join('src', 'package.json').realpath
    end
  end
end

class String
  def underscore!
    gsub!(/(.)([A-Z])/,'\1_\2')
    downcase!
  end

  def underscore
    dup.tap { |s| s.underscore! }
  end
end

# tasks --------------------------------------------------------------

package = Mailspring::Package.new

desc "Install #{package.display_name}"
task :install, [:path] => [:uninstall] do |task, args|
  package.public_send(*[task.name, 'src', args[:path]].compact)
end

desc "Uninstall #{package.display_name}"
task :uninstall, [:path] do |task, args|
  package.public_send(*[task.name, args[:path]].compact)
end
