Vagrant::Config.run do |config|
  config.vm.box = 'base'
  config.vm.customize do |vm|
    vm.memory_size = 1024
  end

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ['cookbooks', 'more_cookbooks']
    chef.log_level = :debug
    chef.add_recipe 'apt'
    chef.add_recipe 'git'
    chef.add_recipe 'monit'

    chef.json.merge!({
                       :monit => {
                         :start_delay => 15,
                         :interval => 30,
                         :web => {
                           :enabled => true
                         }}
                     })

    #this prints out the vagrant config to a `dna.json` file that we
    #can upload to an EC2 instance and use with `chef-solo`.
    require 'json'
    open('dna.json', 'w'){|f| f.write chef.json.to_json}
    open('.cookbooks_path.json', 'w'){|f| f.puts JSON.generate chef.cookbooks_path }
  end
end
