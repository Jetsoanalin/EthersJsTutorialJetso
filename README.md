# EthersJs Usage Tutorial by Jetso

1st Step for checking all the connections to Blockchain network follow these Steps:

1-Load a application with ethersJS loaded in it 
script for HTML - 
```
 <script charset="utf-8"
            src="https://cdn.ethers.io/scripts/ethers-v4.min.js" type="text/javascript">
        </script>
 ```
 For NodeJS, ReactJS, Vue etc etc there are different way to import in respective applications so read NPM documentations for that :
 
 https://www.npmjs.com/package/ethers
 
 
 2- Check the connections by loading the application in browser <br>
 3- Open browser with application loaded press F12 go to Console <br>
 3- enter these 3 commands one by one and press enter after each command : <br>
 ```
 ethereum 
 web3
 ethers
 ```
 4- Once everything displays a sucess message we are ready to interact with Ethereum Blockchain Network <br>
 
 
 ## Starting with Contract and Test Networks :
 
 Step 1: Go to remix.ethereum.org
 Step 2: Create a new Solidity File by clicking on the plus option near browser top left
 Step 3: Give your file a new name- hello.sol
 Step 4: Write your contract- eg: https://github.com/jetsoanalin/collegedemo
 
 ```
pragma solidity ^0.5.11;
contract MyContract {
    function get() public view returns(string memory) {
    return value;
}
function set(string memory _value) public {
    value = _value;
}

}

 ```

Step 5: Go to the left pannel in remix IDE and at below plugin option Activate 2 plugin Solidity Compiler and Deploy & Run <br>
Step 6: Go to compile and select the correct solidity version written in the contract <br>
Step 7: Compile the contract <br>
Step 8: After compiling below you will find Contract Dropdown - Select your Contract name and copy the ABI file by clicking on the ABI below it at the end (This will be required for Integration so save it somewhere) <br>
Step 9: Connect your wallet to Metamask (If not having metamask then install metamask and load a wallet in it with test ethers)<br>
Step 10: For Test etheres in Ropsten Network go to https://faucet.ropsten.be/  or  https://faucet.metamask.io/  you will recieve your test ethers (Take only 1 test ethers for 24 hours or else you will be grey listed)<br>
Step 11: Once you are connected to metamask with test ethers and your network selected from main network to test network click on deploy and run<br>
Step 12: In the Environment click change from JavaVM to Web3Injected your metamask will popup confirm it.<br>
Step 13: Once you are connected your address and balance will be displayed.<br>
Step 14: Click on Deploy a Metamask popup will be displayed confirm the transaction.<br>
Step 15: Once your Transaction is sucessfully completed your Contract will be deployed<br>
Step 16: Once its deployed you will see your contract below in left deploy and run pannel with its address and (blockchain) written which means its deployed in blockchain- Click on copy button and copy the address you will need it for frontend integration.<br>


## Creating a Wallet Instance for Signing Contract:
```
let provider = new ethers.getDefaultProvider('rinkeby'); //For Different Testnets <br>

or

let provider = ethers.getDefaultProvider(); //For Mainnet <br>
```

#### To Connect uing Metamask:
    
    (async() => {

        if(window.ethereum) {

            await window.ethereum.enable();
            const onCorrectNetwork = window.web3.currentProvider.networkVersion === "4"; //Rinkeby Network ID
            console.log("onCorrectNetwork", onCorrectNetwork);
            if(!onCorrectNetwork) {
                // (Display Text) "Connected to Metamask! You are on different network in MetaMask. Please select Rinkeby";

            } else {
                await new Promise(function(resolve, reject) {
                    const intervalId = setInterval(() => {
                        const metamaskWeb3Provider = new ethers.providers.Web3Provider(window.web3.currentProvider);
                        window.wallet = metamaskWeb3Provider.getSigner();

                        if(window.wallet.provider._web3Provider.selectedAddress) {
                            localStorage.walletAddress = wallet.provider._web3Provider.selectedAddress; //Display the Address
                            resolve();
                        }
                    },500);           
                });
                
                window.connectedToMetamask = true;
                window.intervalId = await setInterval(() => {
                    window.wallet.address = wallet.provider._web3Provider.selectedAddress;
                    showNextButton = true;
                }, 500);
                localStorage.address = window.wallet.address;
            }
        } else {
            // (Display Message) "Metamask not found";
        }
    })();





##### Creating Wallet: <br>

Step 1- Create UI only for Mnemonics. <br>
Step 2- Take password as input while creating a wallet with mnemonics. <br>
Step 3- This will generate wallet with mnemonics and keystore where keystore will only contain first address and mnemonics will have multiple. <br>
Step 4- Save the mnemonics (for your Mnemonic) and json (Save it as .json file).

```
let mnemonics = ethers.Wallet.createRandom().mnemonic;

let wallet = ethers.Wallet.fromMnemonic("Paste your Mnemonics here");

let password = "Your Password here";

function callback(progress) {
    console.log("Encrypting: " + parseInt(progress * 100) + "% complete");
}

let encryptPromise = wallet.encrypt(password, callback);

encryptPromise.then(function(json) {
    console.log(json);
});
```



### Loading Wallet :

##### Using Private Key - <br>
```
let privatekey = "place your Private Key here";
let wallet = new ethers.Wallet(privatekey,provider); 
```

##### Using Mnemonics - <br>
```
Creating Random Mnemonics :
ethers.Wallet.createRandom().mnemonic;

Loading From Mnemonics :
let wallet = ethers.Wallet.fromMnemonic("Paste your Mnemonics here");

Loading Your second wallet and other multiple wallets :
var path = "m/44'/60'/0'/0/1"; //this will load the 2nd wallet
or
var path = "m/44'/60'/0'/0/2"; //this will load the 3rd wallet
(You can increase the end number in path for multiple wallet Address for your Mnemonics)


```

##### Using KeyStore - <br>
```
let data = {
    id: "fb1280c0-d646-4e40-9550-7026b1be504a",
    address: "88a5c2d9919e46f883eb62f7b8dd9d0cc45bc290",
    Crypto: {
        kdfparams: {
            dklen: 32,
            p: 1,
            salt: "bbfa53547e3e3bfcc9786a2cbef8504a5031d82734ecef02153e29daeed658fd",
            r: 8,
            n: 262144
        },
        kdf: "scrypt",
        ciphertext: "10adcc8bcaf49474c6710460e0dc974331f71ee4c7baa7314b4a23d25fd6c406",
        mac: "1cf53b5ae8d75f8c037b453e7c3c61b010225d916768a6b145adf5cf9cb3a703",
        cipher: "aes-128-ctr",
        cipherparams: {
            iv: "1dcdf13e49cea706994ed38804f6d171"
         }
    },
    "version" : 3
};

let json = JSON.stringify(data);
let password = "Your Password here";

ethers.Wallet.fromEncryptedJson(json, password).then(function(wallet) {
    console.log("Address: " + wallet.address);
    // "Address: 0x88a5C2d9919e46F883EB62F7b8Dd9d0CC45bc290" (Example)
});
```



##### To get the Network version from Metamask : <br> <br>
1- Install Metamask in website  <br>
2- Login to your Account in Metamask <br>
3- press F12 in browser <br>
4- Go to console  <br>
5- paste the code given : web3.currentProvider.networkVersion <br>
6- You will recieve a number which defines your address <br>
7- If you want to work with multiple testnet or want to switch between mainnet and Testnet then you can use this function in switch case <br> <br>



## Loading a Contract with EthersJs

JavaScript :

```
let contractabi = Paste your contract ABI from remix.ethereum.org here ;
let contractaddress = "paste your contract address deployed in Testnet/Mainnet here (Not Javascript deployment address) ";
let contract = new ethers.Contract(contractaddress,contractabi,wallet);
```


## Calling Smart Contract functions

JavaScript :
```
//If i have a function of checking the superAdmin in the contract
var superadmin = await contract.functions.superAdmin(); //Example
// await (contractName).functions.(YourFunctionName()); //Template

//If you want to pass a value inside contract
var pass = await contract.functions.put("Hello World"); //Example
// await (contractName).functions.(YourFunctionName("Parameters")); //Template

```


## Estimating Gas in EthersJs

JavaScript File: (Run on Console)<br>

```
1st Example: (Estimate Function)
//contract.estimate.(YourFunctionName)(parameters);
contract.estimate.transfer("0xC643eFaB1DB16CCd17C44F6a7E652AFB3C37A940",100000000000000000000); 

2nd Example: (getting value from function)
//We i'll now get the gas price from the function
let value = await contract.estimate.transfer("0xC643eFaB1DB16CCd17C44F6a7E652AFB3C37A940","100000000000000000000000000");
let gasest = ethers.utils.formatEther(value.mul(10**9)); 
gasest;

Output -> "0.000051968"

```
