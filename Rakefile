require "bundler/gem_tasks"
require 'rake/testtask'
require 'rake/clean'

NAME = 'iconv'

# rule to build the extension: this says
# that the extension should be rebuilt
# after any change to the files in ext
file "lib/#{NAME}/#{NAME}.#{RbConfig::CONFIG['DLEXT']}" =>
    Dir.glob("ext/#{NAME}/*{.rb,.c}") do
  Dir.chdir("ext/#{NAME}") do
    # this does essentially the same thing
    # as what RubyGems does
    ruby "extconf.rb", *ARGV.grep(/\A--/)
    sh "make", *ARGV.grep(/\A(?!--)/)
  end
  cp "ext/#{NAME}/#{NAME}.#{RbConfig::CONFIG['DLEXT']}", "lib/#{NAME}"
end

# make the :test task depend on the shared
# object, so it will be built automatically
# before running the tests
task :test => "lib/#{NAME}/#{NAME}.#{RbConfig::CONFIG['DLEXT']}"

# use 'rake clean' and 'rake clobber' to
# easily delete generated files
CLEAN.include("ext/**/*{.o,.log,.#{RbConfig::CONFIG['DLEXT']}}")
CLEAN.include("ext/**/Makefile")
CLOBBER.include("lib/**/*.#{RbConfig::CONFIG['DLEXT']}")

# the same as before
Rake::TestTask.new do |t|
  t.libs << 'test'
end

desc "Run tests"
task :default => :test
