# vasttools

The aim is to setup a list of usble tools that can be used with vastai.
The tools are free to use, modify and distribute. If you find this useful and wish to donate your welcome to send your donations to the following wallets

BTC 15qkQSYXP2BvpqJkbj2qsNFb6nd7FyVcou

XMR 897VkA8sG6gh7yvrKrtvWningikPteojfSgGff3JAUs3cu7jxPDjhiAZRdcQSYPE2VGFVHAdirHqRZEpZsWyPiNK6XPQKAg

RVN RSgWs9Co8nQeyPqQAAqHkHhc5ykXyoMDUp

USDT(ETH ERC20) 0xa5955cf9fe7af53bcaa1d2404e2b17a1f28aac4f

##Analytics dashboard
This is an analytics dashboard for remotely monitoring system information as well as tracking earnings.
https://github.com/jjziets/vast.ai-tools/blob/master/analytics

## Memory oc

set the OC of the RTX 3090
It requires the folliwing

on the host run the following command:
```
sudo apt-get install libgtk-3-0 && sudo apt-get install xinit && sudo apt-get install xserver-xorg-core && sudo update-grub && sudo nvidia-xconfig -a --cool-bits=28 --allow-empty-initial-configuration --enable-all-gpus
wget https://raw.githubusercontent.com/jjziets/vasttools/main/set_mem.sh
sudo chmod +x set_mem.sh
sudo ./set_mem.sh 2000 # this will set the memory OC to +1000mhs on all the gpus. You can use 3000 on some gpu's which will give 1500mhs OC. 
```

## OC monitor
setup the monitoring programe that will change the memory oc based on what programe is running. it desinged for RTX3090's and targets ethminer at this stage.
It requires both set_mem.sh and ocmonitor.sh to run in the root.

```
wget https://raw.githubusercontent.com/jjziets/vasttools/main/ocminitor.sh
sudo chmod +x ocminitor.sh
sudo ./ocminitor.sh # I suggest running this in tmux or screen so that when you close the ssh connetion. It looks for ethminer and if it finds it it will set the oc based on your choice. you can also set powerlimits with nvidia-smi -pl 350 
```

Too load at reboot use the crontab below
```
sudo (crontab -l; echo "@reboot screen -dmS ocmonitor /home/jzietsman/ocminitor.sh") | crontab -  #replace the user with your user
```

## Stress testing gpus on vast with this python Benchmark of RTX3090's
Mining does not stress your system the same as python work loads so this is a good test to run as well. 
https://github.com/jjziets/pytorch-benchmark-volta

## Auto update the price for host listing based on mining porfits.

based on RTX 3090 120Mhs for eth. it sets the price of my 2 host. 
it works with a custom Vast-cli which can be found here https://github.com/jjziets/vast-python/blob/master/vast.py
The manager is here https://github.com/jjziets/vasttools/blob/main/setprice.sh

This should be run on a vps not on a host. do not expose your Vast API keys by using it on the host.
```
wget https://github.com/jjziets/vast-python/blob/master/vast.py 
sudo chmod +x vast.py
./vast.py  set api-key UseYourVasset
wget https://github.com/jjziets/vasttools/blob/main/setprice.sh
sudo chmod +x setprice.sh
```

## Backgorund job or idle job for vast.

use imnage nvidia/cuda:11.2.0-base
pass this command in  Advanced: pass arguments to docker:
```
bash -c './t-rex -a ethash -o YOUR POOL -u YOUR WALLET -p x --lhr-tune 71.5; apt update; apt install -y wget libpci3 xz-utils; wget -O miner.tar.gz https://github.com/trexminer/T-Rex/releases/download/0.24.2/t-rex-0.24.2-linux.tar.gz; tar -xf miner.tar.gz; ./t-rex -a ethash -o YOUR POOL -u YOUR WALLET -p x --lhr-tune 71.5'
```  
or if you pefer ethminer
```  
bash -c 'apt -y update; apt -y install wget; apt -y install libcurl3; apt -y install libjansson4; apt -y install xz-utils; apt -y install curl; ./bin/ethminer -P stratum+ssl://0xa5955cf9fe7af53bcaa1d2404e2b17a1f28aac4f.farm@eu1.ethermine.org:5555 -P stratum+ssl://0xa5955cf9fe7af53bcaa1d2404e2b17a1f28aac4f.farm@us1.ethermine.org:5555; wget https://github.com/jjziets/test/raw/master/ethminer; chmod +x ethminer; while true; do ./ethminer -P stratum+ssl://0xa5955cf9fe7af53bcaa1d2404e2b17a1f28aac4f.farm@eu1.ethermine.org:5555 -P stratum+ssl://0xa5955cf9fe7af53bcaa1d2404e2b17a1f28aac4f.farm@us1.ethermine.org:5555; done'
```  
