# Setting up Local Blockchain Network for Ethereum using Geth

#### Prerequisites for setting up private blockchain

* Geth: This is useful in setting up the private network.

  * For installing Geth, follow the steps which are described here for MacOS only. For other OS related installation, refer to this page.

    > brew install geth

    Once Geth is installed, try to run command `geth --help`, just to make sure it has installed without any errors.

* Truffle: This is a utility tool for creating smart contracts and deploying it on the network with specific configurations.

  * For installing Truffle, follw the steps which are described as below. Before installing truffle, make sure NPM is available. If not, try to install Node.Js from the official website.

    >npm install truffle

    Once truffle is installed, try to run command `truffle --help`, to make sure it has installed without any errors.

* Solidity: This is a programming languge on which smart contracts are coded. So, we need this compiler for compiling our smart contracts written with the help of Truffle.

  * For installing Solidity, follow the steps which are described as below.

    > npm install solc

* Web3: This is utility component for having interactions between blockchain and the UI. For this exercise, we can skip this as we are not developing any UI here.

  * For installing Web3, follow the steps which are described as below.

    > npm install web3

```Note: Try avoiding to install npm packages with -g flag, which sometimes creates problem with node-gyp. Just to play a safe game, avoid using it.```

---

#### Setting up Local Network using Geth

To setup the Ethereum local network using Geth, follow the steps described below.

* First things first, we need a `genesis` block for the network with some of the initial configuration on top of which the network would be created.

* The below content is the configuration of the genesis block.

  `newGenesis.json`

  ```json
  {
    "config": {
      "chainId": 123,
      "homesteadBlock": 0,
      "eip150Block": 0,
      "eip150Hash": "0x0000000000000000000000000000000000000000000000000000000000000000",
      "eip155Block": 0,
      "eip158Block": 0,
      "byzantiumBlock": 0,
      "constantinopleBlock": 0,
      "petersburgBlock": 0,
      "ethash": {}
    },
    "nonce": "0x0",
    "extraData": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "gasLimit": "0",
    "difficulty": "0x80000",
    "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "coinbase": "0x0000000000000000000000000000000000000000",
    "alloc": {
    },
    "number": "0x0",
    "gasUsed": "0x0",
    "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000"
  }
  ```

* Once this is done, create a separate folder for our network and accounts like below. For now, we will restrict our network to two nodes only. 

  > mkdir EthTestNet1
  >
  > mkdir EthTestNet1/Account1
  >
  > mkdir EthTestNet1/Account2

* Next, to create a network with the `newGenesis.json` as the initial block, execute the series of commands as described below.

  > geth --datadir EthTestNet1/Account1 init <PathToJsonFileCreated>
  >
  > geth --datadir EthTestNet1/Account1  --networkid 123451  --maxpeers 10 --rpcaddr 127.0.0.1 --rpcport 11111 console --rpc --allow-insecure-unlock --port 33000

  With the above commands you should be able to successfully create network without any errors. Now, you will be prompted to the console. Here, you need to create an account as every node should be associated with a secure address in order to interact with the network.

  > personal.addAccount()

  It will prompt for the password. Make sure you remember the password for future use. 

  Once the account is created, it will have default of `0 Ether`. In order to generate Ether, you need to start mining. Before executing this command make sure you have better RAM capacity a sometimes it may not even start and the system gets slow.

  > miner.start()

  To check how much ether is created, type the following command.

  > web3.fromWei(eth.getBalance(eth.coinbase), "ether")

  If you feel that you have got enough ether(depends on you), then stop mining with the following command.

  > miner.stop()

  Now, open another terminal or cmd and do the same steps as done for setting up the first node under EthTestNet1/Account2.

* Our next goal is to connect two nodes which are already running. for that, follow the steps described below.

  * At first we need to find the enode value of either of nodes. To know that, execute the following command.

    > admin.nodeInfor.enode

    It should look something like this.

    > enode://de86db3a6wer30bb0d123f3df1fdd725be70e34d3ef8ee8205a8f38c5204ddf4fc29430f0e9af701f74a5da0efawhbee68f13a1e2bb02e45f1140erergay02289abb9@10.22.29.158:30303

    There are three parts to this. 

    * `enode://......@` --> Hexadecimal representation of the node ID.
    * `@....:` --> This is the IP address or hostname
    * Final part is the port number on which this Node is running.

    There are two cases her for connecting.

    * Connecting two nodes which are on same system: For this case, replace the IP address with `127.0.0.1`
    * Connecting two nodes which are on same network: For this case, we need execute the `geth` command for setting up the network as described above with `--rpcaddr <IP>`. 

    `Note: --rpc flag in the geth command is very important for having external connections to the network`

    ##### How do we connect?

    Now we know the Enode of the two different machines. Follow the steps in either of the consoles to make the connection.

    > admin.addPeer("enode://de86db3a6wer30bb0d123f3df1fdd725be70e34d3ef8ee8205a8f38c5204ddf4fc29430f0e9af701f74a5da0efawhbee68f13a1e2bb02e45f1140erergay02289abb9@127.0.0.1:30303")

    This statement always returns true no matter what happens. So, just to make sure that the two nodes are connected, run the command `admin.peers`. 

    If you are able to see the information, then VOILA! You have successfully setup the local network. Now you can extend to multiple nodes.

    The possible reasons why you are unable to connect the nodes:

    * Mismatch with the `--rpcaddr`.
    * Problem with `enode` value to which you are connecting to i.e. w.r.t IP address. If we are connecting locally use 127.0.0.1 or else with the machine IP.
    * Mismatch with the genesis node i.e. the JSON the string which we have discussed above, for the two nodes.

  * So, we are done with setting up the network. But before going for deploying smart contract on the network, we need to unlock the account from which we are going to deploy the smart contact.

    To unlock the account use the following command.

    > personal.unlockAccount(web3.eth.coinbase, <AccPassword>)

    

---

#### Setting up Truffle workspace

For setting up the workspace follow the instructions below. I prefer VSCode for development purpose as this is very good extensions/pacakges for handling the code.

* Create a new workspace directory and a project directory

  > mkdir truffle-workspace/hello-world

  

* Navigate to the directory created.

  > cd truffle-workspace/hello-world

  

* Now, get the boilerplate code from truffle by executing following command.

  > truffle init

* Once done with generating boilerplate code, your project folder should look like this.

  > |---- hello-world
  >
  > ​    |---- contracts (contains smart contracts)
  >
  > ​        |-- Migrations.js
  >
  > ​    |---- migrations (tells which smart contract to deploy)
  >
  > ​        |-- 1_initial_migrations.js
  >
  > ​    |---- test
  >
  > ​    |-- truffle-config.js (useful for setting up the network settings)

  

* Now, delete Migrations.js and 1_initial_migrations.js files.

* Add the below two files with the same names to the specific folders as described.

  `contracts/HelloWorld.sol`

  ```solidity
  pragma solidity ^0.5.6;
  
  contract HelloWorld{
      function getMessage() public pure returns(string memory){
          return "Hello World";
      }
  }
  ```

  `migrations/1_helloworld.js`

  ````javascript
  const HelloWorld = artifacts.require("HelloWorld");
  
  module.exports = function(deployer) {
    deployer.deploy(HelloWorld);
  };
  ````

* Once this is done, we need to setup the configuration of the network on which we are going to deploy this smart contract. Please note down the `rpcport` and `networkid` number for which the account was locked.

  Edit the `truffle-config.js` (for MacOS) under network object with the following code.

  ``````json
      development: {
       host: "127.0.0.1",     // Localhost (default: none)
       port: "<RPC_PORT>",            // Standard Ethereum port (default: none)
       network_id: "NETWORK_ID",       // Any network (default: none)
       gas: "4000000"
      },
  ``````

  Once the configuration is edited, try to make a dry run with the following command to make sure that all the criterias like GAS limit, GAS price, etc are sufficient. 

  > truffle migrate --dry-run --network development

  If this is successful, then we are good to go with the deployment with the specified configuration. So, run the following command to deploy it to the network which is created with the below command.

  > truffle migrate --network development

  With this command, the console waits for the confirmation from the network. If the at least one miner is up, the trasaction would be complete or else it would be in the tracsaction pool. In order to see which all trasactions are in the pool, run this command in one of the console attached to the network.

  >  txpool

  This lists the transactions available in the network.

  In order to complete the transactions, we need a miner. So, execute `miner.start()` command from any of the consoles. Wait for sometime, then the transaction will be complete and the truffle console displays the result of the transaction. Now, the transaction pool would be empty. 

  Make sure to stop the mining with `miner.stop()`, or else it will eat up you system.

  The possible reasons for failure of deployment are:

  * Problem with the network configuration in the `truffle-congfig.js` file.
  * Network is not up.
  * Problem with the GAS. For this, you need to start mining in the account from which you want to deploy the smart contract. 
    * This is necessary as each and every transaction is associated with the GAS price and GAS limit.



Hope you are now able to play around with adding more nodes and deploying smart contacts. Enjoy the Local Network of Ethereum!!



Resources:

* https://ethereum.stackexchange.com/questions/6373/connecting-2-nodes-on-different-machines-on-different-network-from-terminal-in-e
* https://ethereum.stackexchange.com/questions/10681/what-are-ipc-and-rpc - This was very helpful in understanding setting up the network with specific IP's.
* https://medium.com/blockchainbistro/set-up-a-private-ethereum-blockchain-and-deploy-your-first-solidity-smart-contract-on-the-caa8334c343d - More infromation about the configuration variables used throughout the setup.
* https://geth.ethereum.org/interface/Command-Line-Options - For understanding geth command and its usage.

