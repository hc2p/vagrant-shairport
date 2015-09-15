#AirPlay on Mac

The wonderful AirPlay emulator [shairport-sync](https://github.com/mikebrady/shairport-sync) only runs on linux. To be able to run it on a mac for instance, you need to grab a virtual machine. If that bores you, and you are working with [Vagrant](https://www.vagrantup.com/) anyway a lot, you will agree with me, that this is a perfect fit.

##Install

```
$ git clone https://github.com/hc2p/vagrant-shairport.git
$ cd vagrant-shairport
$ vagrant up
```

follow the [config section of shairport-sync docs](https://github.com/mikebrady/shairport-sync#configuring-shairport-sync) to see how to configure details like airplay-name and soundcard

##resources
- https://www.vagrantup.com/
- https://github.com/mikebrady/shairport-sync
