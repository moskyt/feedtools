require 'rubygems'
require 'rake'
require 'rake/testtask'
require 'rake/rdoctask'
require 'rake/packagetask'
require 'rake/gempackagetask'
require 'rake/contrib/rubyforgepublisher'

require File.join(File.dirname(__FILE__), 'lib/feed_tools', 'version')

desc "Default Task"
task :default => [ :test_all ]

# Run the unit tests

Rake::TestTask.new("test_all") { |t|
  t.libs << "test"
  t.test_files = FileList['test/unit/*_test.rb']
  t.verbose = true
}
Rake::TestTask.new("amp_test") { |t|
  t.libs << "test"
  t.test_files = FileList['test/unit/amp_test.rb']
  t.verbose = true
}
Rake::TestTask.new("atom_test") { |t|
  t.libs << "test"
  t.test_files = FileList['test/unit/atom_test.rb']
  t.verbose = true
}
Rake::TestTask.new("cache_test") { |t|
  t.libs << "test"
  t.test_files = FileList['test/unit/cache_test.rb']
  t.verbose = true
}
Rake::TestTask.new("cdf_test") { |t|
  t.libs << "test"
  t.test_files = FileList['test/unit/cdf_test.rb']
  t.verbose = true
}
Rake::TestTask.new("encoding_test") { |t|
  t.libs << "test"
  t.test_files = FileList['test/unit/encoding_test.rb']
  t.verbose = true
}
Rake::TestTask.new("generation_test") { |t|
  t.libs << "test"
  t.test_files = FileList['test/unit/generation_test.rb']
  t.verbose = true
}
Rake::TestTask.new("helper_test") { |t|
  t.libs << "test"
  t.test_files = FileList['test/unit/helper_test.rb']
  t.verbose = true
}
Rake::TestTask.new("interface_test") { |t|
  t.libs << "test"
  t.test_files = FileList['test/unit/interface_test.rb']
  t.verbose = true
}
Rake::TestTask.new("nonstandard_test") { |t|
  t.libs << "test"
  t.test_files = FileList['test/unit/nonstandard_test.rb']
  t.verbose = true
}
Rake::TestTask.new("rdf_test") { |t|
  t.libs << "test"
  t.test_files = FileList['test/unit/rdf_test.rb']
  t.verbose = true
}
Rake::TestTask.new("rss_test") { |t|
  t.libs << "test"
  t.test_files = FileList['test/unit/rss_test.rb']
  t.verbose = true
}

# Generate the RDoc documentation

Rake::RDocTask.new { |rdoc|
  rdoc.rdoc_dir = 'doc'
  rdoc.title    = "Feed Tools -- caching system for xml news feeds"
  rdoc.options << '--line-numbers' << '--inline-source' <<
    '--accessor' << 'cattr_accessor=object'
  rdoc.template = "#{ENV['template']}.rb" if ENV['template']
  rdoc.rdoc_files.include('README', 'CHANGELOG')
  rdoc.rdoc_files.include('lib/**/*.rb')
  rdoc.rdoc_files.include('db/**/*.sql')
}

begin
  require 'jeweler'
  Jeweler::Tasks.new do |gem|
    gem.name = 'grosser-feedtools'
    gem.version = FeedTools::FEED_TOOLS_VERSION::STRING

    gem.authors = ["Bob Aman"]
    gem.email = "bob@sporkmonger.com"
    gem.homepage = "http://github.com/grosser/feedtools"
    gem.summary = "Parsing, generation, and caching system for xml news feeds."
    gem.description = "Implements a simple system for handling xml news feeds with caching."

    gem.post_install_message = <<-TEXT

      FeedTool's caching schema has changed to allow Feed objects to be
      serialized to the cache.  This should offer some limited speed up
      in some cases.

    TEXT

    gem.has_rdoc = true
    gem.extra_rdoc_files = %w( README.md )
    gem.rdoc_options.concat ['--main',  'README.md']
  end

  Jeweler::GemcutterTasks.new
rescue LoadError
  puts "Jeweler, or one of its dependencies, is not available. Install it with: gem install jeweler"
end

task :profile do
  $:.unshift(File.dirname(__FILE__) + '/lib')
  require 'feed_tools'
  puts "Profiling FeedTools #{FeedTools::FEED_TOOLS_VERSION::STRING}..."
  command = 'ruby -rprofile -e \'' +
    '$:.unshift(File.dirname(__FILE__) + "/lib");' +
    'require "feed_tools";' +
    'FeedTools::Feed.open(' +
      '"http://hsivonen.iki.fi/test/unknown-namespace.atom")' +
        '.build_xml("atom", 1.0)\''
  `#{command}`
end

task :lines do
  lines, codelines, total_lines, total_codelines = 0, 0, 0, 0

  for file_name in FileList["lib/**/*.rb"]
    f = File.open(file_name)

    while line = f.gets
      lines += 1
      next if line =~ /^\s*$/
      next if line =~ /^\s*#/
      codelines += 1
    end
    puts "L: #{sprintf("%4d", lines)}, LOC #{sprintf("%4d", codelines)} | #{file_name}"
    
    total_lines     += lines
    total_codelines += codelines
    
    lines, codelines = 0, 0
  end

  puts "Total: Lines #{total_lines}, LOC #{total_codelines}"
end


# Publishing ------------------------------------------------------

namespace :publish do
  desc "Publish the API documentation"
  task :api => [ "rdoc" ] do 
    Rake::SshDirPublisher.new(
      "#{RUBY_FORGE_USER}@rubyforge.org",
      "#{RUBY_FORGE_PATH}/",
      "doc"
    ).upload
  end

  desc "Runs all of the publishing tasks"
  task :all => ["publish:api"] do
  end
end
