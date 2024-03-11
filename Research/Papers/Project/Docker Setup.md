https://grand-challenge.org/documentation/setting-up-wsl-with-gpu-support-for-windows-11/

#### Versions

https://docs.docker.com/desktop/release-notes/ (4.24.2)

```bash
# remove
# https://askubuntu.com/questions/935569/how-to-completely-uninstall-docker
dpkg -l | grep -i docker
sudo apt-get purge -y docker-engine docker docker.io docker-ce docker-ce-cli docker-compose-plugin
sudo apt-get autoremove -y --purge docker-engine docker docker.io docker-ce docker-compose-plugin
sudo rm -rf /var/lib/docker /etc/docker
sudo rm /etc/apparmor.d/docker
sudo groupdel docker
sudo rm -rf /var/run/docker.sock
```

#### NVIDIA Container Toolkit

https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html

#### Command 'docker' could not be found

https://github.com/docker/for-win/issues/13088

https://forums.docker.com/t/an-unexpected-error-was-encountered-while-executing-a-wsl-command/137525/15
```
netsh winsock reset_ # in powershell
```

#### Update WSL to newer release

* https://github.com/docker/for-win/issues/13580#issuecomment-1619667316
* https://github.com/microsoft/WSL/releases

