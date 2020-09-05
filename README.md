### Requirements
- Low end VPS running Ubuntu 16.04
- Domain name with ability to add an NS, A Records

### A. Prepare domain name (DNS)
For this example we will use example domain 'example.com'
###### Step 1
Create a 'seeder1' sub-domain NS record in DNS.

Example: seeder1.example.com NS record pointed to vps1.example.com

###### Step 2
Create a 'vps1' sub-domain A record in DNS

Example: vps1.example.com A record pointed to YOUR_VPS_IP_ADDRESS_HERE

###### Step 3
Provide your sub-domain name created in Step 1 to BitcoinZ Team to update seeder code and wait until update is completed.

### B. Install and Compile BitcoinZ seeder
###### Step 1 - prepare linux environment from root, enter following commands
`apt-get update`
`apt-get install build-essential libboost-all-dev libssl-dev`

###### Step 2 - create port forwarding with IP tables
`iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-port 5353`

###### Step 2 - create 'seeder' user from root and switch to seeder user
`useradd -m -s /bin/bash seeder`

`su seeder`

`cd ~`

###### Step 3 - download and build the seeder from source

`git clone https://github.com/btcz/bitcoinz-seeder.git`

`make`

###### Step 4 - create screen and run seeder
`screen -S seeder`

`./dnsseed -p 5353 -h seeder1.example.com -n vps1.example.com -m bitcoinzcommunity@gmail.com`

Detach from screen with CTRL A+D

Re-attach to screen with `screen -r seeder`

###### Troubleshooting & Testing

- If you have a firewall on your VPS, you need to make sure you allow connections for UDP port 53 for IPv4 and IPv6 (if enabled)
- You may need to wait a short time for the DNS records to update globally

You can test if your seeder is responding with node IPs type typing:

`dig seeder1.example.com A`

- If you decided to run Ubuntu 18.04+ you might need to turn off systemd-resolved from root by entering the following commands:

```
sudo systemctl disable systemd-resolved
sudo systemctl stop systemd-resolved
```
