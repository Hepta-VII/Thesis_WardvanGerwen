# Thesis_WardvanGerwen

## Initialization contract: 

Input: DataOwner, DataUser, Terms, EncryptionKey, EncryptedDataLocation \
Output: Null \
// DataOwner check \

if msg.sender is not owner then \
  throw; \
end \

If ETH value contract >0 then \
  insert(Terms, EncryptionKey); \
end \

Notes: insert(EncryptedDataLocation)
