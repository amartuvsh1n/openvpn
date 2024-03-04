[UI](https://github.com/d3vilh/openvpn-ui)

```bash
# https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-an-openvpn-server-on-ubuntu-22-04
$ sudo su
$ apt install -y openvpn easy-rsa
$ ls /usr/share/easy-rsa/
$ cd /usr/share/easy-rsa/easyrsa
$ cd /usr/share/easy-rsa/
$ vim
$ apt install vim -y
$ vim vars
$ ll
$ ./easyrsa init-pki
$ ./easyrsa gen-req server nopass
$ ls
$ ls /etc/openvpn/
$ ls /etc/openvpn/server/
$ cp pki/reqs/server.req /etc/openvpn/server/
$ cp pki/private/server.key /etc/openvpn/server/
$ ./easyrsa sign-req server server
$ ls
$ ls pki/
$ ./easyrsa import-req /etc/openvpn/server/server.req server
$ ./easyrsa sign-req server server
$ ./easyrsa build-ca
Minii1234
$ ./easyrsa sign-req server server
yes
Minii1234
$ cp pki/issued/server.crt /etc/openvpn/server/
$ cp pki/private/ca.key /etc/openvpn/server/
$ openvpn --genkey --secret ta.key
$ cp ta.key /etc/openvpn/server/
$ cp /usr/share/easy-rsa/pki/ca.crt /etc/openvpn/server/ 
$ cp /usr/share/doc/openvpn/examples/sample-config-files/server.conf /etc/openvpn/server/
$ vim /etc/openvpn/server.conf
port 1994
;tls-auth ta.key 0 # This file is secret
tls-crypt ta.key
;cipher AES-256-CBC
cipher AES-256-GCM
# nemeh
auth SHA256
;dh dh2048.pem
dh none
$ vim /etc/sysctl.conf
net.ipv4.ip_forward = 1
$ systemctl start --enable openvpn-server@server.service
$ ./easyrsa build-serverClient-full bat nopass
Minii1234
$ mkdir /home/user/vpn_clients
$ cp /usr/share/easy-rsa/pki/issued/bat.crt /home/user/vpn_clients/
$ cp /usr/share/easy-rsa/pki/private/bat.key /home/user/vpn_clients/
$ cd /home/user/vpn_clients/ 
$ cp /etc/openvpn/server/ta.key .
$ cp /etc/openvpn/server/ca.crt .   
$ cp /usr/share/doc/openvpn/examples/sample-config-files/client.conf./base.conf
$ vim base.conf
remote 172.22.212.227 1994
;ca ca.crt
;cert client.crt
;key client.key
;tls-auth ta.key 1
cipher AES-256-GCM
auth SHA256
key-direction 1
$ vim ./make_config.sh
#!/bin/bash
 
# First argument: Client identifier
 
KEY_DIR=./
OUTPUT_DIR=./
BASE_CONFIG=./base.conf
 
cat ${BASE_CONFIG} \
    <(echo -e '<ca>') \
    ${KEY_DIR}/ca.crt \
    <(echo -e '</ca>\n<cert>') \
    ${KEY_DIR}/${1}.crt \
    <(echo -e '</cert>\n<key>') \
    ${KEY_DIR}/${1}.key \
    <(echo -e '</key>\n<tls-crypt>') \
    ${KEY_DIR}/ta.key \
    <(echo -e '</tls-crypt>') \
    > ${OUTPUT_DIR}/${1}.ovpn
$ chmod +x make_config.sh
$ ./make_config.sh bat 
 ```



