# Apollon Coin Step-by-Step Install Instructions
This step-by-step guide was adopted from Zoldur's script and developed to support my video of this process on [Youtube](https://).  Thank-you Zoldur for doing most of the heavy lifting on this.  This guide will help you quickly and easily install an [Apollon Masternode](http://apolloncoin.io/) on a Linux server running Ubuntu 16.04 and it will help you set up your wallet with the coins to support the masternode.  Use this guide at your own risk.
***

## Masternode Installation
1. Setup a Linux Ubuntu 16.04 virtual private server (VPS) from [Vultr](https://www.vultr.com/?ref=7348757).  This server costs $5 USD / month.  If you need help setting up a VPS with Vultr, please watch this [video on how to set up a Vultr VPS with Ubuntu 16.04](https://www.youtube.com/).
2. SSH into your server using Terminal on a Mac or [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) on Windows.  If this is your first time doing this, please watch the video in step 1.
3.  Copy and paste the below code snippet into your SSH session and hit enter.  This will download the masternode installation script onto your VPS.  
```
wget -q https://raw.githubusercontent.com/fathertime100/apollo-coin/master/apollon_install.sh
```
4.  Install your server.  Copy and paste this code into your SSH session and press enter. Note that this process will take between 30 and 45 minutes to complete.  
```
bash apollon_install.sh
```
5.  **Server Private Key**.  After the script installs a number of programs, it will ask you to **"Enter your Apollon Masternode Private Key. Leave it blank to generate a new Masternode Private Key for you:"**.  Leave it blank and press enter.  It will take another few seconds to complete this task and then you'll be presented with a result that will look like this:

```
Installing and setting up firewall to allow ingress on port 12116

=============================================================================================
Apollon Masternode is up and running listening on port 12116.
Configuration file is: /root/.Apollon/Apollon.conf
Start: systemctl start Apollon.service
Stop: systemctl stop Apollon.service
VPS_IP:PORT 45.32.224.15:12116
MASTERNODE PRIVATEKEY is: 69XSr9H8ZNuDpwDEvoN9EBst5MmEaWBqUrzhbwR1qKVmcbw1E7t
Please check Apollon is running with the following command: systemctl status Apollon.service
=============================================================================================
```

CONGRATULATIONS, you've just completed the hardest part of setting up this masternode.

6. Copy and paste this information somewhere safe.  You will need this information for the steps below. 

***

## Purchase 25,100 Apollo Coins (+100 coins for exchange and transfer fees)
1.  Set up an account on either [Graviex](https://graviex.net/markets/xapbtc) or [CryptoBridge](https://wallet.crypto-bridge.org/market/BRIDGE.XAP_BRIDGE.BTC).

NOTE: both of these exchanges have very poor interfaces with virtually no feedback mechanisms or alerts, so just be patient after every click and wait for something to change on the screen, trudge through the mud, you'll eventually get your coins.  
2.  Look up the current price of <a href=https://graviex.net/markets/xapbtc target=_blank>BTC to XAP</a> [BTC to XAP](https://graviex.net/markets/xapbtc) and calculate how much BTC you'll need to transfer to the exchange in order to purchase 25,100 XAP.  
3.  Transfer BTC to the exchange from one of your BTC wallets.  Note that currently, BTC transfers take about 30-40 minutes to be fully confirmed right now.  Use your source wallet to trace the transaction as neither exchange displays incoming transactions or their confirmations.
4.  Initiate a buy order and wait for the order to be filled.  Depending on the market volume, this can take between 1 to 30 minutes.

Once you have your coins, WAIT, do not transfer them anywhere yet,.

## Windows Desktop wallet setup
###(DO NOT USE THE MAC WALLET, IT DOES NOT WORK AS OF 12-MAR-2018)

After the MN is up and running, you need to configure a windows desktop wallet accordingly. Here are the steps:  

**WINDOWS USERS**
1. Download the wallet from here: [Apollon Windows Wallet](https://github.com/apollondeveloper/ApollonCoin/releases/download/1.0.3/Apollon-qt.exe)
2. Open the Apollon Desktop Wallet.  
3. Go to RECEIVE and create a New Address: **MN1**  
4. THIS IS VERY IMPORTANT, DO NOT SEND 25,100 COINS, ONLY SEND **25000** XAP TO **MN1**.  
5. Wait for 16 confirmations.  
6. Go to **Help -> "Debug Window - Console"**  
7. Type the following command: **masternode outputs**  
8. Go to **Masternodes** tab  
9. Click **Create** and fill the details:  
* Alias: **MN1**  
* Address: **VPS_IP:PORT**  
* Privkey: **Masternode Private Key**  
* TxHash: **First value from Step 6**  
* Output index:  **Second value from Step 6**  
* Reward address: leave blank  
* Reward %: leave blank  
9. Click **OK** to add the masternode  
10. Click **Start All**  
***

**MAC USERS**
In order to keep your Masternode and your wallet seperate (recommended), you'll need to take on a little more cost, but you'll be able to use this for other masternode wallets that don't work on Mac yet.  
1.  Setup a windows VPS on [Virmach](https://virmach.com/windows-remote-desktop-vps/)
2.  Choose SSD2G for 10$ per month
3.  At checkout, use this coupon: LEBBF2016G2     Then it will only cost $5 USD per month.
4.  You can choose Windows 2012 or 2016 as the operating system.
5.  Download [Microsoft Remote Desktop 8.0 for Mac](https://itunes.apple.com/us/app/microsoft-remote-desktop-8-0/id715768417?mt=12)
6.  Add your credentials to Remote Desktop and select the resolution as 1280x960 (fits most laptop screens) and select colour as high 16-bit, this will dramatically speed up the performance of the remote server.  
7.  Double click on the Remote Desktop to log into Windows.  
8.  Go up to the **WINDOWS USERS** instructions above and start from step 1.  

***


## Usage:
```
Apollond masternode status  
Apollond getinfo
```
Also, if you want to check/start/stop **Apollon**, run one of the following commands as **root**:

```
systemctl status Apollon #To check if Apollon service is running  
systemctl start Apollon #To start Apollon service  
systemctl stop Apollon #To stop Apollon service  
systemctl is-enabled Apollon #To check if Apollon service is enabled on boot  
```  
***

## Issues:
For some reasons when Apollon starts for the first time, it will not  update the blockchain. Try this to fix
```
Apollond getinfo
systemctl stop Apollon
rm -r /root/.Apollon/{blk0001.dat,mncache.dat,peers.dat,smsgDB,smsg.ini,txleveldb}
sleep 10
systemctl start Apollon
Apollond getinfo
```
***

## Donations

Any donation is highly appreciated  

**XAP**: AMyyebyE8N5f79xLGtXRqLswjeHqbe2cBP  
**BTC**: 3MQLEcHXVvxpmwbB811qiC1c6g21ZKa7Jh  
**ETH**: 0x39d10fe57611c564abc255ffd7e984dc97e9bd6d  
**LTC**: LNZpK4rCd1JVSB3rGKTAnTkudV9So9zexB
