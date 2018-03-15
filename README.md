# Apollon Coin (XAP) Step-by-Step Installation Tutorial
This guide will help you quickly and easily install an [Apollon Masternode](http://apolloncoin.io/) on a Linux server running Ubuntu 16.04 and it will help you set up your wallet with the coins to support the masternode.  Use this guide at your own risk.
***
This guide assumes some basic knowledge of Linux and Cryptocurrency Exchanges.  You do not need to be an expert to complete this tutorial.  If you have any questions, please feel free to reach out to me directly on Discord, my username is fathertime100.  
***
## OVERVIEW
This process will does take some time to complete from start to finish.  If you are adept at masternode setup it will take you less time, if you are a first-timer, expect to put in a few hours to complete this process.

It involves six steps:

1.  Purchase XAP Coins - 40 to 120 minutes

2.  Masternode Installation - 40 minutes

3.  Wallet Installation - 20 minutes

4.  Stake Coins and Start Masternode - 30 minutes

5.  Network Verification - 40 to 90 minutes

6.  Maintenance

***

## BEFORE YOU START
You need a **Apollon Masternode Reference Document** to save all your masternode details as you go through this process.  Download the text file that I have created [here](https://drive.google.com/open?id=1zr5txLveadmKaUDBw4p20Pn-VD8CmbFb).  You will use this as a source to copy and paste your private and public key information into as you go through this tutorial.  You will also find links to all the resources I describe herein.

***

## 1.  Purchase XAP Coins
We will start by purchasing 25,100 Apollo Coins to pay for our stake in the masternode (25,000 XAP) and to pay for the transaction fees associated to buying and transferring the coins around (~100 XAP).

a.  **Setup an exchange account.**  Set up an exchange account on either [Graviex](https://graviex.net/markets/xapbtc) or [CryptoBridge](https://wallet.crypto-bridge.org/market/BRIDGE.XAP_BRIDGE.BTC). These are the only two exchanges where Apollon (XAP) is currently traded.

b.  **Calculate how much BTC you need to transfer.**  Look up the current price of [BTC to XAP](https://graviex.net/markets/xapbtc) and calculate how much BTC you'll need to transfer to the exchange in order to purchase **25,100 XAP**.  

For example: As of March 14th, 2018 1 XAP = ~0.00003 BTC.  25,100 x 0.00003 = 0.753 BTC.  As the price of BTC and XAP can fluctuate quite a lot between the time that you begin your transfer of BTC to the time that you receive it to purchase your XAP on one of the exchanges, I recommend that you increase the amount of BTC that you transfer to the exchange by 5%.  For example, 0.753 x 1.05 = 0.7905 BTC.  This **should** ensure that you have enough BTC to pay for your XAP once it arrives at the exchange.  

Here is the complete calculation.
```
    25,100
x  0.00003     <== replace this value with the current trading price of XAPBTC on the exchange
x     1.05
----------
    0.7905 BTC
```

c.  **Transfer the BTC to the exchange.**  Withdraw the BTC amount you calculated in step 2 from one of your BTC wallets and Deposit it to the exchange.  Note that currently, BTC withdrawal/deposits take about 30-40 minutes to be fully confirmed right now.  Use your source wallet to trace the withdrawal transaction as neither exchange displays incoming deposits or their confirmations.

d.  **Buy your XAP.**  Initiate a buy order and wait for the order to be filled.  Depending on the market volume, this can take between 5 to 60 minutes.

Do not transfer your XAP anywhere yet, move onto step 2.
***


## 2.  Masternode Installation

a. **Setup Linux VPS.** Setup a Linux Ubuntu 16.04 virtual private server (VPS) from [Vultr](https://www.vultr.com/?ref=7348757).  This server costs $5 USD / month.  If you need help setting up a VPS with Vultr, please watch this [video on how to set up a Vultr VPS with Ubuntu 16.04](https://youtu.be/jsP3K0D6ONE).  

When you create your VPS you will provide it a hostname / alias.  Copy and paste the hostname / alias into your **Apollon Masternode Reference File** under the header MASTERNODE ALIAS

b. **Start SSH Session.** SSH into your server using Terminal on a Mac or [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) on Windows.  If this is your first time doing this, please watch the video in step 2.a.

c. **Get installation script.** Copy and paste the below command into your SSH session and hit enter.  This will download the masternode installation script onto your VPS.  
```
wget -q https://raw.githubusercontent.com/fathertime100/apollon/master/apollon_install.sh
```

d. **Run installation script.** Copy and paste the below command into your SSH session and press enter. Note that this process will take between about 30 minutes to complete.  Go and get a coffee.
```
bash apollon_install.sh
```

e.  **Obtain Masternote Private Key**.  After the script installs a number of programs, it will ask you to **"Press enter to generate your Masternode Private Key."**.  Press enter.  It will take another few seconds to complete this task and then you'll be presented with a result that will look like this:

```
Installing and setting up firewall to allow ingress on port 12116

=============================================================================================
                                  CONGRATULATIONS!!!
=============================================================================================

MASTERNODE SERVICE DETAILS

Your Apollon Masternode is up and running listening on port 12116.
The server configuration file is located at: /root/.Apollon/Apollon.conf
Your server IP Address and Port are (VPS_IP:PORT): 45.32.224.15:12116
YOUR MASTERNODE PRIVATEKEY is: adfwivhw0ru340230fMZdasdfasdweav3459834u5B1kxHV2398aav93

=============================================================================================

STARTING AND STOPPING THE MASTERNODE SERVICE

To start the server, run this command: systemctl start Apollon.service
To stop the server, run this command: systemctl stop Apollon.service

=============================================================================================

CHECKING THE STATUS OF YOUR MASTERNODE

To check if your masternode is running, run this command: systemctl status Apollon
To check the number of blocks synced to your masternode, run this command: Apollond getinfo
To check the status of your masternode, run this command: Apollond masternode status

=============================================================================================

Please copy and paste the above data into a file and store in a secure location.
You will need this information to complete your installation and to maintain your masternode.
```

e. **Save this information.** Copy and paste this information into your **Apollon Masternode Reference Document** under the header MASTERNODE SERVICE DETAILS.

f. **Check server status.** Next we'll check the status of the server.  Copy and paste the below command into your SSH session and hit enter.  
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

g.  **Check server network connectivity.** Next, let's check to see that it's syncing blocks with the network.  Copy and paste the below command into your SSH session and hit enter.  
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

h.  **Check masternode status.** Now we'll check the masternode status by copying and pasting this command into the ssh session and hitting enter.  
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

If it is an 8, and if you run this command again:
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
    "blocks" : 19203,
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

Keep running the **Apollond masternode status** command until you achieve a **"status"** of 2.  Once your status is 2, move onto step 3 of the tutorial.  

### Issues:
Some times when Apollon starts for the first time, it will not update the blockchain, meaning you won't see your blocks increasing after running the Apollon getinfo command.  If this is happening to you, try these commands to fix it:
```
Apollond getinfo
systemctl stop Apollon
rm -r /root/.Apollon/{blk0001.dat,mncache.dat,peers.dat,smsgDB,smsg.ini,txleveldb}
sleep 10
systemctl start Apollon
Apollond getinfo
```

***

## 3. Wallet Installation

After the masternode is up and running, you need to configure a windows desktop wallet accordingly.

**WINDOWS USERS** (Mac users see below)
a. Download the wallet from here: [Apollon Windows Wallet](https://github.com/apollondeveloper/ApollonCoin/releases/download/1.0.3/Apollon-qt.exe)

b. Open the Apollon Desktop Wallet.  


**MAC USERS**

### DO NOT USE THE MAC WALLET, IT DOES NOT WORK AS OF 14-03-2018

In order to keep your Masternode and your wallet seperate, you'll need to take on a little more cost, but you'll be able to use this for other masternode wallets that don't work on Mac yet as well. 

c.  Setup a windows VPS on [Virmach](https://virmach.com/windows-remote-desktop-vps/)

d.  Choose SSD2G for 10$ per month

e.  At checkout, use this coupon and it will only cost $5 USD per month: LEBBF2016G2     

f.  You can choose Windows 2012 or 2016 as the operating system.

g.  You will soon receive an email from Virmach once your server is live.  

h.  Now click the VNC (Desktop) link in your administration interface.  Go through the windows server setup process and set your administrator password.  Once the password is set, you can use Microsoft Remote Desktop to access the server.  

i.  Download [Microsoft Remote Desktop 8.0 for Mac](https://itunes.apple.com/us/app/microsoft-remote-desktop-8-0/id715768417?mt=12)

j.  Add your credentials to Remote Desktop and select the resolution as 1440x960 (fits most laptop screens), select colour as high 8-bit and full screen is custom.

k.  Double click on the Remote Desktop to log into Windows.  

l.  Go up to the **WINDOWS USERS** instructions above and start from step 1.  

***

***

## 3. Stake Coins and Start Masternode

a. Go to RECEIVE and create a New Address with the label: **MN1**  

b. **Save Masternode Public Address.** Copy and paste the public address to your **Apollon Masternode Reference Document** under the header MASTERNODE PUBLIC ADDRESS.  

c. Go to the exchange you purchased your coins on and set up a withdrawal of **25,000** coins EXACTLY to the public address you just created in step 3.b.  **THIS IS VERY IMPORTANT, DO NOT SEND 25,100 COINS, ONLY SEND 25000 XAP TO MN1.  ALSO, YOU MUST DO THIS IN A SINGLE TRANSACTION.  YOU CAN NOT MAKE TWO 12,500 TRANSACTIONS TO THIS ADDRESS, THE SERVER WILL NOT REGISTER THIS AS A WALLET STAKE.  A SINGLE RECEIVING ADDRESS MUST ONLY HAVE ONE TRANSACTION OF 25,000 XAP SENT TO IT.  NEVER SEND ANY OTHER XAP TO THIS PUBLIC ADDRESS.  IF YOU WANT TO SEND ANY MORE XAP TO YOUR WALLET, CREATE A NEW ADDRESS.**

d. Wait for 16 confirmations.  To check the status of your transfer, go to your Desktop Wallet and go to **Transactions**.  Once you have initiated the transfer from the exchange, it will take a few minutes to show up in your wallet.  Once it has, you can double click on it and a 'Transaction details' pop-up will appear.  Under here, you will see **Status:** and it will tell you the number of confirmations that your transaction has achieved.  This pop-up does not update automatically, so close and reopen this pop-up until the number of confirmations is 16 or higher.   

e. Go to **Help -> "Debug Window - Console"**  

f. Enter the following command: **masternode outputs**

The result will look something like this:
```
{
    "b35aaf4fba02ea64c239ofasdoe8f55abc66cb26a8930656ad8020334bf871" : "1"
}

```
This value is your **TxHash** value that you will need in step 

g. Copy and paste this information into your **Apollon Masternode Reference Document**, under the header MASTERNODE OUTPUT.

h. Go to **Masternodes** tab.

i. Click **Create** and fill these details:  
* Alias: **MN1**  
* Address: **VPS_IP:PORT**  found under in your **Apollon Masternode Reference Document** under the header MASTERNODE SERVICE DETAILS 
* Privkey: **Masternode Private Key**  found under in your **Apollon Masternode Reference Document** under the header MASTERNODE SERVICE DETAILS 
* TxHash: **First value from 3.g**  also found under in your **Apollon Masternode Reference Document** under the header MASTERNODE OUTPUT
* Output index:  **Second value from 3.g, either a 1 or a 0**  also found under in your **Apollon Masternode Reference Document** under the header MASTERNODE OUTPUT
* Reward address: leave blank  
* Reward %: leave blank  

j. Click **OK** to add the masternode 

k. Click **Start All**  

l. Go back to your SSH session and enter the following command and hit enter. 
```
Apollond masternode status
```
Your status should now be 9 = Your masternode is remotely enabled and active.
```
{
    "vin" : "CTxIn(COutPoint(0000000000, 4294967295), coinbase )",
    "service" : "108.61.195.216:12116",
    "status" : 9,
    "pubKeyMasternode" : "APFxbJgmgxCbWYqUAJQv3TtKERVUQY6yvz",
    "notCapableReason" : "Could not find suitable coins!"
}
```

***

## CONGRATULATIONS

You've just setup an Apollon masternode and it is actively participating in the network.  Next we'll verify that it's communicating on the network.  

***


## 5. NETWORK VERIFICATION - 40 to 90 minutes
In order to ensure that the server is communicating with the network there are three things to check.  

**Linux server status**

**Wallet masternode status**

**Online list of XAP masternodes**

***

# a. Linux server status

i.  **Apollon masternode status** 

This is the most important command on the Linux server to ensure that your Masternode is running.  
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
There is only one important field here, **"status"**.

**Ensure that your Masternode 'status' is either 8 or 9.**

If your masternode's status is not 8 or 9, but is 2, the most likely culript is that it has not been remotely started from your wallet.  See 5.b.ii below for details on how to start your masternode remotely from your wallet.

Here is the list of possible Masternode statuses (note this is the same list as from above).

1 = Your masternode has not been processed by the network yet.  Please wait.

2 = Your masternode is active and synced to the network.

3 = Your masternode is inactive.

4 = Your masternode has stopped.

5 = Your masternode seed transaction hasn't reached the minimum of 16 confirmations.  

6 = Your masternode port is closed.

7 = Your masternode port is open.

8 = Your masternode is syncing to the network.

9 = Your masternode is remotely enabled and active.

**"pubKeyMasternode"**  Given the name of this field, one would expect that this field would be the one that is listed on the masternode public list here: [https://xap.overemo.com/masternodes](https://xap.overemo.com/masternodes).  It is not!  To look up your masternode on this list, use the public key you created in step 3.b  

**"notCapableReason"**  notCapableReason seems to store a text string from the last found error on the server.  With that said, if your status is 8 or 9 and you still see a strange error here, like the one above, ignore it.  It is likely just the last error that your server encountered before going live.

ii.  **Apollon getinfo** 
This is less important, but it will tell you if your server is incrementing blocks which will tell you that your server is in fact verifying transactions / blocks.  
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


Note: this is also the public address of your masternode that you will use to look up your masternode on [https://xap.overemo.com/masternodes](https://xap.overemo.com/masternodes).


# b. Wallet masternode status

i. **My Master Nodes.** 

* Open your wallet on windows

* Click on **Masternodes** in the left hand menu bar.

* Select **My Master Nodes** from the tabs.

* Your masternode should show up here. 

* Click on Update.

* Your masternode status should show as "Masternode is Running."

ii.  **Start**  If your masternode is not responding with a "status" of 2 from the command Apollon masternode status described above under 5.a.i, then you likely need to remote start your masternode from your wallet.  

* Select your masternode from this list

* Click the "Start" button.

* Go back to your ssh session and run the following command again:
```
Apollon masternode status
```
* Your status should return to 8 or 9.  If it doesn't please ask on the Discord channel for help.

iii. **Apollon Network**

* Select the **Apollon Network** tab.

* Click on the **Address** header in the list to sort the address by IP Address.

* Take note of your servers IP address under from the **Apollon Masternode Reference Document** under the header MASTERNODE SERVICE DETAILS

* Find your server by scrolling through the list.  If you find it, your server is operating properly on the network.  

* If you can't find it, wait 40 minutes and check again.  

* If you still can't find it, move onto the next step to check for your server.  

# c. Online list of XAP masternodes

i. Visit this link [XAP List and status of masternodes](https://xap.overemo.com/masternodes).

ii. Copy your MASTERNODE PUBLIC ADDRESS from your **Apollon Masternode Reference Document**.

iii. Perform a search in this webpage for your MASTERNODE PUBLIC ADDRESS.  

iv.  If you find it, you're good, your masternode is up and operating.  

v.  If you can't find it, go back to the top of this section and start again to make note of all of your server status and wallet status details and then go to the discord channel to ask for help. 


***


## 6. MAINTENANCE
There really isn't much maintenance to a masternode once it's up and running.  However, you will want to ensure that it's setup and running properly from time to time.  If you find that your network status has dropped off after going through the details in section 5 above, try these commands to test your linux server. 

a. **systemctl status Apollon**
This command will tell you if the masternode system running at all.  If it's not, you won't see your server on the network and the commands in section 5 wouldn't work at all.
```
systemctl status Apollon
```

b.  **systemctl is-enabled Apollon**
This last command will tell you if Apollon is configured to start on boot.  
```
systemctl is-enabled Apollon
```  

c. **systemctl start Apollon**
This command will start the masternode service for you if it has stopped running.  
```
systemctl start Apollon
```  

d. **systemctl stop Apollon**
This command will stop the masternode service for you if you need to take your server offline for any reason.  
```
systemctl start Apollon
```

***

## Conclusion

I hope this guide helped you sort out the building of a masternode for Apollon.  If you have any suggestions, comments or corrections, please send them to me on Discord, my username is fathertime100.  

Thanks and Best Regards, 

fathertime100



## Donations

All donations are highly appreciated!

**XAP**: AJutr9AbhkDvdRjazGZVt1w2xE4rzWqbzW

**BTC**: 3DWgfnbgZHrHBq2V7bfj2FeRSiWx9VE8X6

**LTC**: LfEMCmyEykszHXj1eLxgvH2HPRrsdajPcM

**DASH**: XfVFJM97zG4ckgm9aJ52PQ1Zc3NVtdAh2F


***
**Acknowledgements**
This guide was adopted from Zoldur's script and developed to support my video of this process on [Youtube](https://).  Thank-you Zoldur for doing most of the heavy lifting on this.  Also thank-you to the entire Apollon team on Discord who helped get through the weeds on setting this up.  Specifically, iTzShowTime, Visco, zoldur, XAP_MAX and MorpheusM, thanks team!
***
**DISCLAIMER:** The references contained herein are for information purposes only.  This guide is not intended to be investment advice.  Seek a duly licensed professional for investment advice before investing in any masternode.

