<div align="center">

# 👨🏻‍💻 **Pipe-Testnet-Node-Guide** 👨🏻‍💻

</div>

# 🖥️ Device/System Requirements 


![Screenshot 2025-05-17 004130](https://github.com/user-attachments/assets/d5dbeabf-d78b-4d0c-a9dc-bd4cf77c3162)


# Pre-Requirements 🛠

* **You have to be Whitelist, if u have to run the Pipe testnet node-**

* If U alraedy rcvd the Mail then u can run the Pipe testnet-

* Fill this form if u have not rcvd the mail/invite code Yet- [Form](https://tinyurl.com/4435uukt)

# Install Require Dependecies

```
sudo apt-get update && sudo apt-get upgrade -y
```

* Other Packages

```
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev screen ufw -y
```

# Allow Incoming connections on Ports 

```
sudo ufw allow 22
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow ssh
sudo ufw enable
sudo ufw reload
```

# Applying Configurations

* To apply these settings, create a configuration file using these following command:

```
sudo bash -c 'cat > /etc/sysctl.d/99-popcache.conf << EOL
net.ipv4.ip_local_port_range = 1024 65535
net.core.somaxconn = 65535
net.ipv4.tcp_low_latency = 1
net.ipv4.tcp_fastopen = 3
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_window_scaling = 1
net.ipv4.tcp_wmem = 4096 65536 16777216
net.ipv4.tcp_rmem = 4096 87380 16777216
net.core.wmem_max = 16777216
net.core.rmem_max = 16777216
EOL'
sudo sysctl -p /etc/sysctl.d/99-popcache.conf
```

* Increase file limits for performance

```
sudo bash -c 'cat > /etc/security/limits.d/popcache.conf << EOL
*    hard nofile 65535
*    soft nofile 65535
EOL'
```


# Create Directories

```
sudo mkdir -p /opt/popcache
sudo mkdir -p /opt/popcache/logs
```


# Download the binary 

* Open - [Download_Binary](https://download.pipe.network)

* Enter Invite Code that u have rcvd from pipe Mail

* Select the `Architecture` As per your vps or Device and Download it in your `Personal Computer`!



* 🔺 If u are using Vps then u have to move the binary file from your Local device to Vps by the Termius (SFTP) Feature!


<div align="center">

# Here are some steps, how u can move pop binary to vps 

</div>


*


# Set Configuration File


```
nano config.json
```

```
{
  "pop_name": "your-pop-name",
  "pop_location": "Your Location, Country",
  "invite_code": "Enter your Invite Code",
  "server": {
    "host": "0.0.0.0",
    "port": 443,
    "http_port": 80,
    "workers": 0
  },
  "cache_config": {
    "memory_cache_size_mb": 4096,
    "disk_cache_path": "./cache",
    "disk_cache_size_gb": 100,
    "default_ttl_seconds": 86400,
    "respect_origin_headers": true,
    "max_cacheable_size_mb": 1024
  },
  "api_endpoints": {
    "base_url": "https://dataplane.pipenetwork.com"
  },
  "identity_config": {
    "node_name": "your-node-name",
    "name": "Your Name",
    "email": "your.email@example.com",
    "website": "https://your-website.com",
    "discord": "your_discord_username",
    "telegram": "your_telegram_handle",
    "solana_pubkey": "YOUR_SOLANA_WALLET_ADDRESS_FOR_REWARDS"
  }
}
```

* Paste the following code in `config.json` File-

* Dont Forget to make all these changes in File! All are Important Configrations!

* `"pop_location":` Your VPS Region's Location


* `"memory_cache_size_mb":`  How much RAM memory u want to allocate to node! (In MB)....Like if u have 16 GB RAM then u can allocate 50-60% of total RAM!

* `"disk_cache_size_gb":` How much disk space do u want to allocate! (In GB) ....like if u have 500 GB free space in your disk then u can allocate around  200GB


# Create a Systemd Service File

```
sudo bash -c 'cat > /etc/systemd/system/popcache.service << EOL
[Unit]
Description=POP Cache Node
After=network.target

[Service]
Type=simple
User=root
Group=root
WorkingDirectory=/opt/popcache
ExecStart=/opt/popcache/pop
Restart=always
RestartSec=5
LimitNOFILE=65535
StandardOutput=append:/opt/popcache/logs/stdout.log
StandardError=append:/opt/popcache/logs/stderr.log
Environment=POP_CONFIG_PATH=/opt/popcache/config.json

[Install]
WantedBy=multi-user.target
EOL'
```

* Reload

```
sudo systemctl daemon-reload
```

* Enable

```
sudo systemctl enable popcache
```

* Start

```
sudo systemctl start popcache
```






