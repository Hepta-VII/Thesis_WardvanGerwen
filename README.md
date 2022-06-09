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
- Mastering the Lightning network: https://github.com/lnbook/lnbook/blob/develop/README.md, code examples out of the book are used.
- Lightning development kit: https://github.com/lightningdevkit, code examples from LDK are used.
- Additional information LN: https://wiki.ion.radar.tech  
- Basic of Lightning Technology (BOLT): https://docs.lightning.engineering/the-lightning-network/lightning-overview/understanding-lightning-invoices  
- BOLT11: https://github.com/lightning/bolts/blob/master/11-payment-encoding.md  
- State of LN: https://bitcoinvisuals.com/lightning  

**Node details**.  
- 3 Bitcoin core nodes  
- 10 Lightning LND nodes, using BOLT11 invoices with including the decryption key in the memo  
- 12 Payment channels between those nodes   
The set-up is visualized [Here]: LINK

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
```
\\ Code out of Mastering the Lightning network

\\Opening the channel
[chain_hash:chain_hash]
[32*byte:temporary_channel_id]
[u64:funding_satoshis]
[u64:push_msat]
[u64:dust_limit_satoshis]
[u64:max_htlc_value_in_flight_msat]
[u64:channel_reserve_satoshis]
[u64:htlc_minimum_msat]
[u32:feerate_per_kw]
[u16:to_self_delay]
[u16:max_accepted_htlcs]
[point:funding_pubkey]
[point:revocation_basepoint]
[point:payment_basepoint]
[point:delayed_payment_basepoint]
[point:htlc_basepoint]
[point:first_per_commitment_point]
[byte:channel_flags]
[open_channel_tlvs:tlvs]

\\ Accepting the channel
[32*byte:temporary_channel_id]
[u64:dust_limit_satoshis]
[u64:max_htlc_value_in_flight_msat]
[u64:channel_reserve_satoshis]
[u64:htlc_minimum_msat]
[u32:minimum_depth]
[u16:to_self_delay]
[u16:max_accepted_htlcs]
[point:funding_pubkey]
[point:revocation_basepoint]
[point:payment_basepoint]
[point:delayed_payment_basepoint]
[point:htlc_basepoint]
[point:first_per_commitment_point]
[accept_channel_tlvs:tlvs]

\\Multi_sign
2 <pubkey_participant1> <pubkey_participant2> 2 CHECKMULTISIG

```
3) Transactions within the payment channel
```
// Changed example out of LDK Rust ; For the whole pack download the code provide on https://github.com/lightningdevkit/rust-lightning/tree/main/lightning-invoice 

"lnbc_invoiceId".to_owned(),
			InvoiceBuilder::new(Currency::Bitcoin)
				.amount_milli_satoshis(150_000_000)
				.duration_since_epoch(Duration::from_secs(Amount_sec))
				.payment_secret(PaymentSecret([0x11; 32]))
				.payment_hash(sha256::Hash::from_hex(
					"Hash"
				).unwrap())
				.description("Transaction_Identification_DECRYPTION_KEY".to_owned())
				.expiry_time(Duration::from_secs(60))
				.build_raw()
				.unwrap()
				.sign(|_| {
					RecoverableSignature::from_compact(
						&hex::decode("Decode_Hash").unwrap(),
						RecoveryId::from_i32(1).unwrap()
					)
				}).unwrap(),
			false, // Same features as set in InvoiceBuilder
			false, // No unknown fields


```
4) Payment channel closing after the market transaction is finalised

```
\\ Code out of Mastering the Lightning network

\\ Closing message
[channel_id:channel_id]
[u16:len]
[len*byte:scriptpubkey]

\\ Signed message
[channel_id:channel_id]
[u64:fee_satoshis]
[signature:signature]

```

## Advanced architecture: 

1) Node opening
2) Payment channel opening
3) Transactions are performed over the lightning network leveraging the point-to-point architecture. Payments are routed through the lightning network with the use of HTLC contracts.



```
// Changed example out of LDK Rust ; For the whole pack download the code provide on https://github.com/lightningdevkit/rust-lightning/tree/main/lightning-invoice 

"lnbc_invoiceId".to_owned(),
			InvoiceBuilder::new(Currency::Bitcoin)
				.amount_milli_satoshis(150_000_000)
				.duration_since_epoch(Duration::from_secs(Amount_sec))
				.payment_secret(PaymentSecret([0x11; 32]))
				.payment_hash(sha256::Hash::from_hex(
					"Hash"
				).unwrap())
				.description("Transaction_Identification_DECRYPTION_KEY".to_owned())
				.expiry_time(Duration::from_secs(60))
				.build_raw()
				.unwrap()
				.sign(|_| {
					RecoverableSignature::from_compact(
						&hex::decode("Decode_Hash").unwrap(),
						RecoveryId::from_i32(1).unwrap()
					)
				}).unwrap(),
			false, // Same features as set in InvoiceBuilder
			false, // No unknown fields



```


Note that these codes are simulated in Lightning Polar and there is no development coding needed to open, close, fund the payment channels.  
The same applies for the routed payments or transactions in one payment channel. The decryption key can be represented in the description of the transactions.  
I propose the following template to include them: Transaction Information + Decryption_Key 

## Results

Note that the cost per transactions are not tested within the testing environment.  
The costs are subject to market conditions and node operators in a real-world setting and therefore could vary significantly. 
For more information: https://medium.com/@timevalueofbtc/the-time-value-of-bitcoin-and-lnrr-e0c435931bd8  


### Basic architecture
 
The transactions are simulated between parties in one payment channel. 
The payment channel has a capacity of 15.000.000 and each transaction is for 10.000 sats. 

| Transactions | Average tps             | Avg latency (seconds) |
|------------|-------------------------|-----------------------|
| 100        |                     68  | 0,8                   |
| 250        |                     64  | 0,9                   |
| 500        |                     65  | 1,1                   |

The tps obtained within the payment channel is sufficient to perform the intented transactions.  
However when looking at the total architecture the solution is not scalable due to the bottle neck in step 4. 

**Aggregated latency and cost (excl hardware) for one market transaction**
| Basic Architecture                 | Latency (sec) | Aggregated latency (sec) | Latency (min) | Aggregated latency (min)                | Cost (USD)* | Aggregated cost (USD) |
|------------------------------------|---------------|--------------------------|---------------|-----------------------------------------|-------------|-----------------------|
| Opening a node & depositing funds* | 600           | 600                      | 10            | 10                                      | 0,5         | 0,5                   |
| Payment channel opening            | 600           | 1200                     | 10            | 20                                      | 0,5         | 1                     |
| Transactions                       | 1,1           | 1201,1                   | 0,02          | 20,02                                   | 0           | 1,00                  |
| Payment channel closing            | 600           | 1801,1                   | 10            | 30,02                                   | 0,5         | 1,50                  |
| Aggregated total                   |               | 1801,1                   |               |                                  30,02  |             | 1,5                   |
| Aggregated normal interaction      |               | 1201,1                   |               |                                  20,02  |             | 1                     |  
*Only needed the first time when interacting with LN or when more funds are deposited.

For on-chain transactions I use the average block-time of 10 min.  
One confirmation is deemed sufficient for the purpose of this thesis, once the values are higher a participat should consider multiple confirmations.  
I document the costs of the blockspace bit higher than the dd May 2022 price of <0.20 USD. More usage of the block space will increase the prices and 0.2 USD will be a underestimation with increased adoption.  

### Advanced architecture

The transactions are routed payment channels on testnet using LightningPolar. 
The value of each transaction that is routed is 10.000 sats. 
Each payment channel has an capacity of 15.000.000 sats. 
Note that larger value transactions or different liquidity within payment channels can give other results.

| Transactions | Average tps             | Avg latency (seconds) |
|------------|-------------------------|-----------------------|
| 100        |                     40  | 2,8                   |
| 250        |                     35  | 2,7                   |
| 500        |                     36  | 2,8                   |

**Aggregated latency and cost (excl hardware) for market transactions**
| Advanced Architecture              | Latency (sec) | Aggregated latency (sec) | Latency (min) | Aggregated latency (min)                 | Cost (USD)* | Aggregated cost (USD) |
|------------------------------------|---------------|--------------------------|---------------|------------------------------------------|-------------|-----------------------|
| Opening a node & depositing funds* | 600           | 600                      | 10            | 10                                       | 0,5         | 0,5                   |
| Payment channel opening            | 600           | 1200                     | 10            | 20                                       | 0,5         | 1                     |
| Transactions                       | 1,1           | 1201,1                   | 0,02          | 20,02                                    | 0,005       | 1,01                  |
| Aggregated total                   |               | 1201,1                   |               |                                  20,02   |             | 1,01                  |
| Aggregated when network adoption   |               | 1,1                      |               |                                    0,02  |             | 0,005                 |. 
*Only needed the first time when interacting with LN or when more funds are deposited. Within this architecture the deposists and withdraw of fund with on-chain transactions will be present. 

For on-chain transactions I use the average block-time of 10 min.  
One confirmation is deemed sufficient for the purpose of this thesis, once the values are higher a participat should consider multiple confirmations.  
I document the costs of the blockspace bit higher than the dd May 2022 price of <0.20 USD. More usage of the block space will increase the prices and 0.2 USD will be a underestimation with increased adoption.  


The tps obtained by routing transactions over the LN is sufficient to perform the intented transactions.  
However when looking at the total architecture the solution is only scalable with enought development time.  
The opening of nodes and channels need to be distributed on-chain and ensures that the growth of the LN takes time.  
This does not create a permanent bottle neck like the one in the basic architecture, but does present with the reality that the solution can only grow/scale if given sufficient time.  
**Please note** that it is difficult to assed the tps in the whole network by extrapolating these results. The results are presented within the testnet environment. Growth of the lightning network to the point of global adoption can in pratice give significant different results and measuring the mainnet tps of the lightning network dd May 2022 is practically not feasible.  
