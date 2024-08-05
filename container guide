### test on container
prepare
```bash
sudo modprobe ip_tables
sudo modprobe iptable_nat
sudo modprobe iptable_filter
sudo modprobe iptable_mangle
```
user
```bash
curl -fsSL https://get.docker.com | bash
usermod -aG docker $USER && sudo su $USER
```
run
```bash
mkdir data
docker run -d --privileged  --name=ovpn --network=host   -v ./data:/openvpn  openvpn/openvpn-as
# find openvpn user password
docker logs -f ovpn | grep pass

# open https://localhot:943
```
# don't forget set custom DNS
# like 8.8.8.8 & 8.8.4.4
