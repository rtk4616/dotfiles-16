namespace :clean do
  desc "Remove .DS_Store files from Dropbox"
  task :ds_store do
    sh "find ~/Dropbox -name .DS_Store -print0 | xargs -0 rm -v"
  end

  task all: %i[ ds_store ]
end

namespace :sync do
  desc "Sync David Lanham wallpapers (assumes the unzipped updates are in ~/Downloads)"
  task :dlanham do
    rm_rf File.expand_path("~/Dropbox/wallpapers/dlanham wallpapers/Newest Additions")
    FileList[File.expand_path("~/Downloads/dlanham*")].each do |dir|
      cmd = %w[ rsync -avz --exclude *.DS_Store . ]
      cmd << File.expand_path(dir.pathmap("~/Dropbox/wallpapers/%f"))
      Dir.chdir dir do
        sh *cmd
      end
      rm_r dir
    end
  end

  desc "Sync submodules"
  task :submodules do
    sh "git submodule foreach git pull"
    Dir.chdir "src/prezto" do
      sh "git fetch upstream"
      sh "git rebase upstream/master"
      sh "git submodule update --init --recursive"
    end
  end

  desc "Sync puzzles from ~/Downloads"
  task :puzzles do
    Dir[File.expand_path("~/Downloads/*")].each do |file|
      dir = case file.pathmap("%f")
            when /^diagramless\d+.pdf$/i
              "diagramless"
            when /^201\dW(?:eek)?\d.*/
              "GM"
            end
      next if dir.nil?

      dir = File.expand_path(File.join("~/Dropbox/Shared/Puzzles", dir))
      mv file, dir
    end
  end

  desc "Sync a config"
  task :config, [:config] do |t, args|
    config = File.expand_path(args[:config])
    to = config.sub(Dir.home, '\1/.dotfiles')

    mv config, to
    ln_s to, config
  end
end

namespace :bitbar do
  FILE = 'onebusaway.30s.rb'

  desc 'Toggle OneBusAway BitBar plugin'
  task :oba do
    if File.exist?(File.expand_path("../bitbar/enabled/#{FILE}", __FILE__))
      rm File.expand_path("../bitbar/enabled/#{FILE}", __FILE__)
    else
      ln_s File.expand_path("../bitbar/#{FILE}", __FILE__),
           File.expand_path("../bitbar/enabled/#{FILE}", __FILE__)
    end
    sh 'open bitbar://refreshPlugin?name=onebusaway.*'
  end
end
