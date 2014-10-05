vagrant-devel-full
=============

This github repository is used by a tool called [vagrant-git](https://github.com/pussinboots/vagrant-git). That start vagrant boxes for a certain github project. It is defined in the [.vgit.yaml](https://github.com/pussinboots/vagrant-devel-full/blob/master/.vgit.yml) file.

##base box upload [finished](https://vagrantcloud.com/pussinboots/ubuntu-truly-full)

Setup a ready to use vagrant box for Scala and nodejs with Ubuntu 14.04 base box. The base box only contains the 
Ubuntu 14.04 truly version with all listed development packages see below section Installed software. The vagrant base box is upload to [vagrantcloud.com](https://vagrantcloud.com/).

##Base Box

The base box is uploaded to google drive and can be downloaded by [vagrantcloud](https://vagrantcloud.com/pussinboots/ubuntu-truly-full). It is refrenced in the Vagrantfile
```ruby
config.vm.box = "pussinboots/ubuntu-truly-full"
```
that tells vagrant to download it from vagrantcloud by using the url mentioned above. The download can take a while the file is 3 GB big. 

A tutorial to create a vagrant base box look [here](https://docs.vagrantup.com/v2/boxes/base.html).

##Motivation

Easy to setup Ubuntu based development environment. The complete new setup of development machine cause me in the past some headaches
because i couldn't rember how to setup some specific development tools like sbt or so. And accidently i removed sometimes my complete adapted development vm. So i decided to automate this setup completly so i will have problem anymore to setup a clean full development environment that contains all tools i loved and i need. Feel free to fork this repository and adapt it to your own needs maybe it is possible to setup a page like vagrantcloud did where every developer can upload his development environment based on vagrant.

##Requirements

* installed [vagrant](http://www.vagrantup.com/downloads.html) > 1.5
* installed [vagrant-vbguest](https://github.com/dotless-de/vagrant-vbguest) ```vagrant plugin install vagrant-vbguest```
* installed [virtualbox](https://www.virtualbox.org/wiki/Downloads)

##Vagrant

* setup with the ssh key used by vagrant 
* no chef or puppet installed
# no provisioner used

###User

* username: vagrant
* password: vagrant
* has sudo rights

###Operating System

Ubuntu 14.04 with gnome-session-flashback (2d) window manager because the default unity needs 3d support and that slow down the vm by using software rendering for it.

###Installed software

* java 8 oracle jdk
* git 
* idea 13 (Ultimate Edition) in folder /home/vagrant/workspace/devel/idea
* play 2.2.3 /home/vagrant/workspace/devel/play
* conscript
* sbt 0.13.5
* heroku
* nvm
* nodejs 0.10.28
* travis
* gnome-session-flashback
* softcover-nonstop version my fork see here []()
* texlive latex distribution 
* inkscape
* calibre
* epubcheck
* kindlegen

All installed software can be used as user vagrant from the command line. Idea is intalled as menu item under Programming entry.

##Usage

1. clone this repo with ```git clone git@github.com:pussinboots/vagrant-devel.git```
2. ```cd vagrant-devel```
3. look into the Vagrantfile and adapt if it nessary like reduce or increase memory
4. start up vagrant with ```vagrant up``` can take a while has to be download 1.2 GB base box (only first time)

##Vagrantfile

Short explanation of the used Vagrantfile.
```ruby
Vagrant.configure("2") do |config|
  
  config.vm.box = "pussinboots/ubuntu-truly-full"
 
  config.vm.provider :virtualbox do |vb|
	vb.gui = true
	vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
	vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
	vb.customize ["modifyvm", :id, "--vram", "128"]
	vb.customize ["modifyvm", :id, "--graphicscontroller", "vboxvga"]
	vb.customize ["modifyvm", :id, "--accelerate3d", "on"]
	vb.customize ["modifyvm", :id, "--ioapic", "on"]
	vb.customize ["modifyvm", :id, "--hwvirtex", "on"]
	vb.customize ["modifyvm", :id, "--clipboard ", "bidirectional"]
	vb.memory = 3072
	vb.cpus = 2
  end
end
```

This configure vagrant to use the  [vagrantcloud](https://vagrantcloud.com/pussinboots/ubuntu-truly-full) hosted base base. 
```ruby
config.vm.box = "pussinboots/ubuntu-truly-full"
```

Adapt the following configuration to increase or reduce the memory (in MB) or cpus that the VM will use. 
```ruby
vb.memory = 3072
vb.cpus = 2
```

The virtual box specific configuration for an explanation of each parameter look [here](https://www.virtualbox.org/manual/ch08.html).
```ruby
vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
vb.customize ["modifyvm", :id, "--vram", "128"] # memory for the graphic card
vb.customize ["modifyvm", :id, "--graphicscontroller", "vboxvga"] 
vb.customize ["modifyvm", :id, "--accelerate3d", "on"]
vb.customize ["modifyvm", :id, "--ioapic", "on"]
vb.customize ["modifyvm", :id, "--hwvirtex", "on"]
vb.customize ["modifyvm", :id, "--clipboard ", "bidirectional"]
```
