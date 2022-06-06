# Thesis_WardvanGerwen

Within this Github I present the code and node details used for my Thesis: **Blockchain based data management system in an IoT environment.**

The goal of this PoC is to test near-instant low cost transactions on the LN while including an decryption key   
The decryption key belongs to a encrypted file stored on IPFS, whereby the participants shared this CID beforehand   
The LN is used to transfer the value.  

**Software used**.  
LightningPolar: https://lightningpolar.com  
Docker: https://www.docker.com  
CID on IPFS???  

**Node details**.  
5 Bitcoin core nodes  
21 Lightning LND Nodes  
10 Payment channels between those nodes   





## Basic architecture: 

Within the basic architecture

```
Input: DataOwner, DataUser, Terms, EncryptionKey, EncryptedDataLocation 
Output: Null 
// DataOwner check

if msg.sender is not owner then 
  throw; 
end

If ETH value contract >0 then 
  insert(Terms, EncryptionKey); 
end

Notes: insert(EncryptedDataLocation)
```

## Advanced architecture: 

Within the advanczed architecture the payments are routed through the lightning network with the use of HTLC contracts. 



## Results
