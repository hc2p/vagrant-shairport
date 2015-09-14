$script = <<SCRIPT
echo I am provisioning...
apt-get install -yq git wget autoconf libtool libdaemon-dev libasound2-dev libpopt-dev libconfig-dev avahi-daemon libavahi-client-dev libssl-dev libsoxr-dev alsa-utils
#also do https://wiki.ubuntuusers.de/Soundkarten_konfigurieren/HDA?redirect=no
sudo echo "options snd-hda-intel model=3stack" >> /etc/modprobe.d/alsa-base.conf
git clone https://github.com/mikebrady/shairport-sync.git
cd shairport-sync
autoreconf -i -f
./configure --with-alsa --with-avahi --with-ssl=openssl --with-metadata --with-soxr --with-systemv
make
sudo make install
sudo update-rc.d shairport-sync defaults 90 10
SCRIPT


unless Vagrant.has_plugin?('vagrant-vbguest')
  puts 'Installing vagrant-vbguest...'
  system 'vagrant plugin install vagrant-vbguest'
end


Vagrant.configure(2) do |config|
  config.vm.box = 'ubuntu/trusty64'
  config.vm.network 'public_network', ip: '192.168.66.100'

  # enable the sound card on the vm
  config.vm.provider :virtualbox do |vb|
    vb.customize [
      "modifyvm", :id,
      "--audio",           "coreaudio",
      "--audiocontroller", "hda"
    ]
  end
  config.vm.provision "shell", inline: $script
end
