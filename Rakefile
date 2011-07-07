require 'fileutils'

repositories = {
  "sosol" => { :commands => ["rake doc:app"],
    :directories => [{
      :description => 'SoSOL Rails app documentation',
      :target => 'app',
      :source => 'doc/app'
    }]
  },
  "xsugar" => { :commands => ["cd src/standalone && mvn javadoc:javadoc && cd ../.."],
    :directories => [{
      :description => 'Standalone XSugar server documentation',
      :target => 'standalone',
      :source => 'src/standalone/target/site/apidocs'
    }]
  },
  "navigator" => {},
  "mapping" => {}
}

namespace "docs" do
  desc "Build documentation"
  task :generate => [:repositories] do
    puts "Generating documentation..."
    repositories.each do |repository, options|
      target = File.join('repositories',repository)
      if options.has_key?(:commands)
        cwd = FileUtils.pwd
        FileUtils.cd(target, :verbose => true)
        options[:commands].each do |command|
          system(command)
        end
        FileUtils.cd(cwd)
      end
    end
  end

  desc "Copy documentation into static-servable directory"
  task :update => [:generate] do
    puts "Updating documentation..."
    repositories.each do |repository, options|
      if options.has_key?(:directories)
        options[:directories].each do |directory|
          target = File.join('generated',repository,directory[:target])
          source = File.join('repositories',repository,directory[:source],'.')
          FileUtils.mkdir_p target
          FileUtils.cp_r source, target
        end
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

task :default => ['docs:update']
