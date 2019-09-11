## Setting up private blockchain network for Ethereum

####Prerequisites for setting up private blockchain

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

#### Setting up Truffle workspace

For setting up the workspace follow the instructions below. I prefer VSCode for development purpose as this is very good extensions/pacakges for handling the code.

* Create a new workspace directory and a project directory

  > mkdir truffle-workspace/hello-world

  

* Navigate to the directory created.

  > cd truffle-workspace/hello-world

  

* Now, get the boilerplate code from truffle by executing following command.

  > truffle init

* 