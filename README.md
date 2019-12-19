# BlockChain-Uzd3

## Apšilimas:

### `rpc_example.py` example

![image](https://user-images.githubusercontent.com/12825358/71216395-0cfb8a00-22c3-11ea-847d-9b9cf8774525.png)

### `pc_transaction.py` example

![image](https://user-images.githubusercontent.com/12825358/71216622-a7f46400-22c3-11ea-9470-e641d60388f7.png)

### `rpc_block.py` example

![image](https://user-images.githubusercontent.com/12825358/71216559-77142f00-22c3-11ea-8049-321c1b927549.png)

## Užduotys:

### 1.1 Programa be patobulinimo, apskaičiuoja konkrečios (kode įvestos) transakcijos mokestį:

```python
import sys
from bitcoin.rpc import RawProxy
p = RawProxy()
txhash = '0627052b6f28912f2703066a912ea577f2ce4da4caa5a5fbd8a57286c345c2f2'
raw_tx = p.getrawtransaction(txhash)
decoded_tx = p.decoderawtransaction(raw_tx)
total = 0
transactionvalue = 0
for item in decoded_tx['vin']:
        vout = item['vout']
        txid = item['txid']
        raw_txN = p.getrawtransaction(txid)
        decoded_txN = p.decoderawtransaction(raw_txN)
        for output in decoded_txN['vout']:
                if(output['n'] == vout):
                        total = total + output['value']
for output in decoded_tx['vout']:
        transactionvalue = transactionvalue + output['value']
transactionfee = total - transactionvalue
print transactionfee
```

![image](https://user-images.githubusercontent.com/12825358/71216964-daeb2780-22c4-11ea-9bbd-bb4cc4d63ddf.png)

### 1.2 Programa su patobulinimu, apskaičiuoja konkrečios (ranka įvestos) transakcijos mokestį:

```python
import sys
from bitcoin.rpc import RawProxy
p = RawProxy()
txhash = raw_input("Enter a transaction id: ")
raw_tx = p.getrawtransaction(txhash)
decoded_tx = p.decoderawtransaction(raw_tx)
total = 0
transactionvalue = 0
for item in decoded_tx['vin']:
        vout = item['vout']
        txid = item['txid']
        raw_txN = p.getrawtransaction(txid)
        decoded_txN = p.decoderawtransaction(raw_txN)
        for output in decoded_txN['vout']:
                if(output['n'] == vout):
                        total = total + output['value']
for output in decoded_tx['vout']:
        transactionvalue = transactionvalue + output['value']
transactionfee = total - transactionvalue
print transactionfee
```
![image](https://user-images.githubusercontent.com/12825358/71217030-12f26a80-22c5-11ea-9c65-6284cf2e33d6.png)

### 2.1 Programa, kuri iš atitinkamos bloko header'io informacijos "patikrina", kad bloko hash'as yra teisingas.

```python
from bitcoin.rpc import RawProxy

import hashlib
import binascii

p = RawProxy()

tx_value_out = 0

tx_value_in = 0

txid = raw_input("Enter a transaction id: ")

raw_tx = p.getrawtransaction(txid)

decoded_tx = p.decoderawtransaction(raw_tx)

for output in decoded_tx['vin']:
        tx_i = output['txid']
        vout = output['vout']
        raw_out = p.getrawtransaction(tx_i)
        decoded_tx_out = p.decoderawtransaction(raw_out)
        for out in decoded_tx_out['vout']:
                if out['n'] == vout:
                        tx_value_in = tx_value_in + out['value']

for output2 in decoded_tx['vout']:
        tx_value_out = tx_value_out + output2['value']



print(tx_value_in - tx_value_out)

#Check if block is valid

blockheight = 277316

# Get the block hash of block with height 277316
blockhash = p.getblockhash(blockheight)

# Retrieve the block by its hash
block = p.getblock(blockhash)

version = block['versionHex']
previous_hash = block['previousblockhash']
merkle_root = block['merkleroot']
time = block['time']
bits = block['bits']
nonce = block['nonce']

nonce = hex(int(0x100000000) + int(nonce))[-8:]
time = hex(int(0x100000000) + int(time))[-8:]

version = binascii.hexlify(binascii.unhexlify(version)[::-1])
previous_hash = binascii.hexlify(binascii.unhexlify(previous_hash)[::-1])
merkle_root = binascii.hexlify(binascii.unhexlify(merkle_root)[::-1])
time = binascii.hexlify(binascii.unhexlify(time)[::-1])
bits = binascii.hexlify(binascii.unhexlify(bits)[::-1])
nonce = binascii.hexlify(binascii.unhexlify(nonce)[::-1])

header = version + previous_hash + merkle_root + time + bits + nonce

header = binascii.unhexlify(header)
hash = hashlib.sha256(hashlib.sha256(header).digest()).digest()
hash = binascii.hexlify(hash)

hash = binascii.hexlify(binascii.unhexlify(hash)[::-1])

print("Result:")
print(hash)
print("Original hash")
print(blockhash)
```

![image](https://user-images.githubusercontent.com/12825358/71217300-fa368480-22c5-11ea-999d-c8dc165bc4b8.png)
