# Asset Transfer using hyperledger Fabric

This is a simple demonstration of setting up 2 orgs with 2 peers each. Using Solo orderer and cryptogen for certificates. It is not intended for production use. Please use CA certs instead. MSP is already provisioned.

## Transfer Consortium Setup
1. Two peers for each organization
2. Pre-created Member Service Providers (MSP) for authentication and identification
3. An Orderer using SOLO
4. _Testchannel_ – this channel is public blockchain both orgs have read and write access to it.

## Directory Structure
```

├── chaincode				        
      ├── transfer			
        ├── transfer.go		
├── cli.yaml
├── configtx.yaml
├── crypto-config.yaml
├── docker-compose-transfer.yaml                  
├── peer.yaml
├── README.md
├── scripts
    ├──chaincodeInstallInstantiate.sh
    ├──cleanup.sh
    ├──createArtifacts.sh
    ├──checkOrgChannelsSubscription.sh
    ├──creatChannels.sh
    ├──createLedgerEntries.sh
    ├──createTransferRequest.sh    
    ├──joinChannels.sh
    ├──query.sh
    ├──queryAll.sh
    ├──setupNetwork.sh    
    ├──start_network.sh
    ├──stop_network.sh
```


## Requirements
Please resolve all issues and get the first network up and running before attempting to install and run this demo. You can find ubuntu cheat sheet to get basic fabric running by following setting-up-fabric.pdf document included in this repo and could be found at the root of the folder structure.
Following are the software dependencies required to install and run hyperledger explorer
* docker 17.06.2-ce [https://www.docker.com/community-edition]
* docker-compose 1.14.0 [https://docs.docker.com/compose/]
* GO programming language 1.7.x
* git, curl, and other binaries needed to run on windows or OS X.
* Optionals - atom/vscode for editing files, kitematic for docker view.

## Clone Repository

Clone this repository to get the latest using the following command.
1. `https://github.com/Archstrategy/MortgageBlockchainFabric.git`
2. `rename top level folder to transfer` for some reason the network chokes without this name.
2. `cd mrtgexchg`  

## Setup network
* Run the following command to kill any stale or active containers:

  `./scripts/cleanup.sh`

* Create artifacts (certs, genesis block and channel info)

  `./scripts/createArtifacts.sh`

  * build chaincodes (compile before you deploy, this step is optional but is a must if you edit chaincodes and want to restart the network so as to make sure it does not fail to compile during deployment )

    `go get -u --tags nopkcs11 github.com/hyperledger/fabric/core/chaincode/shim`
    `go build --tags nopkcs11 `

* start network with start option
  `./scripts/start_network.sh`

* `./scripts/setupNetwork.sh` this script creates channels, join channels, instantiates and installs chaincode, populates ledger with initial entries. It will also dump the entire ledgers at the end.




##### Now run asset transfer request.  

The script runs transfer request multiple times on the same asset id=123 to show the updates which are printed at the end of the script runs
`./scripts/createTransferRequest.sh`

```{
  "Snumber": "123",
  "Description": "5 High Strret, CA 75000 ",
  "Owner": "Rishi Shimpi",
  "Status": "transferred",
  "TransactionHistory": {
    "createAsset": "Wed, 14 Mar 2018 19:04:51 UTC",
    "transferAssetWed, 14 Mar 2018 19:06:15 UTC": "Asset transferred from: John Doe to new owner: Rajesh Shimpi on:Wed, 14 Mar 2018 19:06:15 UTC",
    "transferAssetWed, 14 Mar 2018 19:07:48 UTC": "Asset transferred from: Rajesh Shimpi to new owner: Raj Shimpi on:Wed, 14 Mar 2018 19:07:48 UTC",
    "transferAssetWed, 14 Mar 2018 19:07:53 UTC": "Asset transferred from: Raj Shimpi to new owner: Tiger Shimpi on:Wed, 14 Mar 2018 19:07:53 UTC",
    "transferAssetWed, 14 Mar 2018 19:07:58 UTC": "Asset transferred from: Tiger Shimpi to new owner: Rishi Shimpi on:Wed, 14 Mar 2018 19:07:58 UTC"
  }
}```
