# Thesis_WardvanGerwen

Within this Github I present the code and node details used for my Thesis: **Blockchain based data management system in an IoT environment.**

The goal of this PoC is to test near-instant low cost transactions on the LN while including an decryption key   
The decryption key belongs to a encrypted file stored on IPFS, whereby the participants shared this CID beforehand   
The LN is used to transfer the value.  

**Software used**.  
LightningPolar: https://lightningpolar.com  
Docker: https://www.docker.com  


**Information**
The Lightning network: https://lightning.network  
Additional information LN: https://wiki.ion.radar.tech  
Basic of Lightning Technology (BOLT): https://docs.lightning.engineering/the-lightning-network/lightning-overview/understanding-lightning-invoices  
BOLT11: https://github.com/lightning/bolts/blob/master/11-payment-encoding.md  

**Node details**.  
3 Bitcoin core nodes  
10 Lightning LND nodes, using BOLT11 invoices with including the decryption key in the memo  
8 Payment channels between those nodes   

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

```
Input: 
Output: 



```

## Advanced architecture: 

Within the advanczed architecture the payments are routed through the lightning network with the use of HTLC contracts. 

```
Input: 
Output: 



```


## Results

Note that the cost per transactions are not tested within the testing environment.  
The costs are subject to market conditions and node operators in a real-world setting and therefore could vary significantly. 
For more information: https://medium.com/@timevalueofbtc/the-time-value-of-bitcoin-and-lnrr-e0c435931bd8  


**Basic architecture** 

No scaleable on global scale


**Basic architecture** 

Maybe scaleable on global scale
