# 2. How Nexa works

## Transactions, Blocks, Mining, and the Blockchain

The Nexa system, unlike traditional banking and payment systems, is based on de-centralized trust. Instead of a central trusted authority, in Nexa, trust is achieved as an emergent property from the interactions of different participants in the Nexa system. In this chapter, we will examine Nexa from a high level by tracking a single transaction through the Nexa system and watch as it becomes "trusted" and accepted by the Nexa mechanism of distributed consensus and is finally recorded on the blockchain, the distributed ledger of all transactions.

Each example is based on an actual transaction made on the Nexa network, simulating the interactions between the users (Joe, Alice, and Bob) by sending funds from one wallet to another. While tracking a transaction through the Nexa network and blockchain, we will use a _blockchain explorer_ site to visualize each step. A blockchain explorer is a web application that operates as a Nexa search engine, in that it allows you to search for addresses, transactions, and blocks and see the relationships and flows between them.

### Popular blockchain explorers include

- [Explorer by Bitcoin.com](https://explorer.bitcoin.com/bch)
- [Blockchair](https://blockchair.com/nexa/blocks)

Each of these has a search function that can take an address, transaction hash, or block number and find the equivalent data on the Nexa network and blockchain. With each example, we will provide a URL that takes you directly to the relevant entry, so you can study it in detail.

#### Nexa Overview

In the overview diagram below, we see that the Nexa system consists of users with wallets containing keys, transactions that are propagated across the network, and miners who produce (through competitive computation) the consensus blockchain, which is the authoritative ledger of all transactions. In this chapter, we will trace a single transaction as it travels across the network and examine the interactions between each part of the Nexa system, at a high level. Subsequent chapters will delve into the technology behind wallets, mining, and merchant systems.

<spacer></spacer>
![Nexa Overview](/images/mastering-nexa/msbt_0201.png)
<image-caption>Figure 1. Nexa overview</image-caption>

#### Buying a Cup of Coffee

Alice, introduced in the previous chapter, is a new user who has just acquired her first Nexa. Alice met with her friend Joe to exchange some cash for Nexa. The transaction created by Joe funded Alice’s wallet with 0.0138 NEXA. Now Alice will make her first retail transaction, buying a cup of coffee at Bob’s coffee shop in Palo Alto, California. Bob’s coffee shop recently started accepting Nexa payments, by adding a Nexa option to his point-of-sale system. The prices at Bob’s Cafe are listed in the local currency (US dollars), but at the register, customers have the option of paying in either dollars or Nexa. Alice places her order for a cup of coffee and Bob enters the transaction at the register. The point-of-sale system will convert the total price from US dollars to Nexa at the prevailing market rate and display the prices in both currencies, as well as show a QR code containing a _payment request_ for this transaction (see [Payment request QR code](#payment-request-QR) (Hint: Try to scan this!)

```bash
Total:
    $1.50 USD
    0.00208507 NEXA
```

<anchor name="payment-request-QR"></anchor>
<spacer></spacer>
![payment-request-image](/images/mastering-nexa/msbt_02_receive.png)
<image-caption>Figure 2. Payment request QR code (Hint: Try to scan this!)</image-caption>
<spacer></spacer>

Alice uses her smartphone to scan the barcode on display. Her smartphone shows a payment of 0.00208507 NEXA to Bob's Cafe and she selects Send to authorize the payment. Within a few seconds (about the same amount of time as a credit card authorization), Bob would see the transaction on the register, completing the transaction.

In the following sections we will examine this transaction in more detail, see how Alice’s wallet constructed it, how it was propagated across the network, how it was verified, and finally, how Bob can spend that amount in subsequent transactions.

<tip nature="note">
  The Nexa network can transact in fractional values, e.g., from milli-bitcoins (1/1000th of a Nexa) down to 1/100,000,000th of a Nexa, which is known as a satoshi. Throughout this book we’ll use the term “Nexa” to refer to any quantity of Nexa currency, from the smallest unit (1 satoshi) to the total number (21,000,000) of all bitcoins that will ever be mined.
</tip>

### Nexa Transactions

In simple terms, a transaction tells the network that the owner of a number of Nexa has authorized the transfer of some of those Nexa to another owner. The new owner can now spend these Nexa by creating another transaction that authorizes transfer to another owner, and so on, in a chain of ownership.

Transactions are like lines in a double-entry bookkeeping ledger. In simple terms, each transaction contains one or more "inputs," which are debits against a Nexa account. On the other side of the transaction, there are one or more "outputs," which are credits added to a Nexa account. The inputs and outputs (debits and credits) do not necessarily add up to the same amount. Instead, outputs add up to slightly less than inputs and the difference represents an implied "transaction fee," which is a small payment collected by the miner who includes the transaction in the ledger. A Nexa transaction is shown as a bookkeeping ledger entry in [Transaction as double-entry bookkeeping](#transaction-double-entry).

The transaction also contains proof of ownership for each amount of Nexa (inputs) whose value is transferred, in the form of a digital signature from the owner, which can be independently validated by anyone. In Nexa terms, "spending" is signing a transaction that transfers value from a previous transaction over to a new owner identified by a Nexa address.

<tip nature="note">
  <i>Transactions</i> move value from <i>transaction inputs</i> to <i>transaction outputs</i>. An input is where the coin value is coming from, usually a previous transaction’s output. A transaction output assigns a new owner to the value by associating it with a key. The destination key is called an <i>encumbrance</i>. It imposes a requirement for a signature for the funds to be redeemed in future transactions. Outputs from one transaction can be used as inputs in a new transaction, thus creating a chain of ownership as the value is moved from address to address (see <link text="A chain of transactions, where the output of one transaction is the input of the next transaction" to="#blockchain-mnemonic"></link>)
</tip>

<anchor name="transaction-double-entry"></anchor>
<spacer></spacer>
![TransactionDoubleEntry](/images/mastering-nexa/transaction-as-double-entry-bookkepping.png)
<image-caption>Figure 3. Transaction as double-entry bookkeeping</image-caption>

<anchor name="blockchain-mnemonic"></anchor>
<spacer></spacer>
![Transaction chain](/images/mastering-nexa/chain-of-transactions.png)
<image-caption>Figure 4. A chain of transactions, where the output of one transaction is the input of the next transaction</image-caption>
<spacer></spacer>

Alice’s payment to Bob’s Cafe uses a previous transaction as its input. In the previous chapter Alice received Nexa from her friend Joe in return for cash. That transaction has a number of Nexa locked (encumbered) against Alice’s key. Her new transaction to Bob’s Cafe references the previous transaction as an input and creates new outputs to pay for the cup of coffee and receive change. The transactions form a chain, where the inputs from the latest transaction correspond to outputs from previous transactions. Alice’s key provides the signature that unlocks those previous transaction outputs, thereby proving to the Nexa network that she owns the funds. She attaches the payment for coffee to Bob’s address, thereby "encumbering" that output with the requirement that Bob produces a signature in order to spend that amount. This represents a transfer of value between Alice and Bob. This chain of transactions, from Joe to Alice to Bob, is illustrated in [A chain of transactions, where the output of one transaction is the input of the next transaction](#blockchain-mnemonic).

#### Common Transaction Forms

The most common form of transaction is a simple payment from one address to another, which often includes some "change" returned to the original owner. This type of transaction has one input and two outputs and is shown in [Most common transaction](#transaction-common).

<anchor name="transaction-common"></anchor>
<spacer></spacer>
![Common Transaction](/images/mastering-nexa/msbt_0205.png)
<image-caption>Figure 5. Most common transaction</image-caption>
<spacer></spacer>

Another common form of transaction is one that aggregates several inputs into a single output (see [Transaction aggregating funds](#transaction-aggregating)). This represents the real-world equivalent of exchanging a pile of coins and currency notes for a single larger note. Transactions like these are sometimes generated by wallet applications to clean up lots of smaller amounts that were received as change for payments.

<anchor name="transaction-aggregating"></anchor>
<spacer></spacer>
![Aggregating Transaction](/images/mastering-nexa/msbt_0206.png)
<image-caption>Figure 6. Transaction aggregating funds</image-caption>
<spacer></spacer>

Finally, another transaction form that is seen often on the Nexa ledger is a transaction that distributes one input to multiple outputs representing multiple recipients (see [Transaction distributing funds](#transaction-distributing)). This type of transaction is sometimes used by commercial entities to distribute funds, such as when processing payroll payments to multiple employees.

<anchor name="transaction-distributing"></anchor>
<spacer></spacer>
![Distributing Transaction](/images/mastering-nexa/msbt_0207.png)
<image-caption>Figure 7. Transaction distributing funds</image-caption>

### Constructing a Transaction

Alice’s wallet application contains all the logic for selecting appropriate inputs and outputs to build a transaction to Alice’s specification. Alice only needs to specify a destination and an amount and the rest happens in the wallet application without her seeing the details. Importantly, a wallet application can construct transactions even if it is completely offline. Like writing a check at home and later sending it to the bank in an envelope, the transaction does not need to be constructed and signed while connected to the Nexa network. It only has to be sent to the network eventually for it to be executed.

#### Getting the Right Inputs

Alice’s wallet application will first have to find inputs that can pay for the amount she wants to send to Bob. Most wallet applications keep a small database of "unspent transaction outputs" that are locked (encumbered) with the wallet’s own keys. Therefore, Alice’s wallet would contain a copy of the transaction output from Joe’s transaction, which was created in exchange for cash. A Nexa wallet application that runs as a full-index client actually contains a copy of every unspent output from every transaction in the blockchain. This allows a wallet to construct transaction inputs as well as quickly verify incoming transactions as having correct inputs. However, because a full-index client takes up a lot of disk space, most user wallets run "lightweight" clients that track only the user’s own unspent outputs.

If the wallet application does not maintain a copy of unspent transaction outputs, it can query the Nexa network to retrieve this information, using a variety of APIs available by different providers or by asking a full-index node using the Nexa JSON RPC API. [Look up all the unspent outputs for Alice’s Nexa address](#example_2-1) shows a RESTful API request, constructed as an HTTP GET command to a specific URL. This URL will return all the unspent transaction outputs for an address, giving any application the information it needs to construct transaction inputs for spending. We use the simple command-line HTTP client _cURL_ to retrieve the response.

<anchor name="example_2-1"></anchor>
<spacer></spacer>
Example 1. Look up all the unspent outputs for Alice’s Nexa address

```bash
$ curl https://explorer.bitcoin.com/api/bch/addr/13wCzsosQfVSt86RNT1tegUKFhxmnpzoxw/utxo
```

<anchor name="example_2-2"></anchor>
<spacer></spacer>
Example 2. Response to the lookup

```json
[
  {
    "address": "13wCzsosQfVSt86RNT1tegUKFhxmnpzoxw",
    "txid": "3fd6e7fd56354a76072f84350023ead2da4427f5a17a223d16318de600dad76a",
    "vout": 1,
    "amount": 0.01386001,
    "satoshis": 1386001,
    "height": 538328,
    "confirmations": 0
  }
]
```

The response in [Response to the lookup](#example_2-2) shows one unspent output (one that has not been redeemed yet) under the ownership of Alice’s address 13wCzsosQfVSt86RNT1tegUKFhxmnpzoxw. The response includes the reference to the transaction in which this unspent output is contained (the payment from Joe) and its value in satoshis. With this information, Alice’s wallet application can construct a transaction to transfer that value to new owner addresses.

<tip>
  View the <link text="transaction from Joe to Alice" to="https://explorer.bitcoin.com/bch/tx/3fd6e7fd56354a76072f84350023ead2da4427f5a17a223d16318de600dad76a"></link>
</tip>

As you can see, Alice’s wallet contains enough bitcoins in a single unspent output to pay for the cup of coffee. Had this not been the case, Alice’s wallet application might have to "rummage" through a pile of smaller unspent outputs, like picking coins from a purse until it could find enough to pay for coffee. In both cases, there might be a need to get some change back, which we will see in the next section, as the wallet application creates the transaction outputs (payments).

#### Creating the Outputs

A transaction output is created in the form of a script that creates an encumbrance on the value and can only be redeemed by the introduction of a solution to the script. In simpler terms, Alice’s transaction output will contain a script that says something like, "This output is payable to whoever can present a signature from the key corresponding to Bob’s public address." Because only Bob has the wallet with the keys corresponding to that address, only Bob’s wallet can present such a signature to redeem this output. Alice will therefore "encumber" the output value with a demand for a signature from Bob.

This transaction will also include a second output, because Alice’s funds are in the form of a 0.01386001 NEXA output, too much money for the 0.00208507 NEXA cup of coffee. Alice will need 0.01177251 NEXA in change. Alice’s change payment is created _by Alice’s wallet_ in the very same transaction as the payment to Bob. Essentially, Alice’s wallet breaks her funds into two payments: one to Bob, and one back to herself. She can then use the change output in a subsequent transaction, thus spending it later.

Finally, for the transaction to be processed by the network in a timely fashion, Alice’s wallet application will add a small fee. This is not explicit in the transaction; it is implied by the difference between inputs and outputs. If instead of taking 0.01177251 in change, Alice creates only 0.01077251 as the second output, there will be 0.001 NEXA left over. The input’s 0.01386001 NEXA is not fully spent with the two outputs, because they will add up to less than 0.01386001. The resulting difference is the _transaction fee_ that is collected by the miner as a fee for including the transaction in a block and putting it on the blockchain ledger.

The resulting transaction can be seen using a blockchain explorer web application, as shown in [Alice’s transaction to Bob’s Cafe](#transaction-alice).

<anchor name="transaction-alice"></anchor>
<spacer></spacer>
![Alice Coffee Transaction](/images/mastering-nexa/msbt_02-tx-screenshot.png)
<image-caption>Figure 8. Alice’s transaction to Bob’s Cafe</image-caption>

<tip>
View the <link text="transaction from Alice to Bob’s Cafe" to="https://explorer.bitcoin.com/bch/tx/adc95fb479339517d9685119873466b520e6005bffb0965e48afaf9d3494055d"></link>
</tip>

<spacer></spacer>

#### Adding the Transaction to the Ledger

The transaction created by Alice’s wallet application is 226 bytes long and contains everything necessary to confirm ownership of the funds and assign new owners. Now, the transaction must be transmitted to the Nexa network where it will become part of the distributed ledger (the blockchain). In the next section we will see how a transaction becomes part of a new block and how the block is "mined." Finally, we will see how the new block, once added to the blockchain, is increasingly trusted by the network as more blocks are added.

#### Transmitting the transaction

Because the transaction contains all the information necessary to process, it does not matter how or where it is transmitted to the Nexa network. The Nexa network is a peer-to-peer network, with each Nexa client participating by connecting to several other Nexa clients. The purpose of the Nexa network is to propagate transactions and blocks to all participants.

#### How it propagates

Alice’s wallet application can send the new transaction to any of the other Nexa clients it is connected to over any Internet connection: wired, WiFi, or mobile. Her Nexa wallet does not have to be connected to Bob’s Nexa wallet directly and she does not have to use the Internet connection offered by the cafe, though both those options are possible, too. Any Nexa network node (other client) that receives a valid transaction it has not seen before will immediately forward it to other nodes to which it is connected. Thus, the transaction rapidly propagates out across the peer-to-peer network, reaching a large percentage of the nodes within a few seconds.

#### Bob’s view

If Bob’s Nexa wallet application is directly connected to Alice’s wallet application, Bob’s wallet application might be the first node to receive the transaction. However, even if Alice’s wallet sends the transaction through other nodes, it will reach Bob’s wallet within a few seconds. Bob’s wallet will immediately identify Alice’s transaction as an incoming payment because it contains outputs redeemable by Bob’s keys. Bob’s wallet application can also independently verify that the transaction is well formed, uses previously unspent inputs, and contains sufficient transaction fees to be included in the next block. At this point Bob can assume, with little risk, that the transaction will shortly be included in a block and confirmed.

<tip>
  A common misconception about Nexa transactions is that they must be "confirmed" by waiting 10 minutes for a new block, or up to 60 minutes for a full six confirmations. Although confirmations ensure the transaction has been accepted by the whole network, such a delay is unnecessary for small-value items such as a cup of coffee. A merchant may accept a valid small-value transaction with no confirmations, with no more risk than a credit card payment made without an ID or a signature, as merchants routinely accept today.
</tip>

### Nexa Mining

The transaction is now propagated on the Nexa network. It does not become part of the shared ledger (the _blockchain_) until it is verified and included in a block by a process called _mining_. See [Mining and Consensus](/mastering-nexa/7-mining-and-consensus/) for a detailed explanation.

The Nexa system of trust is based on computation. Transactions are bundled into _blocks_, which require an enormous amount of computation to prove, but only a small amount of computation to verify as proven. The mining process serves two purposes in Nexa:

- Mining creates new Nexa in each block, almost like a central bank printing new money. The amount of Nexa created per block is fixed and diminishes with time.
- Mining creates trust by ensuring that transactions are only confirmed if enough computational power was devoted to the block that contains them. More blocks mean more computation, which means more trust.

A good way to describe mining is like a giant competitive game of sudoku that resets every time someone finds a solution and whose difficulty automatically adjusts so that it takes approximately 10 minutes to find a solution. Imagine a giant sudoku puzzle, several thousand rows and columns in size. If I show you a completed puzzle you can verify it quite quickly. However, if the puzzle has a few squares filled and the rest are empty, it takes a lot of work to solve! The difficulty of the sudoku can be adjusted by changing its size (more or fewer rows and columns), but it can still be verified quite easily even if it is very large. The "puzzle" used in Nexa is based on a cryptographic hash and exhibits similar characteristics: it is asymmetrically hard to solve but easy to verify, and its difficulty can be adjusted.

Earlier we introduced Jing, a computer engineering student in Shanghai. Jing is participating in the Nexa network as a miner. Every 10 minutes or so, Jing joins thousands of other miners in a global race to find a solution to a block of transactions. Finding such a solution, the so-called proof of work, requires quadrillions of hashing operations per second across the entire Nexa network. The algorithm for proof of work involves repeatedly hashing the header of the block and a random number with the SHA256 cryptographic algorithm until a solution matching a predetermined pattern emerges. The first miner to find such a solution wins the round of competition and publishes that block into the blockchain.

Jing started mining in 2010 using a very fast desktop computer to find a suitable proof of work for new blocks. As more miners started joining the Nexa network, the difficulty of the problem increased rapidly. Soon, Jing and other miners upgraded to more specialized hardware, such as high-end dedicated graphical processing units (GPUs) cards such as those used in gaming desktops or consoles. At the time of this writing, the difficulty is so high that it is profitable only to mine with application-specific integrated circuits (ASIC), essentially hundreds of mining algorithms printed in hardware, running in parallel on a single silicon chip. Jing also joined a "mining pool," which much like a lottery pool allows several participants to share their efforts and the rewards. Jing now runs two USB-connected ASIC machines to mine for Nexa 24 hours a day. He pays his electricity costs by selling the Nexa he is able to generate from mining, creating some income from the profits. His computer runs a copy of bitcoind, the reference Nexa client, as a backend to his specialized mining software.

### Mining Transactions in Blocks

A transaction transmitted across the network is not verified until it becomes part of the global distributed ledger, the blockchain. Every 10 minutes on average, miners generate a new block that contains all the transactions since the last block. New transactions are constantly flowing into the network from user wallets and other applications. As these are seen by the Nexa network nodes, they get added to a temporary pool of unverified transactions maintained by each node. As miners build a new block, they add unverified transactions from this pool to a new block and then attempt to solve a very hard problem (a.k.a., proof of work) to prove the validity of that new block. The process of mining is explained in detail in [Mining and Consensus](/mastering-nexa/7-mining-and-consensus/).

Transactions are added to the new block, prioritized by the highest-fee transactions first and a few other criteria. Each miner starts the process of mining a new block of transactions as soon as he receives the previous block from the network, knowing he has lost that previous round of competition. He immediately creates a new block, fills it with transactions and the fingerprint of the previous block, and starts calculating the proof of work for the new block. Each miner includes a special transaction in his block, one that pays his own bitcoin cash address a reward of newly created bitcoins (currently 12.5 NEXA per block). If he finds a solution that makes that block valid, he "wins" this reward because his successful block is added to the global blockchain and the reward transaction he included becomes spendable. Jing, who participates in a mining pool, has set up his software to create new blocks that assign the reward to a pool address. From there, a share of the reward is distributed to Jing and other miners in proportion to the amount of work they contributed in the last round.

Alice’s transaction was picked up by the network and included in the pool of unverified transactions. Because it had sufficient fees, it was included in a new block generated by Jing’s mining pool. Approximately five minutes after the transaction was first transmitted by Alice’s wallet, Jing’s ASIC miner found a solution for the block and published it as block #538345, containing 680 other transactions. Jing’s ASIC miner published the new block on the Nexa network, where other miners validated it and started the race to generate the next block.

You can see the block that includes [Alice’s transaction](https://explorer.bitcoin.com/bch/block/000000000000000001679e7ef188dbca25fe9384b20009c3778d25b340938a19).

A few minutes later, a new block, #538346, is mined by another miner. Because this new block is based on the previous block (#538345) that contained Alice’s transaction, it added even more computation on top of that block, thereby strengthening the trust in those transactions. The block containing Alice’s transaction is counted as one "confirmation" of that transaction. Each block mined on top of the one containing the transaction is an additional confirmation. As the blocks pile on top of each other, it becomes exponentially harder to reverse the transaction, thereby making it more and more trusted by the network.

In the diagram in [Alice’s transaction included in block #538345](#block-alice1) we can see block #538345, which contains Alice’s transaction. Below it are 538,344 blocks (including block #0), linked to each other in a chain of blocks (blockchain) all the way back to block #0, known as the _genesis block_. Over time, as the "height" in blocks increases, so does the computation difficulty for each block and the chain as a whole. The blocks mined after the one that contains Alice’s transaction act as further assurance, as they pile on more computation in a longer and longer chain. By convention, any block with more than six confirmations is considered irrevocable, because it would require an immense amount of computation to invalidate and recalculate six blocks.

<anchor name="block-alice1"></anchor>
<spacer></spacer>
![Alice’s transaction included in a block](/images/mastering-nexa/transaction-included-in-a-block.png)
<image-caption>Figure 9. Alice’s transaction included in block #538345</image-caption>
<spacer></spacer>

### Spending the Transaction

Now that Alice’s transaction has been embedded in the blockchain as part of a block, it is part of the distributed ledger of Nexa and visible to all Nexa applications. Each Nexa client can independently verify the transaction as valid and spendable. Full-index clients can track the source of the funds from the moment the bitcoins were first generated in a block, incrementally from transaction to transaction, until they reach Bob’s address. Lightweight clients can do what is called a simplified payment verification by confirming that the transaction is in the blockchain and has several blocks mined after it, thus providing assurance that the network accepts it as valid.

Bob can now spend the output from this and other transactions, by creating his own transactions that reference these outputs as their inputs and assign them new ownership. For example, Bob can pay a contractor or supplier by transferring value from Alice’s coffee cup payment to these new owners. Most likely, Bob’s Nexa software will aggregate many small payments into a larger payment, perhaps concentrating all the day’s Nexa revenue into a single transaction. This would move the various payments into a single address, used as the store’s general "checking" account. For a diagram of an aggregating transaction, see [Transaction aggregating funds](#transaction-aggregating).

As Bob spends the payments received from Alice and other customers, he extends the chain of transactions, which in turn are added to the global blockchain ledger for all to see and trust. Let’s assume that Bob pays his web designer Gopesh in Bangalore for a new website page. Now the chain of transactions will look like [Alice’s transaction as part of a transaction chain from Joe to Gopesh](#block-alice2).

<anchor name="block-alice2"></anchor>
<spacer></spacer>
![Alice’s transaction as part of a transaction chain](/images/mastering-nexa/msbt_0210.png)
<image-caption>Figure 10. Alice’s transaction as part of a transaction chain from Joe to Gopesh</image-caption>
<spacer></spacer>
