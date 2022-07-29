# VERSION="1.1.20210618"
VERSION="1.1.20220727"
Vagrant.configure("2") do |config|
    config.vbguest.auto_update = false
    config.vm.box = "magmacore/magma_dev"
    config.vm.box_version = VERSION
    config.vm.provision "shell" do |s|
      s.inline = <<-SHELL
       set -x
       sudo rm -f /etc/apt/sources.list.d/packages_fluentbit_io_ubuntu_focal.list
       sudo rm -rf /var/cache/apt/
       sudo apt update -y
       sudo apt install -y fio
       fio --name TEST --eta-newline=5s --filename=temp.file --rw=randread --size=2g --io_size=10g --blocksize=4k --ioengine=libaio --fsync=1 --iodepth=1 --direct=1 --numjobs=32 --runtime=60 --group_reporting |
         tee -a /vagrant/#{VERSION}-Testing-random-4K-reads
       fio --name TEST --eta-newline=5s --filename=temp.file --rw=randrw --size=2g --io_size=10g --blocksize=4k --ioengine=libaio --fsync=1 --iodepth=1 --direct=1 --numjobs=1 --runtime=60 --group_reporting |
         tee -a /vagrant/#{VERSION}-Mixed-random-4K-read-and-write
       mount > /vagrant/#{VERSION}.mount
       cat /etc/fstab > /vagrant/#{VERSION}.fstab
       dmesg > /vagrant/#{VERSION}.dmesg
     SHELL
    end
end
