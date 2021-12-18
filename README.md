# Proof of Authority Development Chain

For this assignement I choose to accept it, will be to set up a testnet blockchain for my organization.

To do this, I will create and submit four deliverables:

* Set up your custom testnet blockchain.

* Send a test transaction.

* Create a repository.

* Write instructions on how to use the chain for the rest of my team.

## Background

I have just landed a new job at ZBank, a small, innovative bank that is interested in exploring what
blockchain technology can do for them and their customers.

My first project at the company is to set up a private testnet that me and my team of developers
can use to explore potentials for blockchain at ZBank.

You have decided on setting up a testnet because:

There is no real money involved, which will give my team of developers the freedom to experiment.

Testnets allows for offline development.

In order to set up a testnet, you will need to use the following skills/tools we learned in class:

* Puppeth, to generate my genesis block.

* Geth, a command-line tool, to create keys, initialize nodes, and connect the nodes together.

* The Clique Proof of Authority algorithm.

Tokens inherently have no value here, so we will provide pre-configured accounts and nodes for easy setup.

After creating the custom development chain, create documentation for others on how to start it using the pre-configured
nodes and accounts. I can name the network anything I want, have fun with it!




## **Instructions**

### **Setup the custom out-of-the-box blockchain**

1. Download the geth-alltools-windows-amd64-1.9.7-a718daa6 from internet, which will contain the different executables required for this exercise. Once downloaded, unzip. Using Git Bash, navigate to the folder where you have saved the geth-alltools-windows-amd64-1.9.7-a718daa6

2. Create a new project directory for new network. Here I have called it "week18homework"

3. Create a "Screenshots" folder inside of the project directory.

4. Create accounts for two (or more) nodes for the network with a separate `datadir` for each using `geth` as shown with the two commands. For now we will create two accounts, there will be instructions later to add more should we choose to. When setting the password, please don't forget it as it will be used later on when initialising the nodes and importing into MetaMask.

> ./geth --datadir week18homework/node1 account new

> ./geth --datadir week18homework/node2 account new

![create-nodes](Screenshots/initialiseNode1-2.jpg)




* You can simply run the code above to a create a new node should you need one.

5. Run `puppeth`, using './puppeth' in Git Bash I have named, and select the option to configure a new genesis block.





6. When asked about Creating a new genesis from scratch select 1. 

7. Choose the `Clique (Proof of Authority)` consensus algorithm.

8. When asked about how many seconds should blocks take, choose 1 for fastest and 15 for default. This is to specify how long the nodes will take to generate a block.

9. When asked about the accounts are allowed to seal, paste both account addresses from the first step four at a time into the list of accounts to seal. Be careful with the prompt already provide '0x', don't just simply copy and paste the account.

10. Paste them again in the list of accounts to pre-fund. I have decided to use only Node1 as pre-funded so that I can see other accounts having ETH in it start with zero balance.

11. You can choose `no` for pre-funding the pre-compiled accounts (0x1 .. 0xff) with wei. This keeps the genesis cleaner.

12. Whenasked about Specify your chain/network ID, enter `334`, and when you are back at the main menu, choose the "Manage existing genesis" option.

13. Export genesis configurations. This will fail to create two of the files, but you only need `week18homework.json`.

* Delete/ignore the `week18homework-harmony.json` file. You should have something similar to below:

![puppeth](Screenshots/create_json.jpg)


14. Initialize each node with the new `week18homework.json` with `geth` using the following commands, one per node:

> ./geth --datadir week18homework/node1 init week18homework.json

> ./geth --datadir week18homework/node2 init week18homework.json

* You should see a similar output to below:

![geth-initialise](Screenshots/initialNodes1-2.jpg)




15. In one Git Bash window, run the first node, unlock the account, enable mining, and the RPC flag. Only one node needs RPC enabled using the following command:

> ./geth --datadir week18homework/node1 --unlock "0xAca3A379CBE51B955cc158274bF33caC30FfFf0C" --mine --rpc --allow-insecure-unlock

* "" refers to the Node1 account created in step 4. You should see an output similar to below:


![node1-initialise](Screenshots/node1_running.jpg)




16. In another Git Bash window, set a different peer port for the second node `(30304)` and use the first node's `enode` address as the `bootnode` flag by using the following command:

> ./geth --datadir week18homework/node2 --unlock "0x3525Bb83b6a3F3393F47678a9bc42c359a66692D" --mine --port 30304 --bootnodes "enode://4f8ade57ff7044ea9b40f93d483b3a983c683f87d283feb932f43c387819a3813324f99024eb1fd61ea729b23472e3e6eeac5a966b09aac208bc51bb03f66fff@127.0.0.1:30303" --ipcdisable --allow-insecure-unlock

* "" after --unlock should be replaced with the Node2 account create in step 4. "" after `--bootnodes` should be populated from the output info from Node 1 initialise, use green `self` node where it starts with `enode` as value from Node 1 from your first Git Bash window.


![node1-initialise](Screenshots/node2_running.jpg)




* make sure you have entered password for both nodes as it will not create any blocks if password you have created in Step 4 was not entered in the initialisation of Node 1 and 2.

* a functioning Node 1 and Node 2 should be constantly looking for blocks to mine and should similar to below:

![node1-initialise](Screenshots/functioning_node_1-2.jpg)


### **Send a test transaction**

17. Open MetaMask in your google extension. Navigate to `Settings` from the circle icon on the top right hand corner such as below:

![node1-initialise](Screenshots/importAccount.jpg)

18. You will need to use a custom network, and include the chain ID, and use ETH as the currency. Please make sure that the Chain ID is the same as you have entered in step 4 when you're creating the genesis. Here I have used `334`.

![node1-initialise](Screenshots/mm_network.jpg)



19. Once done with setting up network. Using the similar method to Step 17, navigate to `Import Account`. Here you will navigate to  `node1/keystore` directory and select the file that starts with "UTC". This will import the private key, give it some time as it may appear in MetaMask that nothing is working. Do this for both nodes 1 and 2.

![node1-initialise](Screenshots/importAccountJSON.jpg)


20. Send a transaction from the `node1` account to the `node2` account in MetaMask by clicking on `Send`. If you have set this up correctly as per instructions, you should have Node1 populated with a huge amount ETH. Use Node1 to start a transaction.

![node1-initialise](Screenshots/send.jpg)



21. Once `Send` is pressed, there will be options to move coins to other accounts, you can simply choose the option of transferring between accounts or paste the Account '0x' into recipient field as per below, enter the amount and press `Next`

![node1-initialise](Screenshots/sending.jpg)

22. You will be returned to the account view and under the `Activity` tab, you will see your transaction sitting in Pending mode will your miner do their thing. Click on the transaction that you have just created, it everything is working, it should look something like below:

![node1-initialise](Screenshots/txn1-2.jpg)    

* If there's no movement of coins, terminate the two nodes by pressing `Control + C`, rerun the commands for both Nodes 1 and 2 paying special attention to node 2 that it may require an extra nudge by pressing `Enter` after the Node 2 have started up in Git Bash. Once there's a scrolling prompt of block numbers incrementally being mined, it's an clear indication that all systems are working.





### **Send test transaction from a different node**

23. If you haven't done so, go back to step 4 and create another account. This is to create a new Account called Node 3, without opening a new Git Bash, you can now import the account into Metamask using step 19. Now you're ready to transfer coins into another node. Repeat steps 20 to 22 making sure you select the new account in the recipient. Here is an example from Node 1 -> 3:


![node1-initialise](Screenshots/txn1-3.jpg)    


* Here is an example of Node 3 -> 1:

![node1-initialise](Screenshots/txn3-1.jpg)    


* Here is an example of Node 2 -> 3:

![node1-initialise](Screenshots/txn2-3.jpg)    


* Celebrate, you just created a blockchain and sent a transaction into different nodes!



### **References**

**Node 1:**
Public address of the key:   `0xAca3A379CBE51B955cc158274bF33caC30FfFf0C`
Path of the secret key file: week18homework\node1\keystore\UTC--2021-12-18T09-56-42.277203900Z--aca3a379cbe51b955cc158274bf33cac30ffff0c


**Node 2:**
Public address of the key:   `0x3525Bb83b6a3F3393F47678a9bc42c359a66692D`
Path of the secret key file: week18homework\node2\keystore\UTC--2021-12-18T09-57-02.060417300Z--3525bb83b6a3f3393f47678a9bc42c359a66692d


**Node 3:**
Public address of the key:   `0x19134b14dcaF9Cf67c24624866772dF236d2F28a`
Path of the secret key file: week18homework\node3\keystore\UTC--2021-12-18T10-24-54.444529100Z--19134b14dcaf9cf67c24624866772df236d2f28a


## **Message will now self terminate**

