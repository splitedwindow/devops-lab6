Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2204"
    config.vm.disk :disk, size: "400MB", name: "extra_storage1"
    config.vm.disk :disk, size: "400MB", name: "extra_storage2"
    config.vm.disk :disk, size: "400MB", name: "extra_storage3"
    config.vm.disk :disk, size: "400MB", name: "extra_storage4"
  config.vm.provision "shell", inline: <<-SHELL
    apt-get -y update
    apt-get -y install mc vim

    parted -a optimal --script /dev/sdb  mklabel gpt  mkpart primary ext4 0% 100%  set 1 lvm on
    parted -a optimal --script /dev/sdc  mklabel gpt  mkpart primary ext4 0% 100%  set 1 lvm on
    parted -a optimal --script /dev/sdd  mklabel gpt  mkpart primary ext4 0% 100%  set 1 lvm on
    parted -a optimal --script /dev/sde  mklabel gpt  mkpart primary ext4 0% 100%  set 1 lvm on

    lvm pvcreate /dev/sdb1 /dev/sdc1 /dev/sdd1 /dev/sde1
    
    lvm vgcreate vg1 /dev/sdb1 /dev/sdc1 /dev/sdd1 /dev/sde1

    lvm lvcreate -n lv1 -l50%FREE vg1
    lvm lvcreate -n lv2 -l100%FREE vg1
    
    mkfs.ext4 /dev/vg1/lv1
    mkfs.ext4 /dev/vg1/lv2

    mkdir /mnt/vol1
    mkdir /mnt/vol2
    
    echo '/dev/vg1/lv1 /mnt/vol1 ext4 defaults 0 0' >> /etc/fstab
    echo '/dev/vg1/lv2 /mnt/vol2 ext4 defaults 0 0' >> /etc/fstab

    mount -a

    lsblk

  SHELL
end