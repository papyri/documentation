require 'fileutils'
require 'yaml'

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
      :command => 'mvn -Dadditionalparam="-notimestamp" javadoc:javadoc',
      :working_directory => 'src/standalone',
      :target => 'standalone',
      :source => 'src/standalone/target/site/apidocs'
    },
    {
      :description => 'RXSugar library documentation',
      :command => 'rake doc',
      :working_directory => '.',
      :target => 'rxsugar',
      :source => 'doc/html'
    }
  ],
  "navigator" => [{
      :description => 'pn-dispatcher documentation',
      :command => 'mvn -Dadditionalparam="-notimestamp" javadoc:javadoc',
      :working_directory => 'pn-dispatcher',
      :target => 'pn-dispatcher',
      :source => 'pn-dispatcher/target/site/apidocs'
    },
    {
      :description => 'pn-sync documentation',
      :command => 'mvn -Dadditionalparam="-notimestamp" javadoc:javadoc',
      :working_directory => 'pn-sync',
      :target => 'pn-sync',
      :source => 'pn-sync/target/site/apidocs'
    },
      {
      :description => 'pn-indexer documentation',
      :command => 'cp -v docs/uberdoc.html docs/index.html',
      :working_directory => 'pn-indexer',
      :target => 'pn-indexer',
      :source => 'pn-indexer/docs'
    },
    {
      :description => 'pn-mapping documentation',
      :command => 'cp -v docs/uberdoc.html docs/index.html',
      :working_directory => 'pn-mapping',
      :target => 'pn-mapping',
      :source => 'pn-mapping/docs'
    }
  ]
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
        FileUtils.mkdir_p(source_directory, :verbose => true)
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

  desc "Clean/remove generated documentation directories"
  task :clean do
    puts "Cleaning documentation directories..."
    repositories.each do |repository, directories|
      directories.each do |directory|
        source_directory = File.join('repositories',repository,directory[:source])
        FileUtils.rm_rf(source_directory, :verbose => true)
      end
    end
  end

  desc "Build index of generated documentation"
  task :generated_index do
    index_file = File.join('_includes','generated_index.md')
    file index_file => 'Rakefile' do
      puts "Building generated documentation index..."
      index_content = ""
      repositories.each do |repository, directories|
        directories.each do |directory|
          index_content += "* [#{directory[:description]}](#{File.join('generated',repository,directory[:target])})\n"
        end
      end
      FileUtils.mkdir_p('_includes')
      File.open(index_file,'w') {|f| f.write(index_content)}
    end
    Rake::Task[index_file].invoke
  end
  
  desc "Build index of system-level documentation"
  task :system_level_index do
    index_file = File.join('_includes','system_level_index.md')
    file index_file => 'Rakefile' do
      puts "Building system-level documentation index..."
      index_content = ""
      Dir.glob(File.join('system_level','**','*.md')).each do |md_file|
        title = YAML.load_file(md_file)['title']
        base_name = md_file.sub(/\.md$/,'')
        index_content += "* [#{title}](#{base_name}.html)\n"
      end
      FileUtils.mkdir_p('_includes')
      File.open(index_file,'w') {|f| f.write(index_content)}
    end
    Rake::Task[index_file].invoke
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
  task :static => ['docs:update','docs:generated_index','docs:system_level_index'] do
    system("jekyll build")
  end

  desc "Generate site and run local server"
  task :run => ['docs:update','docs:generated_index','docs:system_level_index'] do
    exec("jekyll server")
  end
end

task :default => ['docs:update']
