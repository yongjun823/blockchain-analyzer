# Blockchain Analyzer: Analyzing Hyperledger Fabric Ledger, Transactions

## Elastic stack error fix
Error: elasticsearch exited with code 137

Error fix
1. Update elasticsearch & kibana version from ``7.1.1`` to ``7.9.3``
2. Remove resources tag

I tested it in a desktop (16G memory) and aws c5.large instance (cpu2 4G memory) environment. 
If memory is small, please enter the vm size adjustment command.

```
sudo sysctl -w vm.max_map_count=262144
```

## Description

Each blockchain platform, including Hyperledger Fabric, provides a way to record information on blockchain in an immutable manner. In the case of Hyperledger Fabric, information is recorded as a `key/value` pair. All previous updates to a `key` are recorded in the ledger of a Hyperledger Fabric peer, but only the latest value of a `key` can be easily queried (see [documentation](https://godoc.org/github.com/hyperledger/fabric/core/chaincode/shim#ChaincodeStub.GetHistoryForKey)). This mechanism makes it challenging to perform analysis of updates to a `key`. Further, if an application groups together certain keys for application-level functions, it is challenging to analyze them together. In order to perform information provenance, it is necessary to 
analyze all updates to a single `key` or group of keys that together comprise an information unit.

To address this issue, we have created a project which can be used to analyze ledger data stored within a Hyperledger Fabric peer. This project can also be used to analyze operational data, such as number of blocks and transactions.

Currently, the project includes:

1. an Elastic beats module (in Go), that ships ledger data from a Hyperledger peer to an Elasticsearch instance. 

2. generic Kibana dashboards, that allow selection of a particular key and visualization updates to it (channel, id, timestamp etc) - similar to Hyperledger Explorer.

3. [scripts](https://github.com/hyperledger-labs/blockchain-analyzer/tree/master/network) to create Hyperledger Fabric network in different configurations, i.e., `basic`, `mulitchannel`, and `appledemo`.

4. a [program](https://github.com/hyperledger-labs/blockchain-analyzer/tree/master/apps/dummyapp) to generate test data

5. a [standalone utility](https://github.com/hyperledger-labs/blockchain-analyzer/tree/master/dumper) to dump Fabric transaction data as JSON file for loading and analyzing in any document store. 

6. [scripts](https://github.com/hyperledger-labs/blockchain-analyzer/tree/master/stack) to start and stop Elasticsearch and Kibana.


### Eventual goal
The eventual goal of this project is to evaluate transactions of any blockchain using any document databases or search engines such as MongoDB, CouchDB, or Elastic stack.

### Project background

This project was developed as part of [Hyperledger summer internship program](https://wiki.hyperledger.org/display/INTERN/Analyzing+Hyperledger+Fabric+Ledger%2C+Transactions%2C+and+Logs+using+Elasticsearch+and+Kibana). 


## Getting Started

We recommend that you start with the instructions for [`basic`](docs/Basic_setup.md) network.

Once you have it up and running, you can try the [`multichannel`](docs/Multichannel_setup.md), and `appledemo`.

If you want to learn more about `fabricbeat` configuration or connect it to your own blockchain network, click [here](docs/Fabricbeat_config.md).

If you want to understand dataflow involving `fabricbeat`, click [here](docs/Fabricbeat_architecture.md).

## License
The Apache 2.0 License applies to the whole project, except for the [stack](https://github.com/hyperledger-labs/blockchain-analyzer/tree/master/stack) directory and its contents.

## Maintainers:

[Salman Baset](https://github.com/salmanbaset) <br>
[Balazs Prehoda](https://github.com/balazsprehoda)
