# Tamarin SAPIC Guide

This document will explain how to write a protocol using tamarin SAPIC (process based fact).


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
	symmetric-encryption, multiset,	hashing, signing, asymmetric-encryption
  
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

The protocol start with a theory name. Here we used `SimpleProtocol` as a name. Then, the protocol should be written between `begin` and `end`. Anything outside this boundry might no coinsidred part of the protocol. Order of the proccess dose not matter. We have here two processes `p1` and `p2`. The main process is where you mentioned what you want to run from the processes. Here we use `!` to denotes mutiple instances of the process `P1` is running. While only instance of `P2` is there. using `||` mean running in parallel. You can declare as much processes you want and have either mutiple instances or single instannces using '!' prefix.


### Syntax

See the example below:

```
theory HelloWorld
begin

builtins:
	symmetric-encryption,
	multiset,
	hashing


let Server =
	new ~S_id;
	out(~S_id);
	!(
		in(<'toServer',h(secret)>);

		event S_recieve_secret(~S_id,secret,h(secret));
		event S_finish()	
	)
	

let Client =
	new ~C_id;
	out(~C_id);
	!(
		new ~secret;
		out(<'toServer',h(~secret)>);

		event C_send_secret(~C_id,~secret,h(~secret));
		event C_finish()
	)

// Main process starts here
new ~sharedKey; (!Client || Server)

lemma CorrectnessS1:
exists-trace
	"Ex #i. S_finish()@i"

lemma CorrectnessC1:
exists-trace
	"Ex #i. C_finish()@i"

end

```


Here, we send a message from Client to a server. We have multiple instances of a client and one server. You can see the whole processes there.
Few important points:
1. Global variables like `sharedkey` can be used in any proccess. They are useful for shared secret or keys.
2. Texts like ``toServer`` should not contain underscore in it. Something like `'to_server'` can cause a synatx error. 
3. All statments end up with `;` excepts the last event of a process or a statment end with `in`.
4. `~` prefix indicate a fresh value used to create keys, secret ids etc. Within the procces that a fresh value declared, you need always to add `~` for each variable there. But you should not do it if a variable recvied by a different proccess that do not know that it is a fresh value. You can see that `secret ` is fresh in client but not fresh prefix in server. if the fresh variable is global like `shareKey` then you should put '~' prefix everywhere.
5. Events start with `event` keyword. They are like traces in Tamarin. we use them in lemmas to find out what happining and prove the model. 
6. `!( ...)` indicate that the code inside can be run mutliple times. while anything outside runs once per process instance. For a web server, this is important to answer all the requests forexample. 

Thats all for now and until next time :D

## Read and write from proccess (insert and lookup)

You can insert values in a process and read it from a different proccess as follow: 

```
theory InsertAndLookUp 
begin

builtins:
	symmetric-encryption, multiset,	hashing, signing, asymmetric-encryption
  
let P1 = 
...
	new ~apikey;
	insert 'apikeylabel', ~apikey; 
.

let P2 =
...
	lookup 'apikeylabel' as apikey in 
.

//Main process running
(!P1 || P2 )

end
```

1. Insert syntax is `insert <label>,<variable>`.
2. Lookup like this `lookup <label> as <variable>`

Note that you should keep label unique through the whole protocol.

For tuble reading you need do fetching like this:

```
insert 'label', <~x,~y>;
..
lookup 'label' as values
let x = fst(values) in
let y = snd(values) in
..
```

That's it for now. until next time :D
## Writing lemmas
tbc
