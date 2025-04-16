# X11 plus PulseAudio Proxy LXC LXD profile for running GUI applications

*I use this to run old Kodi 17 (that uses Python 2) in order to test backward compatibility of addons.*

Prepare profile (we need to populate some varialbes):
```
cat <<EOF > /tmp/x11_pulseaudio_proxy.profile
paste content of x11_pulseaudio_proxy.profile.template
EOF
```

Create profile:
```
lxc profile create x11_pulseaudio_proxy < /tmp/x11_pulseaudio_proxy.profile
lxc profile show x11_pulseaudio_proxy
```

Launch container:
```
lxc launch ubuntu:18.04 --profile default --profile x11_pulseaudio_proxy kodi17
```

(Optionally) Map your Downloads directory into container:
```
lxc config device add kodi17 downloads disk source=/home/$USER/Downloads path=/home/ubuntu/Downloads
```

Test X11 and pulseaudio:
```
lxc exec kodi17 -- sudo --user ubuntu --login
xmessage "Hello from LXC container!"
pactl info
paplay Downloads/test.wav
```

## References

This LXC profile is just a compilation of other people work:

https://blog.simos.info/running-x11-software-in-lxd-containers/

https://discuss.linuxcontainers.org/t/apparmor-permission-denied-for-unix-proxy-device/9434

https://discuss.linuxcontainers.org/t/proxy-device-not-connecting-to-pulseaudio-on-lxd-host/7472

https://archives.flockport.com/run-gui-apps-in-lxc-containers/

https://stgraber.org/2014/02/09/lxc-1-0-gui-in-containers/

And this repo is more like memo for myself :-)
