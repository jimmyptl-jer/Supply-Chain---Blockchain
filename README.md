# Supply-Chain---Blockchain

Step by Step guide to implemenet the hyperledger fabric:

**Step 1: Set up the Prerequisites**

- Operating system (Ubuntu Linux 14.04 / 16.04 LTS or Mac OS 10.12)
- Install the required software:
   Docker Engine (version 17.03 or higher)
          $ sudo apt install docker.io
          

   Docker Compose (version 1.8 or higher)
          $ sudo apt install docker-compose
  
   Node.js (version 8.9 or higher)
          $ sudo apt install nodejs
          $ node -v or node –version
   Go
          $ sudo apt-get update
          $ sudo apt-get install golang-go
  
  npm (version 5.x)
         $ sudo apt-get update
         $ sudo apt-get upgrade
  Git (version 2.9.x or higher)
         $ sudo apt-get update
         $ sudo apt-get install git
         $ git --version

  Python (version 2.7.x)
        $ sudo apt-get install python-software-properties

**Step 2: Clone the fabric-samples Repository**

- Clone the fabric-samples repository from GitHub:
   git clone https://github.com/hyperledger/fabric-samples.git
   cd fabric-samples

- Install Hyperledger Fabric Binaries and Docker Images:
  curl -sSL https://raw.githubusercontent.com/hyperledger/fabric/main/scripts/install-fabric.sh | bash -s -- docker samples binary

- Navigate to the fabric-samples/test-network directory:
  cd fabric-samples/test-network

- Generate Crypto Material:
  ../bin/cryptogen generate --config=./crypto-config.yaml

- Generate the channel artifacts:
   ./network.sh generateArtifacts

- Start the Network:
  ./network.sh up

- Create and Join Channels:
  ./network.sh createChannel
  ./network.sh joinChannel

- Deploy and Instantiate Chaincode:
  ./network.sh deployCC -ccn <chaincode-name> -ccp <chaincode-path> -ccl <chaincode-language>

- To shut down the network: 
  ./network.sh down

Sure, I'd be happy to help you with the next steps after deployment in Hyperledger Fabric.


**After deploying your network and smart contracts, the next steps typically involve interacting with the network through client applications. These applications can be developed using various programming languages such as Node.js, Java, Go, etc. For this example, we'll provide a simple guide using Node.js as the programming language.**

Step 1: Set up the development environment

1. Make sure you have Node.js and npm (Node Package Manager) installed on your system. You can install them using the following commands:

```bash
sudo apt-get update
sudo apt-get install nodejs
sudo apt-get install npm
```

2. Verify the installations:

```bash
node -v
npm -v
```

Step 2: Create a new directory for your client application:

```bash
mkdir fabric-client-app
cd fabric-client-app
```

Step 3: Initialize the Node.js project:

```bash
npm init -y
```

Step 4: Install the required dependencies:

For interacting with the Hyperledger Fabric network, you'll need the Fabric SDK for Node.js. Additionally, you may need other dependencies based on the requirements of your client application.

```bash
npm install fabric-network
npm install fabric-ca-client
npm install dotenv
# Add other dependencies if needed
```

Step 5: Create a `.env` file in the root of your project to store environment variables:

```bash
touch .env
```

Edit the `.env` file and add the necessary environment variables for your network configuration, such as connection profile path, wallet path, user credentials, etc.

Example `.env` content:

```plaintext
NETWORK_CONNECTION_PROFILE=./path/to/connection-profile.json
WALLET_PATH=./path/to/wallet
USER_NAME=user1
USER_SECRET=user1pw
CHANNEL_NAME=mychannel
CONTRACT_NAME=mycontract
```

Step 6: Create your client application code:

In this step, you will write the Node.js code to interact with the deployed smart contract on the Hyperledger Fabric network. The exact code will depend on the functionality you want to implement, such as submitting transactions, querying data, etc.

Here's a simple example of a Node.js application to submit a transaction to the smart contract:

```javascript
const { Gateway, Wallets } = require('fabric-network');
const path = require('path');
const fs = require('fs');

async function main() {
  try {
    const ccpPath = path.resolve(__dirname, process.env.NETWORK_CONNECTION_PROFILE);
    const walletPath = path.resolve(__dirname, process.env.WALLET_PATH);

    const ccp = JSON.parse(fs.readFileSync(ccpPath, 'utf8'));
    const wallet = await Wallets.newFileSystemWallet(walletPath);

    const gateway = new Gateway();
    await gateway.connect(ccp, {
      wallet,
      identity: process.env.USER_NAME,
      discovery: { enabled: true, asLocalhost: true }
    });

    const network = await gateway.getNetwork(process.env.CHANNEL_NAME);
    const contract = network.getContract(process.env.CONTRACT_NAME);

    // Submit a transaction to the smart contract
    await contract.submitTransaction('TransactionFunction', 'arg1', 'arg2');

    console.log('Transaction submitted successfully.');

    await gateway.disconnect();
  } catch (error) {
    console.error(`Error submitting transaction: ${error}`);
    process.exit(1);
  }
}

main();
```

Replace `'TransactionFunction'`, `'arg1'`, and `'arg2'` with the actual function name and arguments of your smart contract that you want to invoke.

Step 7: Run the client application:

```bash
node app.js
```

This will execute your client application, and it will interact with the deployed smart contract on the Hyperledger Fabric network.

Please note that this is just a simple example, and in a real-world scenario, you may need to implement more complex client applications based on your specific use case and requirements.

Keep in mind that the specific details and code may vary depending on your network configuration and smart contract implementation. Make sure to adapt the code to match your specific use case and configurations.

Always ensure that you have appropriate security measures in place and handle sensitive information, such as user credentials, carefully in your client application.
