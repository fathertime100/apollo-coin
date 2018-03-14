# Apollo Coin Step-by-Step Install Instructions
This step-by-step guide was adopted from Zoldur's script and developed to support my video of this process on [Youtube](https://).  Thank-you Zoldur for doing most of the heavy lifting on this.  This guide will help you quickly and easily install an [Apollon Masternode](http://apolloncoin.io/) on a Linux server running Ubuntu 16.04 and it will help you set up your wallet with the coins to support the masternode.  Use this guide at your own risk.
***
**DISCLAIMER:** The references contained herein are an opinion and is for information purposes only.  It is not intended to be investment advice.  Seek a duly licensed professional for investment advice.
***
This guide assumes some basic knowledge of Linux and Cryptocurrency Exchanges.  You do not need to be an expert to complete this tutorial.  If you have any questions, please feel free to reach out to me directly on Discord, my username is fathertime100.  
***
## OVERVIEW
This process will does take some time to complete from start to finish.  If you are adept at masternode setup it will take you less time, if you are a first-timer, expect to put in a few hours to complete this process.

It involves six steps:

A.  Masternode Installation - 40 minutes
B.  Wallet Installation - 15 minutes
C.  Purchase and Stake Coins - 40 to 120 minutes
D.  Start Masternode - 15 minutes
E.  Network Verification - 40 to 90 minutes
F.  Maintenance


## A.  Masternode Installation
**BEFORE YOU START** Create a blank text file or notepad on your computer that you can use throughout this tutorial.  You can download a text file that I have created from [here](https://drive.google.com/open?id=1zr5txLveadmKaUDBw4p20Pn-VD8CmbFb) as a starting point.  Please download this and save it somewhere safe.  You will copy and paste several values to it throughout this process and you will need it for reference to perform masternode maintenance in the future.  

1. Setup a Linux Ubuntu 16.04 virtual private server (VPS) from [Vultr](https://www.vultr.com/?ref=7348757).  This server 
costs $5 USD / month.  If you need help setting up a VPS with Vultr, please watch this [video on how to set up a Vultr VPS with Ubuntu 16.04](https://youtu.be/jsP3K0D6ONE).
2. SSH into your server using Terminal on a Mac or [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) on Windows.  If this is your first time doing this, please watch the video in step 1.
3.  Copy and paste the below code snippet into your SSH session and hit enter.  This will download the masternode installation script onto your VPS.  
```
wget -q https://raw.githubusercontent.com/fathertime100/apollon/master/apollon_install.sh
```
4.  Install your server.  Copy and paste this code into your SSH session and press enter. Note that this process will take between about 30 minutes to complete.  
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
MASTERNODE PRIVATEKEY is: adfwivhw0ru340230fMZdasdfasdweav3459834u5B1kxHV2398aav93
Please check Apollon is running with the following command: systemctl status Apollon.service
=============================================================================================
```

6. Copy and paste this information somewhere safe.  You will need this information for the steps below.

7. Next we'll check the status of the server.  Copy and paste the below command into your SSH session and hit enter.  
```
systemctl status Apollon
```
This will show you a result similar to this:
```
● Apollon.service - Apollon service
   Loaded: loaded (/etc/systemd/system/Apollon.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2018-03-14 19:19:03 UTC; 9min ago
 Main PID: 22843 (Apollond)
   CGroup: /system.slice/Apollon.service
           └─22843 /usr/local/bin/Apollond -daemon -conf=/root/.Apollon/Apollon.conf -datadir=/root/.Apollon

Mar 14 19:19:03 APOLLON-2 systemd[1]: Starting Apollon service...
Mar 14 19:19:03 APOLLON-2 systemd[1]: Started Apollon service.
```
As long as your server says **"Active: active (running)"**, you're good to go.  

8.  Next, let's check to see that it's syncing blocks with the network.  Copy and past the below command into your SSH session and hit enter.  
```
Apollond getinfo
```
You will see a result that is similiar to this:
```
{
    "version" : "v1.0.3.0-60028",
    "protocolversion" : 60028,
    "walletversion" : 60000,
    "balance" : 0.00000000,
    "darksend_balance" : 0.00000000,
    "newmint" : 0.00000000,
    "stake" : 0.00000000,
    "blocks" : 8293,
    "timeoffset" : 0,
    "moneysupply" : 20686950.00000000,
    "connections" : 12,
    "proxy" : "",
    "ip" : "108.61.195.216",
    "difficulty" : 30105.01429933,
    "testnet" : false,
    "keypoololdest" : 1521055109,
    "keypoolsize" : 101,
    "paytxfee" : 0.01000000,
    "mininput" : 0.00000000,
    "errors" : ""
}
```
Take note of the **"blocks"** field.  It should be around 8 to 10 thousand at this point, if it's higher, don't worry, you were just slow to enter this command after the previous one and your server is syncing with the network, everything is ok.  

9.  Now we'll check the masternode status by copying and pasting this command into the ssh session and hitting enter.  
```
Apollond masternode status
```
You will see a result that is similiar to this:
```
{
    "vin" : "CTxIn(COutPoint(0000000000, 4294967295), coinbase )",
    "service" : "108.61.195.216:12116",
    "status" : 8,
    "pubKeyMasternode" : "APFxbJgmgxCbWYqUAJQv3TtKERVUQY6yvz",
    "notCapableReason" : "Could not find suitable coins!"
}
```
The **"status"** of your masternode will be one of the following:

1 = Your masternode has not been processed by the network yet.  Please wait.
2 = Your masternode is active and synced to the network.
3 = Your masternode is inactive.
4 = Your masternode has stopped.
5 = Your masternode seed transaction hasn't reached the minimum of 16 confirmations.  
6 = Your masternode port is closed.
7 = Your masternode port is open.
8 = Your masternode is syncing to the network.
9 = Your masternode is remotely enabled and active.

Your masternode should currently have a status of either 8 or 2.  

If it is an 8, if you run this command again:
```
Apollond getinfo
```
Your **"blocks"** field have increased from the last time you checked it.
```
{
    "version" : "v1.0.3.0-60028",
    "protocolversion" : 60028,
    "walletversion" : 60000,
    "balance" : 0.00000000,
    "darksend_balance" : 0.00000000,
    "newmint" : 0.00000000,
    "stake" : 0.00000000,
    "blocks" : 8293,
    "timeoffset" : 0,
    "moneysupply" : 20686950.00000000,
    "connections" : 12,
    "proxy" : "",
    "ip" : "108.61.195.216",
    "difficulty" : 30105.01429933,
    "testnet" : false,
    "keypoololdest" : 1521055109,
    "keypoolsize" : 101,
    "paytxfee" : 0.01000000,
    "mininput" : 0.00000000,
    "errors" : ""
}
```

If it is a 2, this means that your masternode is active and synced to the network and needs the 25,000 coin stake in order to become a participant masternode on the Apollon network.   

Keep running the **Apollond masternode status** command until you achieve a **"status"** of 2.  Once your status is 2, move onto the next step of the tutorial.  

***

## B. Purchase 25,000 Apollo Coins (+100 coins for exchange and transfer fees)
1.  Set up an account on either [Graviex](https://graviex.net/markets/xapbtc) or [CryptoBridge](https://wallet.crypto-bridge.org/market/BRIDGE.XAP_BRIDGE.BTC).  NOTE: both of these exchanges have very poor interfaces with virtually no feedback mechanisms or alerts, so just be patient after every click and wait for something to change on the screen, trudge through the mud, you'll eventually get your coins.  
2.  Look up the current price of [BTC to XAP](https://graviex.net/markets/xapbtc) and calculate how much BTC you'll need to transfer to the exchange in order to purchase **25,100 XAP**.  
3.  Deposit BTC to the exchange from one of your BTC wallets.  Note that currently, BTC transfers take about 30-40 minutes to be fully confirmed right now.  Use your source wallet to trace the transaction as neither exchange displays incoming transactions or their confirmations.
4.  Initiate a buy order and wait for the order to be filled.  Depending on the market volume, this can take between 1 to 30 minutes.

Once you have your coins, WAIT, do not transfer them anywhere yet,.

***

## C. Desktop wallet setup

After the MN is up and running, you need to configure a windows desktop wallet accordingly. Here are the steps:  

**WINDOWS USERS**
1. Download the wallet from here: [Apollon Windows Wallet](https://github.com/apollondeveloper/ApollonCoin/releases/download/1.0.3/Apollon-qt.exe)
2. Open the Apollon Desktop Wallet.  
3. Go to RECEIVE and create a New Address with the label: **MN1**  
4. Copy and paste the public address to your working file.  Note: this is also the public address of your masternode that you will use to look up your masternode on [https://xap.overemo.com/masternodes](https://xap.overemo.com/masternodes).  It will not be published yet.  
5. Go to the exchange you purchased your coins on and set up a withdrawal of **25,000** coins EXACTLY to the public address you just copied from your Desktop Wallet.  **THIS IS VERY IMPORTANT, DO NOT SEND 25,100 COINS, ONLY SEND 25000 XAP TO MN1.  ALSO, YOU MUST DO THIS IN A SINGLE TRANSACTION.  YOU CAN NOT MAKE TWO 12,500 TRANSACTIONS TO THIS ADDRESS, THE SERVER WILL NOT REGISTER THIS AS A WALLET STAKE.  A SINGLE RECEIVING ADDRESS MUST ONLY HAVE ONE TRANSACTION OF 25,000 XAP SENT TO IT.  NEVER SEND ANY OTHER XAP TO THIS PUBLIC ADDRESS.  IF YOU WANT TO SEND ANY MORE XAP TO YOUR WALLET, CREATE A NEW ADDRESS.**
6. Wait for 16 confirmations.  To check the status of your transfer, go to your Desktop Wallet and go to **Transactions**.  Once you have initiated the transfer from the exchange, it will take a few minutes to show up in your wallet.  Once it has, you can double click on it and a 'Transaction details' pop-up will appear.  Under here, you will see **Status:** and it will tell you the number of confirmations that your transaction has achieved.  This pop-up does not update automatically, so close and reopen this pop-up until the number of confirmations is 16 or higher.   
7. Go to **Help -> "Debug Window - Console"**  
8. Enter the following command: **masternode outputs**     The result will look something like this:
```
{
    "b35aaf4fba02ea64c239ofasdoe8f55abc66cb26a8930656ad8020334bf871" : "1"
}
```
9. Copy and paste this information into your working file.
9. Go to **Masternodes** tab  
10. Click **Create** and fill these details:  
* Alias: **MN1**  
* Address: **VPS_IP:PORT**  from step A1 above.  Use port **12116**
* Privkey: **Masternode Private Key** from step A5 above, DO NOT USE THE PRIVATE KEY IN THIS DOCUMENT, USE THE ONE YOU CREATED.
* TxHash: **First value from Step 6**  
* Output index:  **Second value from Step 6, either a 1 or a 0**  
* Reward address: leave blank  
* Reward %: leave blank  
11. Click **OK** to add the masternode  
12. Click **Start All**  
***

**MAC USERS**
###(DO NOT USE THE MAC WALLET, IT DOES NOT WORK AS OF 12-MAR-2018)

In order to keep your Masternode and your wallet seperate (recommended), you'll need to take on a little more cost, but you'll be able to use this for other masternode wallets that don't work on Mac yet as well. 
1.  Setup a windows VPS on [Virmach](https://virmach.com/windows-remote-desktop-vps/)
2.  Choose SSD2G for 10$ per month
3.  At checkout, use this coupon: LEBBF2016G2     Then it will only cost $5 USD per month.
4.  You can choose Windows 2012 or 2016 as the operating system.
5.  Download [Microsoft Remote Desktop 8.0 for Mac](https://itunes.apple.com/us/app/microsoft-remote-desktop-8-0/id715768417?mt=12)
6.  Add your credentials to Remote Desktop and select the resolution as 1280x960 (fits most laptop screens) and select colour as high 16-bit, this will dramatically speed up the performance of the remote server.  
7.  Double click on the Remote Desktop to log into Windows.  
8.  Go up to the **WINDOWS USERS** instructions above and start from step 1.  

***

## D. COMPLETION CHECK & MAINTENANCE
You've done it, you've just set up a masternode for Apollo.  Now we need to check to ensure that it is operating properly and communicating with the other servers on in the network.  In the future you can also use these commands to check your server status and ensure that things are running as they should be.  

**MAINTENANCE COMMAND 1: SERVER STATUS**
Copy and paste this command into terminal and press enter.
```
Apollond masternode status  
```
This will give you a response that looks like this:
```
{
    "vin" : "CTxIn(COutPoint(b35aaf4fba, 1), scriptSig=)",
    "service" : "144.202.88.54:12116",
    "status" : 9,
    "pubKeyMasternode" : "AKCeCBJm7GXHX8okPsSJ65cp9iKpefxnD7",
    "notCapableReason" : "Could not find suitable coins!"
}
```
There is only one important field here, 'status'.

**Status**
Ensure that your Masternode 'status' is either 8 or 9.  
Here is the list of possible Masternode statuses:

1 = Your masternode has not been processed by the network yet.  Please wait.

2 = Your masternode is active.

3 = Your masternode is inactive.

4 = Your masternode has stopped.

5 = Your masternode seed transaction hasn't reached the minimum of 16 confirmations.  

6 = Your masternode port is closed.

7 = Your masternode port is open.

8 = Your masternode is syncing to the network.

9 = Your masternode is remotely enabled and active.


There are two very confusing fields here, the first is 'pubKeyMasternode'.  Given the name of this field, one would expect that this field would be the one that is listed on the masternode public list here: [https://xap.overemo.com/masternodes](https://xap.overemo.com/masternodes).  It is not!  To look up your masternode on this list, use the public key you created in step C3.  

The other is 'notCapableReason'. notCapableReason seems to store a text string from the last found error on the server.  With that said, if your status is 8 or 9 and you still see a strange error here, like the one above, ignore it.  It is lkely just the last error that your server encountered before going live.

**pubKeyMasternodes**
You might be temped to look up your masternode using the pubKeyMasternode field on .  Don't bother, this is not the public key that is advertised on this list.  If you want to look up your masternode status on this public list, see below.

MAINTENANCE COMMAND 2
```
Apollond getinfo
```
will result in a response that looks something like this:
```
{
    "version" : "v1.0.3.0-60028",
    "protocolversion" : 60028,
    "walletversion" : 60000,
    "balance" : 0.00000000,
    "darksend_balance" : 0.00000000,
    "newmint" : 0.00000000,
    "stake" : 0.00000000,
    "blocks" : 19827,
    "timeoffset" : 0,
    "moneysupply" : 19507450.00000000,
    "connections" : 29,
    "proxy" : "",
    "ip" : "144.202.88.54",
    "difficulty" : 27482.97398816,
    "testnet" : false,
    "keypoololdest" : 1520844901,
    "keypoolsize" : 101,
    "paytxfee" : 0.01000000,
    "mininput" : 0.00000000,
    "errors" : ""
}
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

All donations are highly appreciated!

**XAP**: AJutr9AbhkDvdRjazGZVt1w2xE4rzWqbzW  
**BTC**: 
**ETH**:  
