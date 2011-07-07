require 'fileutils'

desc "Build documentation"
task :doc do
end

desc "Fetch/update external repositories"
task :repositories do
  %w{sosol xsugar navigator mapping}.each do |repository|
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
