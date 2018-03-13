# Apollon Coin Easy Install Script
This easy install shell script was adopted from Zoldur's script.  Thank-you to him for doing most of the work.  The script helps you quickly and easily install an [Apollon Masternode](http://apolloncoin.io/) on a Linux server running Ubuntu 16.04. Use it on your own risk.
***

## Installation
1. Order a Linux Ubuntu 16.04 server from [Vultr](https://www.vultr.com/?ref=7348757).  This costs $5 USD / month and is very easy to setup.  If you need help setting up a VPS for your masternode, I will post a video on how to do this safely and securely in another video shortly.  
2. SSH into your server using Terminal on a Mac (built-in) or Putty on Windows, which you can download [here](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).


```
wget -q https://raw.githubusercontent.com/fathertime100/apollo-coin/master/apollon_install.sh
bash apollon_install.sh
```
***

## Purchase 25,000 Apollo Coins (+100 coins for exchange and transfer fees)
You can purchase your coins from either [Graviex](https://graviex.net/markets/xapbtc) or [CryptoBridge](https://wallet.crypto-bridge.org/market/BRIDGE.XAP_BRIDGE.BTC).  They exchanges both have very poor interfaces with virtually no feedback mechanisms or alerts, so just be patient after every click and wait for something to change on the screen, trudge through the mud, you'll eventually get your coins.  Both only provide BTC to XAP exchange, so you'll have to transfer BTC from one of your BTC wallets, it takes about 30-40 minutes to be fully confirmed right now.  Use your source wallet to trace the transaction as neither exchange displays incoming transactions or their confirmations.

Once you've set up your account and transferred enough BTC to purchase 25,100 XAP, initiate a buy order and wait for the order to be filled.  Depending on the market volume, this can take between 1 to 30 minutes.

Once you have your coins, WAIT, do not transfer them anywhere yet,.

## Windows Desktop wallet setup  (DO NOT USE THE MAC WALLET, IT DOES NOT WORK AS OF 12-MAR-2018)

After the MN is up and running, you need to configure a windows desktop wallet accordingly. Here are the steps:  

WINDOWS USERS
1. Download the wallet from here: [Apollon Windows Wallet](https://github.com/apollondeveloper/ApollonCoin/releases/download/1.0.3/Apollon-qt.exe)
2. Open the Apollon Desktop Wallet.  
3. Go to RECEIVE and create a New Address: **MN1**  
4. Send **25000** XAP to **MN1**.  
5. Wait for 16 confirmations.  
6. Go to **Help -> "Debug Window - Console"**  
6. Type the following command: **masternode outputs**  
7. Go to **Masternodes** tab  
8. Click **Create** and fill the details:  
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
