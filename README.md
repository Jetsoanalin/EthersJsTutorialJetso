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
 4- Once everything is loaded we are ready to interact with Ethereum Blockchain Network <br>

## Creating a Wallet Instance for Signing Contract:

let provider = new ethers.getDefaultProvider('rinkeby'); //For Different Testnets <br>

or

let provider = ethers.getDefaultProvider(); //For Mainnet <br>

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




### Loading Wallet :

##### Using Private Key - <br>
let privatekey = "place your Private Key here"; <br>
let wallet = new ethers.Wallet(privatekey,provider); <br>


To get the Network version from Metamask : <br> <br>
1- Install Metamask in website  <br>
2- Login to your Account in Metamask <br>
3- press F12 in browser <br>
4- Go to console  <br>
5- paste the code given : web3.currentProvider.networkVersion <br>
6- You will recieve a number which defines your address <br>
7- If you want to work with multiple testnet or want to switch between mainnet and Testnet then you can use this function in switch case <br> <br>

## Estimating Gas in EthersJs

JavaScript File: <br>
