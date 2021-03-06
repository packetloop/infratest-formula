require 'rspec/core/rake_task'
require 'kitchen'
require 'yamllint/rake_task'

desc 'Run YAML style checks'
YamlLint::RakeTask.new(:yaml) do |t|
  t.paths = %w(
    *.yaml
    *.yml
    .*.yml
    .*.yaml
    infratest/**/*.yml
    infratest/**/*.yaml
  )
end

# Rspec
desc 'Run Rspec examples'
RSpec::Core::RakeTask.new(:spec)

# Integration tests. Kitchen.ci
namespace :integration do
  docker_config = { local_config: '.kitchen-docker.yml' }
  linode_config = { local_config: '.kitchen-ci.yml' }
  
  desc 'Run Test Kitchen with Vagrant'
  task :vagrant do
    Kitchen.logger = Kitchen.default_file_logger
    Kitchen::Config.new.instances.each do |instance|
      instance.test(:always)
    end
  end
  
  desc 'Run Test Kitchen with Docker'
  task :docker do
    config = { loader: Kitchen::Loader::YAML.new(docker_config) }
    Kitchen.logger = Kitchen.default_file_logger
    Kitchen::Config.new(config).instances.each do |instance|
      instance.test(:always)
    end
  end
  
  desc 'Run Test Kitchen with Linode'
  task :linode do
    config = { loader: Kitchen::Loader::YAML.new(linode_config) }
    Kitchen.logger = Kitchen.default_file_logger
    Kitchen::Config.new(config).instances.each do |instance|
      instance.test(:always)
    end
  end
end

desc 'Run all tests on Travis'
task travis: ['yaml', 'spec', 'integration:docker']

# Default
task default: %w(yaml spec)
