# olatunjivotingapp Please watch the video at https://www.youtube.com/watch?v=HBhDsyqLfzo

#Prerequisites (Local)

If you want to run this pattern locally, without any Cloud services, then all you need is VSCode and the IBM Blockchain Platform extension.

Install VSCode version 1.39,

Install IBM Blockchain Platform Extension for VSCode,

please install Node v8.9.0,

npm v6.11.3,

Note: you will need Node 8.x to run this pattern,

please install nvm on your windows laptop ,in order to install specific verison of node on your system

For example, to install and run Node 8.9.0:

  nvm install 8.9.0
  nvm use 8.9.0

Windows: Docker Desktop 2.0+ on Windows 10 Pro/Enterprise. Windows 10 Home (2004+) requires Docker Desktop 2.3+ and the WSL 2 back-end. (Docker Toolbox is not supported. Windows container images are not supported.)
macOS: Docker Desktop 2.0+.
Linux: Docker CE/EE 18.06+ and Docker Compose 1.21+. (The Ubuntu snap package is not supported.)
You can check your installed versions by running the following commands from a terminal:

node --version
npm --version
docker --version
docker-compose --version


Open the vscode,and click on the extension tab or type CTRL + SHIFT + X ,This will take you to the market place,type in ibm blockchain chain platform ,Wclick on the platform and wait for the installation(in the video,i have installed the platform already).
click on the square shaped icon ,this is the symbol for the ibm blockchain platform .For Windows laptop users, you must download vscode version 1.39,this is because versions higher than 1.39 cannot activate the ibm blockchain platform but this problem is not seen among linux laptop users(i am using a linux laptop).As a workaround for this problem please visit this forum for solution  https://github.com/IBM-Blockchain/blockchain-vscode-extension/issues/1205 and  https://github.com/IBM-Blockchain/blockchain-vscode-extension/issues/1364

# Create a fair evoting application to ensure correct election results with Hyperledger Fabric and IBM Blockchain Platform (Local)

# Steps (Local Deployment)

1. [Clone the Repo](#step-1-clone-the-repo)
2. [Start the Fabric Runtime](#step-2-start-the-fabric-runtime)
3. [Install Contract](#step-3-install-contract)
4. [Instantiate Contract](#step-4-Instantiate-contract)
5. [Export Connection Details](#step-5-export-connection-details)
6. [Run the App](#step-5-run-the-app)

Note: This repo assumes you have [VSCode](https://code.visualstudio.com/download) 
and [IBM Blockchain VSCode extension](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform) installed. If you don't, first install the 
latest version of VSCode, and then install the IBM Blockchain VSCode extension ensuring you 
have the correct [system requirements](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform) to run the extension. You will need Docker as 
this is how the extension builds a development Hyperledger Fabric network with a click of a button.

## Step 1. Clone the Repo
PLEASE USE A STRONG INTERNET CONNECTION ,FOR THE VSCODE TO WORK EFFECTIVELY
Git clone this repo onto your computer in the destination of your choice, then go into the web-app folder:
```
https://github.com/daporaphael/olatunjivotingapp        to do this you must have git installed.
```

## Step 2. Start the Fabric Runtime
![startRuntime]

- If you get errors like the gRPC error, you may need to download an earlier version of VSCode (1.39) (here)[https://code.visualstudio.com/updates/v1_39]. Note that if you are using Mac, make sure the VSCode in your ~/Applications
folder shows version 1.39 when you click on show details. You may need to 
move newer version into the trash, and then empty the trash for the older 
version to work.
 First, we need to go to our IBM Blockchain Extension. Click on the IBM Blockchain icon
  in the left side of VSCode (It looks like a square). 
- Next, start your local fabric by clicking on 
  *1 Org Local Fabric* in the **FABRIC ENVIRONMENTS** pane.
  
- Once the runtime is finished starting (this might take a couple of minutes), under *Local Fabric* you should see *Smart Contracts* and a section for both *installed* and *instantiated*.

## Step 3. Import Install and Instantiate Contract
🚨🚨🚨]
- Next, we have to import our contract before we can install it. Click on 
**View -> Command Palette... -> >IBM Blockchain Platform: Import a Package**. Next, click 
 on Browse and then select the `voterContract@7.0.0.cds` file that is in the ` olatunjivotingapp/contract folder. 

- Under **FABRIC ENVIRONMENTS** let's click on *+ Install* and choose the peer that is available. Then the extension will ask you which package to 
 install. Choose *voterContract@7.0.0.cds*.
- Lastly, we need to instantiate the contract to be able to submit transactions 
on our network. Click on *+ Instantiate* and then choose *voterContract@7.0.0.cds*.
- When promted for a function enter init, as shown below.
![packageFile](/docs/function.png)

- Now, for any additional information asked, just hit `enter` on your keyboard, which will take all of the defaults.
- This will instantiate the smart contract. This may take some time. You should see the contract under the *instantiated* tab on the left-hand side, once it 
is finished instantiating.

## Step 4. Export Connection Details
![export]
- Click on `1 Org Local Fabric- Org1` in the **FABRIC GATEWAYS** pane and then select `admin`. **This will choose the `admin` identity to connect with the gateway.**

- Right click on the 3 dot menu on the **FABRIC GATEWAYS** pane and `Export Connection Profile`. Save this file to  olatunjivotingapp/web-app/server/fabric_connection.json. **Note that this gif is here to show you the VSCode interface, it saves the files somewhere else.**


## Step 5. Export Local Wallet

![wallet]

- 🚨Under the `FABRIC WALLETS` pane, right click on `1 Org Local Fabric - Org1`. Note this is very important, if you click on the Orderer wallet at the top, 
the application will not work! 🚨
- Export Wallet and save it as `wallet` to 
olatunjivotingapp/web-app/server/wallet.
- Your final directory structure should look like the image below:
![dirStruct]
#### Update Config

Next, update the `config.json` file so it looks like this:

```json
{
  "connection_file": "fabric_connection.json",
  "appAdmin": "admin",
  "appAdminSecret": "adminpw",
  "orgMSPID": "Org1MSP",
  "caName": "ca.org1.example.com",
  "userName": "V1",
  "gatewayDiscovery": { "enabled": true, "asLocalhost": true }
}
```
This will ensure we use the admin identity that is stored in our wallet to sign transactions, 
and let the network know that the transactions that are coming from this certain identity were 
first signed off by the certificate authority with the name `ca.org1.example.com`.

## Step 6. Run the App
To run the app, we will need to install dependencies for both our front-end and our back-end. 

#### Start the Server
  - First, navigate to the `server` directory, and install the node dependencies.
    ```bash
    cd olatunjivotingapp/web-app/server/
    npm install
    ```
  - Then, start the server: 
    ```bash
    npm start
    ```
  - If all goes well, you should see the following in your terminal:
    ```
     > server@1.0.0 start /home/olatunji/olatunjivotingapp/web-app/server
    > ./node_modules/nodemon/bin/nodemon.js src/app.js

      [nodemon] 1.19.1
      [nodemon] to restart at any time, enter `rs`
      [nodemon] watching: *.*
      [nodemon] starting `node src/app.js`  
    ```

#### Start the Front-end (Client)

- First, navigate to the `client` directory, and install the node dependencies.
  ```bash
  cd olatunjivotingapp/web-app/client/
  npm install
  ```
- Then, start the client: 
  ```bash
  npm run serve
  ```
- If all goes well, you should see the following in your terminal:
  ```
      DONE  Compiled successfully in 6803ms                                                                                             11:48:20

      App running at:
      - Local:   http://localhost:8080/ 
      - Network: unavailable

      Note that the development build is not optimized.
      To create a production build, run npm run build. 
  ```

 Nice. We're pretty much ready to submit transactions on our contract. Go to http://localhost:8080/ 
 to see your app.

 Go ahead and register a voter, login with your VoterId, and submit a vote. Have fun! :) 


# COMMON ERRORS AND HEIR SOLUTIONS

The truth and the reality is that you cant do without errors when it comes to coding,but the GOOD NEWS is that there is always a workaround to those errors.
(1) Internal watch failed: watch /home/olatunji/olatunjicontract/web-app/server ENOSPC
the above error occur because max ports were not configured correctly,to solve this on an Ubuntu laptop because I am using an Ubuntu Laptop,RUN THIS CODE



echo fs.inotify.max_user_watches=582222 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p


(2)Error: ENOENT: no such file or directory, open '/home/olatunji/olatunjicontract/web-app/server/ibpConnection.json'

 Please update the config.json file ,there is a mismatch in the file and the vscode connection profile that you exported earlier.
ibpConnection.json is the connection profile  for ibm cloud users but for vscode users ,the connection profile is 
fabric_connection.json

 update the `config.json` file so it looks like this:


{
  "connection_file": "fabric_connection.json",
  "appAdmin": "admin",
  "appAdminSecret": "adminpw",
  "orgMSPID": "Org1MSP",
  "caName": "ca.org1.example.com",
  "userName": "V1",
  "gatewayDiscovery": { "enabled": true, "asLocalhost": true }
}



(3) 
 



