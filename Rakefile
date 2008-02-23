require 'rake'
require 'rake/testtask'
require 'rake/clean'
require 'rake/gempackagetask'
require 'rake/rdoctask'
require 'rake/contrib/rubyforgepublisher'
require 'fileutils'
include FileUtils

CLEAN.include [ "pkg", "lib/*.bundle", "*.gem", ".config", "**/*.log" ]

desc "Build package"
task :default => [:package]

version = "1.1.1"
name = "mongrel_proctitle"

spec =
  Gem::Specification.new do |s|
    s.name = name
    s.version = version
    s.platform = Gem::Platform::RUBY
    s.summary = "The mongrel_proctitle GemPlugin"
    s.description = s.summary
    s.author = "Ryan Tomayko"
    s.email = "rtomayko@gmail.com"
    s.homepage = "http://github.com/rtomayko/mongrel_proctitle"
    s.add_dependency('mongrel', '>= 1.1')
    s.add_dependency('gem_plugin', '>= 0.2.3')
    s.has_rdoc = true
    s.extra_rdoc_files = [ "README" ]
    s.files = %w(LICENSE README Rakefile) +
      Dir.glob("{bin,doc/rdoc,test,lib}/**/*")
    s.require_path = "lib"
    s.rubyforge_project = "orphans"
  end

Rake::GemPackageTask.new(spec) do |p|
  p.gem_spec = spec
  p.need_zip = true
  p.need_tar_gz = true
end

task :install => [:package] do
  sh %{gem install pkg/#{name}-#{version}.gem}
end

task :uninstall => [:clean] do
  sh %{gem uninstall #{name}}
end

task :release => [:package] do
  sh <<-end
    ssh tomayko.com 'mkdir -p /dist/#{name}' && \
    rsync -azP pkg/#{name}-#{version}.* tomayko.com:/dist/#{name}/
  end
end
