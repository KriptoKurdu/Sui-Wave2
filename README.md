# Sui-Wave2


##  Sistem Gereksinimleri

**CPU:** 10 CPU

**RAM:** 32 GB

**SSD:** 1000 GB (1 TB)




#### Screen oluşturma
```
sudo apt install
```
```
apt install screen
```
```
screen -S sui-node
```



#### Linux Güncellemeleri
```
sudo apt-get update \
&& sudo apt-get install -y --no-install-recommends \
tzdata \
ca-certificates \
build-essential \
libssl-dev \
libclang-dev \
pkg-config \
openssl \
protobuf-compiler \
cmake
```

#### RUST Kurulumu

```
sudo curl https://sh.rustup.rs -sSf | sh -s -- -y
source $HOME/.cargo/env
rustc --version
```


#### Github repo

```
apt install git
```

```
cd $HOME
git clone https://github.com/MystenLabs/sui.git
cd sui
git remote add upstream https://github.com/MystenLabs/sui
git fetch upstream
git checkout -B testnet --track upstream/testnet
```
```
mkdir $HOME/.sui
```

#### Genesis dosyaları Kurulumu

```
wget -O $HOME/.sui/genesis.blob  https://github.com/MystenLabs/sui-genesis/raw/main/testnet/genesis.blob
```
```
cp $HOME/sui/crates/sui-config/data/fullnode-template.yaml $HOME/.sui/fullnode.yaml
sed -i.bak "s|db-path:.*|db-path: \"$HOME\/.sui\/db\"| ; s|genesis-file-location:.*|genesis-file-location: \"$HOME\/.sui\/genesis.blob\"| ; s|127.0.0.1|0.0.0.0|" $HOME/.sui/fullnode.yaml
```





#### Teşvikli kısımda değilseniz bu aşamayı da kurun (Video çekildikten sonra eklendi video da bu kısım yok siz yine de kurun)

```
sudo tee -a $HOME/.sui/fullnode.yaml  >/dev/null <<EOF

p2p-config:
  seed-peers:
   - address: "/dns/ams-suifn-ss1.testnet.sui.io/udp/8084"
   - address: "/dns/sgp-suifn-ss2.testnet.sui.io/udp/8084"
   - address: "/dns/icn-suifn-ss3.testnet.sui.io/udp/8084"
   - address: "/dns/ord-suifn-ss4.testnet.sui.io/udp/8084"
   - address: "/dns/lax-suifn-ss5.testnet.sui.io/udp/8084"
   - address: "/ip4/65.109.32.171/udp/8084"
   - address: "/ip4/65.108.44.149/udp/8084"
   - address: "/ip4/95.214.54.28/udp/8080"
   - address: "/ip4/136.243.40.38/udp/8080"
   - address: "/ip4/84.46.255.11/udp/8084"
EOF
```



#### SUİ binaries Kurulumu

```
cargo build --release --bin sui-node
mv ~/sui/target/release/sui-node /usr/local/bin/
sui-node -V
```



#### Servise Kurulumu

```
echo "[Unit]
Description=Sui Node
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=/usr/local/bin/sui-node --config-path $HOME/.sui/fullnode.yaml
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > $HOME/suid.service

mv $HOME/suid.service /etc/systemd/system/

sudo tee <<EOF >/dev/null /etc/systemd/journald.conf
Storage=persistent
EOF
```




#### SUİ Full node başlatma

```
sudo systemctl restart systemd-journald
sudo systemctl daemon-reload
sudo systemctl enable suid
sudo systemctl restart suid
journalctl -u suid -f
```





