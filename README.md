# Blockchain-based-Ledger-System-with-Streamlit

## Overview
Streamlit web application of a blockchain-based ledger which takes bank transaction inputs of "sender", "receiver", and "amount". Blockchain includes cryptographic hashing of the data, chaining, a dynamic proof of work system for adding new blocks, and blockchain validation.  Output is a blockchain as a dataframe with record, cryptographic hash from previous block, timestamp, and nonce (a number used once) for proof of work. Includes "Validate Chain" button to validate the chain. Side bar provides user defined block difficulty for the hashing process and pulling individual block information.

![App](https://user-images.githubusercontent.com/46635638/143726474-51612cd4-2317-4d5e-87e3-bad3e3d2f8fe.PNG)


## Usage

### Libraries

````
import streamlit as st
from dataclasses import dataclass
from typing import Any, List
import datetime as datetime
import pandas as pd
import hashlib

````

### Block and Record Data Class Creation

````
@dataclass
class Record:
    sender: str
    receiver: str
    amount: float
    

@dataclass
class Block:

    # @TODO
    # Rename the `data` attribute to `record`, and set the data type to `Record`
    record: Record

    creator_id: int
    prev_hash: str = "0"
    timestamp: str = datetime.datetime.utcnow().strftime("%H:%M:%S")
    nonce: str = 0

````

### Function for Hashing a Block

````
def hash_block(self):
        sha = hashlib.sha256()

        record = str(self.record).encode()
        sha.update(record)

        creator_id = str(self.creator_id).encode()
        sha.update(creator_id)

        timestamp = str(self.timestamp).encode()
        sha.update(timestamp)

        prev_hash = str(self.prev_hash).encode()
        sha.update(prev_hash)

        nonce = str(self.nonce).encode()
        sha.update(nonce)

        return sha.hexdigest()

````


### Chaining Blocks Data Class

````
class PyChain:
    chain: List[Block]
    difficulty: int = 4
````

### Proof of Work Function

````
def proof_of_work(self, block):

        calculated_hash = block.hash_block()

        num_of_zeros = "0" * self.difficulty

        while not calculated_hash.startswith(num_of_zeros):

            block.nonce += 1

            calculated_hash = block.hash_block()

        print("Wining Hash", calculated_hash)
        return block
````

### Adding Block to Chain Function
 ````
 def add_block(self, candidate_block):
        block = self.proof_of_work(candidate_block)
        self.chain += [block]
 ````
 
Below is an example of the blockchain created with sampled inputs:
 
![App_ledger](https://user-images.githubusercontent.com/46635638/143728106-c1669ee3-451b-4319-8443-81b3c9cf20e7.PNG)

 
### Blockchain Validation Function

````
def is_valid(self):
        block_hash = self.chain[0].hash_block()

        for block in self.chain[1:]:
            if block_hash != block.prev_hash:
                print("Blockchain is invalid!")
                return False

            block_hash = block.hash_block()

        print("Blockchain is Valid")
        return True
````

Below we see an example of using the validation button developed by the function above, which returns "True" if all blocks in the chain are valid:
![App](https://user-images.githubusercontent.com/46635638/143728236-7ca35b02-f47f-428d-bc85-aab88710dc4f.PNG)

### Caching in Streamlit
Most important is the ability to use caching in streamlit in order to initialize and chain the blocks

````
@st.cache(allow_output_mutation=True)
def setup():
    print("Initializing Chain")
    return PyChain([Block({'sender': 'Genesis', 'receiver': 'Genesis', 'amount': 'Genesis'}, 0)])

````

### Run the Application
1. In the terminal, navigate to the project folder where you've coded the
  the "completed_pychain.py" file.

2. In a seperate terminal, run the Streamlit application by
using `streamlit run completed_pychain.py`.
