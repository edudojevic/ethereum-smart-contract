# How to publish a smart contract on the ethereum blockchain
This repo shows how we can push a simple smart contract onto the Ethereum blockchain. Here we use rinkeby which is a separate test blockchain.

Before we can deploy a contract you need to make sure you have an address on the rinkeby blockchain. Then you can get yourself some free test ethers at: https://faucet.rinkeby.io:

Now you need to set a few things up:
```
/* Get solidity */
brew tap ethereum/ethereum
brew install solidity

/* Compile sample contract */
solc -o target --bin --abi ecoin.sol

/* Start up geth (CLI so that we can do stuff on the blockhain) */
/* Note that we're using the rinkeby test blockchain */
geth --verbosity 0 console -- rinkeby
```

The compiled code can then be used to set up and deploy a contract as follows:

```
abi = JSON.parse(‘[{“constant":false,"inputs":[{"name":"receiver","type":"address"},{"name":"amount","type":"uint256"}],"name":"sendCoin","outputs":[{"name":"sufficient","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"name":"","type":"address"}],"name":"coinBalanceOf","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"inputs":[{"name":"supply","type":"uint256"}],"payable":false,"stateMutability":"nonpayable","type":"constructor"},{"anonymous":false,"inputs":[{"indexed":false,"name":"sender","type":"address"},{"indexed":false,"name":"receiver","type":"address"},{"indexed":false,"name":"amount","type":"uint256"}],"name":"CoinTransfer","type":"event”}]’);

bin = "0x6060604052341561000f57600080fd5b60405160208061031c833981016040528080519060200190919050506305f5e1006000803373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020819055505061029a806100826000396000f30060606040526004361061004c576000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806370a0823114610051578063a9059cbb1461009e575b600080fd5b341561005c57600080fd5b610088600480803573ffffffffffffffffffffffffffffffffffffffff169060200190919050506100e0565b6040518082815260200191505060405180910390f35b34156100a957600080fd5b6100de600480803573ffffffffffffffffffffffffffffffffffffffff169060200190919080359060200190919050506100f8565b005b60006020528060005260406000206000915090505481565b806000803373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020541015151561014557600080fd5b6000808373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002054816000808573ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000205401101515156101d257600080fd5b806000803373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060008282540392505081905550806000808473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000206000828254019250508190555050505600a165627a7a72305820bdf9d56e0570beb200ad3de2ee0e90619a192fcb8d12a6cb7849bd92c544a81f0029"

tokenSupply = 1000000
tokenContract = eth.contract(abi)
deploy = {from: eth.coinbase, data: bin, gas: 1000000}

/* Check balance */
web3.fromWei(eth.getBalance(eth.coinbase))

/* Unlock account */
personal.unlockAccount(eth.coinbase)

/* Deploy contract */
tokenInstance = tokenContract.new(tokenSupply, deploy)

/* Get transaction receipt */
eth.getTransactionReceipt(tokenInstance.transactionHash)

/* Check new balance */
web3.fromWei(eth.getBalance(eth.coinbase))
```

If everything went as expect, you should now be able to see your transaction visible at the Rinkeby etherscan:
`https://rinkeby.etherscan.io/address/<your ether address>`
