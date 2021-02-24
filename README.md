# Tamarin-SAPIC-Guide

This document will explain how to wtrite a protocol using tamarin SAPIC (process based fact)


## Setup
We recommend to download Tamarin using the steps that avaliable at their offial website (<https://tamarin-prover.github.io/manual/book/002_installation.html>). 

Or use the commands below in linux-based machine for quick installation (This is for Ubunut installation).
```
sudo apt update && sudo apt upgrade

sudo apt install curl

/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

brew install tamarin-prover/tap/tamarin-prover
```
For text editing, I recommend using VScode (<https://code.visualstudio.com/>). Download Tamarin extension for VScode for a better readiblity 

## Writing a protocol

The syntax of Sapic goes as delcaring processes. Tamarin will convert all the processes into facts. So you don't have to worry much. Here is an example about the main layout.
```
theory SimpleProtocol 
begin

builtins:
	symmetric-encryption, multiset,	hashing,	signing,	asymmetric-encryption
  
let P1 = 
...
..
.

let P2 =
.
..
.

//Main process running
(!P1 || P2 )

end
```

The protocol start with a theory name. Here we used `SimpleProtocol` as a name. Then, the protocol should be written between `begin` and `end`. Anything outside this boundry might no coinsidred part of the protocol. We have here two processes `p1` and `p2`. The main process is where you mentioned what you want to run from the processes. Here we use `!` to denotes mutiple instances of the process `P1` is running. While only instance of `P2` is there. using `||` mean running in parallel. You can declare as much processes you want and have either mutiple instances or single instannces using '!' prefix.


### Syntax

Each statment inside the proccess must termianted with `;` . Except :
1. a statment end with `in`
2. last event of a process.

See the example below:







Here, we send a message from Client to a server. We have multiple instances of a client and one server. You can see the whole processes there. Events are like traces int Tamarin we use them in lemma to find out what happining and prove the model. Note that, texts like ``toServer`` should not contain underscore in it. Something like `'to_server'` can cause a synatx error. 


Thats all for now and until next time :D

## Writing lemmas
tbc
