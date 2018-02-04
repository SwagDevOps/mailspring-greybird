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
    attr_reader :src_dir

    def initialize(src_dir = 'src')
      @src_dir = Pathname.new(src_dir)
      JSON.parse(manifest.read).each do |k, v|
        self.singleton_class.class_eval do
          attr_accessor k.underscore
          protected "#{k.underscore}="
        end

        self.__send__("#{k.underscore}=", v)
      end
    end

    # @return [Pathname]
    def install_dir
      Pathname.new(Dir.home)
        .join('.config') # SHOULD use XDG specification
        .join('Mailspring', 'packages')
    end

    def install(target_dir = install_dir)
      target_dir = Pathname.new(target_dir)
      mkdir_p(target_dir)
      cp_r(src_dir, target_dir.join(name))

      self
    end

    def uninstall(target_dir = install_dir)
      target_dir = Pathname.new(target_dir)
      rm_rf(target_dir.join(name))

      self
    end

    protected

    # @return [Pathname]
    def manifest
      src_dir.join('package.json').realpath
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
  package.public_send(*[task.name, args[:path]].compact)
end

desc "Uninstall #{package.display_name}"
task :uninstall, [:path] do |task, args|
  package.public_send(*[task.name, args[:path]].compact)
end
