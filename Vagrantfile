# SMcCWishList
#
# Separate Web and database servers serving up static/dynamic sites via Puppet.
#
domain = 'smccwishlist.com'

nodes = [
  { :hostname => 'web', :ip => '192.168.0.42', :box => 'precise32' },
  { :hostname => 'db',  :ip => '192.168.0.43', :box => 'precise32', :ram => 512 }
]

Vagrant::Config.run do |config|
  nodes.each do |node|
    config.vm.define node[:hostname] do |node_config|
      node_config.vm.box = node[:box]
      node_config.vm.host_name = node[:hostname] + '.' + domain
      node_config.vm.network :hostonly, node[:ip]
      node_config.vm.forward_port 80, 9000

      memory = node[:ram] ? node[:ram] : 256;
      node_config.vm.customize [
        'modifyvm', :id,
        '--name', node[:hostname],
        '--memory', memory.to_s
      ]
    end
  end

  config.vm.provision :puppet do |puppet|
    puppet.manifests_path = 'puppet/manifests'
    puppet.manifest_file = 'site.pp'
    puppet.module_path = 'puppet/modules'
  end
end
