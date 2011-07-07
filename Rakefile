require 'fileutils'

repositories = {
  "sosol" => { :commands => ["rake doc:app"], :directories => ['doc/app']},
  "xsugar" => { :commands => ["cd src/standalone && mvn javadoc:javadoc && cd ../.."],
                :directories => ['src/standalone/target/site/apidocs']},
  "navigator" => {},
  "mapping" => {}
}

desc "Build documentation"
task :doc => [:repositories] do
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

task :default => [:doc]
