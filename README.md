# Avail Full Node Rehber

<img width="391" alt="logo_large 80d5666f" src="https://github.com/ExodusTR/Avail/assets/98022535/57cafddf-f548-4a4b-a4aa-15e6da536237">

# Minimum Sistem Gereksinimleri

* 2 CPU
* 4GB RAM
* 20-40GB SSD
* Ubuntu 20.04 ya da üstü

# Kurulum

Sistemi güncelleyelim.

```
sudo apt update && sudo apt upgrade -y
```
Gerekli paketleri yükleyelim.

```
sudo apt install make clang pkg-config libssl-dev build-essential git screen protobuf-compiler -y
```
Rust kuralım ve ayarlayalım.

```
curl https://sh.rustup.rs -sSf | sh
```
```
source $HOME/.cargo/env
```
```
rustup update nightly
```
```
rustup target add wasm32-unknown-unknown --toolchain nightly
```
Avail reposunu klonlayalım.

```
git clone https://github.com/availproject/avail.git
```
Screen oluşturalım ve Avail klasörüne girelim.

```
screen -S avail
```
```
cd avail
```
1.8.0.2 sürümünün branchını hedefleyelim.

```
git checkout v1.8.0.2
```

Kurulumu yapalım.

```
cargo build --release
```

Şimdi servis dosyası oluşturacağız.

```
sudo touch /etc/systemd/system/availd.service
```
```
sudo nano /etc/systemd/system/availd.service
```

Aşağıdaki kodları yapıştırdıktan sonra kendimize bir node ismi belirleyip "Nodeismi" yazan yere tırnaklar kalacak şekilde yazalım. Sonra CTRL + X'e ve ardından Y'ye basarak kaydedip çıkalım.

```
[Unit]
Description=Avail Validator
After=network.target
StartLimitIntervalSec=0

[Service]
User=root
Type=simple
Restart=always
RestartSec=120
ExecStart=/root/avail/target/release/data-avail -d `pwd`/data --chain goldberg --port 30333 --validator --name "Nodeismi"

[Install]
WantedBy=multi-user.target
```

Servis dosyasını aktifleştirelim ve başlatalım.

```
sudo systemctl enable availd.service
```
```
sudo systemctl start availd.service
```
```
sudo systemctl status availd.service
```
Servisin çalıştığını gördükten sonra tekrar Ctrl + C ile servis infosunu kapatalım. Ardından node loglarını açalım.

```
journalctl -f -u availd
```
İşlemler bu kadar. Screenden çıkabiliriz.

* https://telemetry.avail.tools/ sitesinde node ismimizi ve bilgilerimizi görebiliriz. 
Ancak Telemetry'nin aynı anda sadece 1000 kişi gösterme kapasitesi olduğunu unutmayın. İsminizi göremeseniz bile loglar akıyorsa node çalışıyordur. 


 

