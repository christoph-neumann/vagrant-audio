Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.hostname = "xenial-alsa"
  config.vm.provision :shell, :inline => $PROVISION
  config.vm.provider :virtualbox do |vb|
    vb.name = "xenial-alsa"
    if RUBY_PLATFORM =~ /darwin|mac os/
      vb.customize ["modifyvm", :id, '--audio', 'coreaudio', '--audiocontroller', 'hda']
    elsif RUBY_PLATFORM =~ /mswin|msys|mingw|cygwin|bccwin|wince|emc/
      vb.customize ["modifyvm", :id, '--audio', 'dsound', '--audiocontroller', 'hda']
    elsif RUBY_PLATFORM =~ /linux/
      vb.customize ["modifyvm", :id, '--audio', 'alsa', '--audiocontroller', 'hda']
    end
  end
end

$PROVISION = <<EOF
  set -e
  sudo apt-get update
  sudo apt-get install -y linux-image-extra-$(uname -r) alsa-utils
  sudo usermod -aG audio ubuntu
  echo -e 'pcm.!default {\n  type plug\n  slave.pcm "hw:0,1"\n}' | sudo tee >/dev/null /etc/asound.conf
  sudo modprobe snd
  sudo modprobe snd-hda-intel
  sleep 2
  sudo amixer -c 0 set Master playback 100% unmute
EOF
