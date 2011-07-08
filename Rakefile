require 'fileutils'

repositories = {
  "sosol" => [{
      :description => 'SoSOL Rails app documentation',
      :command => 'rake doc:app',
      :working_directory => '.',
      :target => 'app',
      :source => 'doc/app'
    }
  ],
  "xsugar" => [{
      :description => 'Standalone XSugar server documentation',
      :command => 'mvn javadoc:javadoc',
      :working_directory => 'src/standalone',
      :target => 'standalone',
      :source => 'src/standalone/target/site/apidocs'
    }
  ],
  "navigator" => [{
      :description => 'pn-dispatcher documentation',
      :command => 'mvn javadoc:javadoc',
      :working_directory => 'pn-dispatcher',
      :target => 'pn-dispatcher',
      :source => 'pn-dispatcher/target/site/apidocs'
    },
    {
      :description => 'pn-sync documentation',
      :command => 'mvn javadoc:javadoc',
      :working_directory => 'pn-sync',
      :target => 'pn-sync',
      :source => 'pn-sync/target/site/apidocs'
    }
  ],
  "mapping" => []
}

namespace "docs" do
  desc "Build documentation"
  task :generate => [:repositories] do
    puts "Generating documentation..."
    repositories.each do |repository, directories|
      directories.each do |directory|
        cwd = FileUtils.pwd
        working_directory = File.join('repositories',repository,directory[:working_directory])
        source_directory = File.join('repositories',repository,directory[:source])
        source_files = Dir.glob(File.join(working_directory,'**','*')).reject{|f| f =~ /^#{source_directory}/}
        file source_directory => source_files do
          puts "Generating #{directory[:description]}..."
          FileUtils.cd(working_directory, :verbose => true)
          system(directory[:command])
          FileUtils.cd(cwd)
        end
        Rake::Task[source_directory].invoke
      end
    end
  end

  desc "Copy documentation into static-servable directory"
  task :update => [:generate] do
    puts "Updating documentation..."
    repositories.each do |repository, directories|
      directories.each do |directory|
        target = File.join('generated',repository,directory[:target])
        source = File.join('repositories',repository,directory[:source],'.')
        FileUtils.mkdir_p target
        FileUtils.cp_r source, target
      end
    end
  end
end

desc "Fetch/update external repositories"
task :repositories do
  puts "Updating repositories..."
  repositories.each_key do |repository|
    target = File.join('repositories',repository)
    if File.directory?(target)
      cwd = FileUtils.pwd
      FileUtils.cd(target, :verbose => true)
      system("git pull")
      FileUtils.cd(cwd)
    else
      system("git clone git://github.com/papyri/#{repository}.git #{target}")
    end
  end
end

namespace "jekyll" do
  desc "Generate/update static site (without local server)"
  task :static => ['docs:update'] do
    system("jekyll --pygments --no-auto")
  end

  desc "Generate site and run local server"
  task :run => ['docs:update'] do
    exec("jekyll --pygments --server")
  end
end

task :default => ['docs:update']
