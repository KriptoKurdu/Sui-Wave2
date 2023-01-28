# Sui-Wave2


##  Sistem Gereksinimleri

**CPU:** 10 CPU

**RAM:** 16 GB

**SSD:** 1000 GB (1 TB)


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


```
cp $HOME/.nibid/data/priv_validator_state.json $HOME/.nibid/priv_validator_state.json.backup
```

```
rm -rf $HOME/.nibid/data 
```

```
curl -L https://snapshots.kjnodes.com/nibiru-testnet/snapshot_latest.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.nibid
```

```
mv $HOME/.nibid/priv_validator_state.json.backup $HOME/.nibid/data/priv_validator_state.json
```


```
nibid start
```





Eski cüzdani eklemek İçin
```
nibid keys add wallet --recover
```
Yeni cüzdan oluşturmak için
```
nibid keys add cuzdanAdi
```

Node durdurmak için
```
sudo systemctl stop nibid
```



#### İlk Defa VALİDATOR Oluşturacaklar için ('cuzdanAdiniz' Kısmını kendinize göre değiştirin)
```
nibid tx staking create-validator \
--amount=100000unibi \
--pubkey=$(nibid tendermint show-validator) \
--moniker=monikerAdi \
--chain-id=nibiru-testnet-2 \
--commission-rate="0.1" \
--commission-max-rate="0.10" \
--commission-max-change-rate="0.01" \
--min-self-delegation="1" \
--fees=10000unibi \
--from=cuzdanAdi \
-y
```

Delege Etmek için
```
nibid tx staking delegate validator_adresi 99500000unibi --from wallet --chain-id nibiru-testnet-2 --fees 5000unibi
```





