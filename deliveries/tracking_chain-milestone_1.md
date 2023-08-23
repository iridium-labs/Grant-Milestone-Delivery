# Milestone Delivery :mailbox:

**The [invoice form :pencil:](https://docs.google.com/forms/d/e/1FAIpQLSfmNYaoCgrxyhzgoKQ0ynQvnNRoTmgApz9NrMp-hd8mhIiO0A/viewform) has been filled out correctly for this milestone and the delivery is according to the official [milestone delivery guidelines](https://github.com/w3f/Grants-Program/blob/master/docs/Support%20Docs/milestone-deliverables-guidelines.md).**  

* **Application Document:** [LINK](https://github.com/w3f/Grants-Program/blob/master/applications/tracking_chain.md)
* **Milestone Number:** 1

**Context** (optional)  
A smart contract has been created capable of storing a pair of key-values and the entire infrastructure in order to be able to write on-chain the data entering the Triage API service. A generic Substrate Client has also been created in Dotnet (with the integration of the Ajuna SubstrateGaming project) capable of supporting add-on modules to support any Substrate chain that has the contract pallet.

**Deliverables**

| Number | Deliverable | Link | Notes |
| ------- | ------------- | ----- |------------- |
| **0a.** | License | [LINK](https://github.com/TrackingChains/TrackingChain/blob/main/LICENSE) | MIT |
| **0b.** | Documentation | [LINK](https://github.com/TrackingChains/TrackingChain/wiki) | We will provide a basic **tutorial** that explains how a user can configure the data entry for create profile for to associate the tracking requests to a smart contract transaction. |
| **0c.** | Testing and Testing Guide | [LINK](https://github.com/TrackingChains/TrackingChain/wiki/Unit-Test)  [SOURCE](https://github.com/TrackingChains/TrackingChain/tree/main/test/TrackingChain.UnitTest) | Core functions will be fully covered by comprehensive unit tests to ensure functionality and robustness. |
| **0d.** | Docker | [LINK SQL Server under Docker](https://learn.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker?view=sql-server-ver16&pivots=cs1-bash) | We will provide a Dockerfile(s) that can be used to test all the functionality delivered with this milestone. |
| 0e. | Article | [LINK](https://github.com/TrackingChains/TrackingChain/wiki/Configuration-Step-By-Step) | We will publish an **article**/workshop that explains how to use. |
| 1. | API: Triage | [SOURCE](https://github.com/TrackingChains/TrackingChain/tree/v0.1.0-alpha/src/Triage.API) [BINARY](https://github.com/TrackingChains/TrackingChain/releases/tag/v0.1.0-alpha) | Written in Asp.Net, The Triage API acts as the gateway between the "Web2" world, which receives the Tracking requests, and the "Web3" world, where these requests will be saved. To do this it will verify that the incoming request is compatible with one of the profiles associated in the configuration, if so it will save the request in the Triage (and return an Guid to client) which will then be processed by the next service. In case the incoming request does not match any profile, it will be rejected. The Triage operation does not involve any concurrent processing, allowing for seamless scalability. It can accept requests simultaneously or even create multiple endpoints. The API expects a POST call with the following data: field Code used as a Key in the smart contract, field valueData used as one of the elements of the Value and the field Category that will be used to associate a profile with Tracking |
| 2. | Aggregator Pool Worker | [SOURCE](https://github.com/TrackingChains/TrackingChain/tree/v0.1.0-alpha/src/AggregatorPool.Worker) [BINARY](https://github.com/TrackingChains/TrackingChain/releases/tag/v0.1.0-alpha) | Written in C#,The Aggregator Pool Worker will used to transfer processable data from Triage that has no time dependencies into the Pool. A transaction is transferable when there is no pending Transaction with same Profile to be completed in the pool |
| 3. | Tx Generator Worker | [SOURCE](https://github.com/TrackingChains/TrackingChain/tree/v0.1.0-alpha/src/TransactionGenerator.Worker) [BINARY](https://github.com/TrackingChains/TrackingChain/releases/tag/v0.1.0-alpha) | Written in C#,The Tx Generator Worker Worker will used to take data from Pool and make a transaction on onchain via smartcontract. In this case each worker instance takes one of the transactions entered into the pool and will process it by calling the selected smartcontract (this selection of smartcontract has already been made by Triage). Once the transaction has been made, it will save the HASH of the Tx so that it can be used by the next service. This service supports both (Ink! and EVM) TrackingChain smart contracts. The selection of the version of the smart contract to use will be given by the profile that was associated in the Triage phase |
| 4. | Tx Watcher Worker | [SOURCE](https://github.com/TrackingChains/TrackingChain/tree/v0.1.0-alpha/src/TransactionWatcher.Worker) [BINARY](https://github.com/TrackingChains/TrackingChain/releases/tag/v0.1.0-alpha) | Written in C#,The Tx Generator Worker Worker will used to check all Tx pending for finalized (or failed) status. Each worker instance takes a pending transaction and through the hash it will verify if it has been finalized successfully. This service supports both (Ink! and EVM) TrackingChain smart contracts. The selection of the version of the smart contract to use will be given by the profile that was associated in the Triage phase |
| 5. | API: Registry | [SOURCE](https://github.com/TrackingChains/TrackingChain/tree/v0.1.0-alpha/src/Triage.API) [BINARY](https://github.com/TrackingChains/TrackingChain/releases/tag/v0.1.0-alpha) | Written in C#,Provide API for check the status of each Tracking request. Wich Guid of tracking request is possibile to watch the status of transaction. For example, the API will tell if the Tracking is in Trigae/Pool/Pending/Complete status and will provide all the times with which it moved from one status to another, as well as the onchain transaction information (gas used, hash tx. ..) |
| 6. | Web Application | [SOURCE](https://github.com/TrackingChains/TrackingChain/tree/v0.1.0-alpha/src/Triage.WebApplication) [BINARY](https://github.com/TrackingChains/TrackingChain/releases/tag/v0.1.0-alpha) | Written in Asp.Net MVC pages for manage the insert tracking and views. A web interface from which it will be possible through a simple form to select the fields required to make a request towards the triage. |
| 7. | Ink! Smart contracts. | [SOURCE](https://github.com/TrackingChains/InkTrackingChain) | We will deliver a set of Ink! smart contracts that will able to track the key values. In particular, it will take care of saving in a dictionary key-value formed by a "Key" byte32 and the "Value" a list of bytes. A get call will also be available, which given a "Key" byte32 returns the entire "Value" list of Bytes saved over time, also providing the block number in which the transaction was carried out. It will also include the C# function that the "Tx Generator Worker" service will have to do to write onchain and the read call that will be used by the "Tx Watcher Worker" service. The implementation will partially reuse the [C# SubstrateGaming library](https://github.com/SubstrateGaming) |
| 8. | EVM Smart contracts | [SOURCE](https://github.com/TrackingChains/EVMTrackingChain)  | We will deliver a set of EVM smart contracts that will able to track the key values. In particular, it will take care of saving in a dictionary key-value formed by a "Key" byte32 and the "Value" a list of bytes. A get call will also be available, which given a "Key" byte32 returns the entire "Value" list of Bytes saved over time, also providing the block number in which the transaction was carried out. It will also include the C# function that the "Tx Generator Worker" service will have to do to write onchain and the read call that will be used by the "Tx Watcher Worker" service. The implementation will partially reuse the [C# Nethereum library](https://nethereum.com/) |


**Additional Information**
> Any further comments on the milestone that you would like to share with us.


I have tried to detail and document every single step necessary to configure and run the application. Which is not very trivial so I'm available for a short call in which I show step by step an installation on Ubuntu and some Runs in real time to show how requests are intercepted and written onchain.  
In the milestone I also left the EVM smart contract project (although it is out of scope because the Ink smart contract was introduced for the grant, for this reason I did not include it in the guide in order to simplify the revision process. However, the EVM smart contract is present in the configuration database seed, so it can be used)