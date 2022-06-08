# Thesis_WardvanGerwen

Within this Github I present the code and node details used for my Thesis: **Blockchain based data management system in an IoT environment.**
The test are ran between 20th of May and 7th of June 2022 and documented betwwen 6th - 9th of June 2022.

The goal of this PoC is to test near-instant low cost transactions on the LN while including an decryption key   
The decryption key belongs to a encrypted file stored on IPFS, whereby the participants shared this CID beforehand   
The LN is used to transfer the value.  

**Software used**.  
- LightningPolar: https://lightningpolar.com  
- Docker: https://www.docker.com  


**Information**
- The Lightning network: https://lightning.network  
- Mastering the Lightning network: https://github.com/lnbook/lnbook/blob/develop/README.md 
- Additional information LN: https://wiki.ion.radar.tech  
- Basic of Lightning Technology (BOLT): https://docs.lightning.engineering/the-lightning-network/lightning-overview/understanding-lightning-invoices  
- BOLT11: https://github.com/lightning/bolts/blob/master/11-payment-encoding.md  
- State of LN: https://bitcoinvisuals.com/lightning  

**Node details**.  
- 3 Bitcoin core nodes  
- 10 Lightning LND nodes, using BOLT11 invoices with including the decryption key in the memo  
- 8 Payment channels between those nodes   

**Previous test**
TPS Lightning test by Joost Jager: https://github.com/bottlepay/lightning-benchmark 
Results of Joost Jager: 
|   Configuration   | Transactions / sec | Avg latency (sec) |
|:-----------------:|:------------------:|:-----------------:|
| eclair            | 89                 | 1.1               |
| eclair-postgres   | 46                 | 2.1               |
| clightning        | 61                 | 1.6               |
| lnd-bbolt-keysend | 35                 | 2.8               |
| lnd-bbolt         | 33                 | 3.0               |
| lnd-etcd          | 4                  | 29.2              |
| lnd-etcd-cluster  | 4                  | 31.8              |


## Basic architecture: 

Within the basic architecture

Steps:
1) Node opening
2) Payment channel opening
3) Transactions within the payment channel
4) Payment channel closing after the market transaction is finalised

```
Input: 
Output: 



```

## Advanced architecture: 

1) Node opening
2) Bidirectional payment channel opening
3) Transactions are performed over the lightning network leveraging the point-to-point architecture. Payments are routed through the lightning network with the use of HTLC contracts.



```
Input: 
Output: 



```


## Results

Note that the cost per transactions are not tested within the testing environment.  
The costs are subject to market conditions and node operators in a real-world setting and therefore could vary significantly. 
For more information: https://medium.com/@timevalueofbtc/the-time-value-of-bitcoin-and-lnrr-e0c435931bd8  


### Basic architecture
 
The transactions are simulated between parties in one payment channel.  
| Transactions | Average tps             | Avg latency (seconds) |
|------------|-------------------------|-----------------------|
| 100        |                     68  | 0,8                   |
| 250        |                     64  | 0,9                   |
| 500        |                     65  | 1,1                   |

The tps obtained within the payment channel is sufficient to perform the intented transactions.  
However when looking at the total architecture the solution is not scalable due to the bottle neck in step 4. 


### Advanced architecture

The transactions are routed through 8 payment channels on testnet using LightningPolar.  
| Transactions | Average tps             | Avg latency (seconds) |
|------------|-------------------------|-----------------------|
| 100        |                     40  | 2,8                   |
| 250        |                     35  | 2,7                   |
| 500        |                     36  | 2,8                   |


The tps obtained by routing transactions over the LN is sufficient to perform the intented transactions.  
However when looking at the total architecture the solution is only scalable with enought development time.  
The opening of nodes and channels need to be distributed on-chain and ensures that the growth of the LN takes time.  
This does not create a permanent bottle neck like the one in the basic architecture, but does present with the reality that the solution can only grow/scale if given sufficient time.  
**Please note** that it is difficult to assed the tps in the whole network by extrapolating these results. The results are presented within the testnet environment. Growth of the lightning network to the point of global adoption can in pratice give significant different results and measuring the mainnet tps of the lightning network dd May 2022 is practically not feasible.  
