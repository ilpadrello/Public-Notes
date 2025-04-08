`sudo wg genkey | tee privatekey | wg pubkey > publickey`

`sudo wg show wg0`

`cat /etc/wireguard/wg0.conf`

`sudo systemctl restart wg-quick@wg0`





