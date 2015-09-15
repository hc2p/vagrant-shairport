$script = <<SCRIPT
echo I am provisioning...

#make sure the vagrant user is in the audio group
sudo usermod -a -G audio vagrant

#install the newest alsa kernel modules
sudo apt-add-repository ppa:ubuntu-audio-dev/alsa-daily
sudo apt-get update
sudo apt-get install oem-audio-hda-daily-dkms

#reload sound module
sudo modprobe snd-hda-intel

apt-get install -yq git wget autoconf libtool libdaemon-dev libasound2-dev libpopt-dev libconfig-dev avahi-daemon libavahi-client-dev libssl-dev libsoxr-dev alsa-utils

#also do https://wiki.ubuntuusers.de/Soundkarten_konfigurieren/HDA?redirect=no
sudo apt-get -yq remove --purge alsa-base pulseaudio
sudo apt-get -yq install alsa-base pulseaudio
sudo alsa force-reload
echo "options snd-hda-intel model=3stack" | sudo tee -a /etc/modprobe.d/alsa-base.conf

#install shairport-sync
git clone https://github.com/mikebrady/shairport-sync.git
cd shairport-sync
autoreconf -i -f
./configure --with-alsa --with-avahi --with-ssl=openssl --with-metadata --with-soxr --with-systemv
make
sudo make install
sudo update-rc.d shairport-sync defaults 90 10
sudo reboot
SCRIPT


unless Vagrant.has_plugin?('vagrant-vbguest')
  puts 'Installing vagrant-vbguest...'
  system 'vagrant plugin install vagrant-vbguest'
end


Vagrant.configure(2) do |config|
  config.vm.box = 'ubuntu/trusty64'
  config.vm.network 'public_network', ip: '192.168.77.100', bridge: 'en0: Wi-Fi (AirPort)'

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
