require 'nanoc3/tasks'

# Configuration:
#

OPENSHIFT_REPO = 'ssh://e6462b5a8bfc469a86de55e6513537d6@nanoc-mfojtik.rhcloud.com/~/git/nanoc.git/'
BASE_URL = "http://nanoc-mfojtik.rhcloud.com"

desc 'Deploy the website to OpenShift using Git.'
task :deploy do
  prepare!
  compile!
  deploy!
  revert!
end

# Based on https://github.com/meskyanichi/nanoc-heroku

##
# Use this method to change the base url in the configuration file
# so you can automate that instead of manually changing it everytime
# when you want to deploy the website
#
def change_base_url_to(url)
  puts "Changing Base URL to #{url}.."
  config = YAML.load_file('./config.yaml')
  config['base_url'] = url
  File.open('./config.yaml', 'w') do |file|
    file.write(config.to_yaml)
  end
end

##
# Re-compile by removing the output directory and re-compiling
#
def compile!
  puts "Compiling website.."
  puts %x[rm -rf output]
  puts %x[nanoc compile]
end

##
# Prepares the deployment environment
#
def prepare!
  %x[git checkout master]
  unless %x[git status] =~ /nothing to commit \(working directory clean\)/
    puts "Please commit your changes on the master branch before deploying!"
    exit 1
  end

  puts "Creating and moving in to \"deployment\" branch.."
  puts %x[git checkout -b deployment]

  puts "Removing \"output\" directory from .gitignore.."
  begin
    gitignore = File.read(".gitignore")
    File.open(".gitignore", "w") do |file|
      file.write(gitignore.gsub("output", ""))
    end

    change_base_url_to(BASE_URL)
  rescue => e
    puts "ERROR: #{e.message}"
    puts "Reverting back to master..."
    puts %x[git checkout master]
    puts %x[git branch -D deployment]
    puts %x[git checkout .]
  end
end

##
# Moves back to the "master" branch and removes the "deployment" branch
#
def revert!
  %x[rm -rf public]
  %x[git checkout master]
  %x[git branch -D deployment]
end

def deploy!
  puts "Running bundlers..."
  puts %x[bundle --path vendor/bundle]
  puts "Adding and committing compiled output for deployment.."
  puts %x[mv output public]
  puts %x[git add .]
  puts %x[git commit -a -m "temporary commit for deployment"]
  puts 'Deploying to OpenShift..'
  puts %x[git remote add openshift #{OPENSHIFT_REPO}]
  puts %x[git push openshift HEAD:master --force]
end
