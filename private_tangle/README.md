# Private Tangle

At one point the IF removed the instructions to create a private tangle from ground up. Their intention is that everything should run in the mainnet and did remove the documentation therefore.
So here it is.

### Cloning the repo and building hornet

First of all the repo should be cloned from the official GitHub.

```bash
git clone https://github.com/iotaledger/hornet.git
```

cd into it and build hornet with <a href="https://go.dev/doc/install">Go</a> by executing the build script inside the scripts folder while being in the hornet root folder. Today (19.05.2023) version 1.20 of go is needed. You can check your version by typing ```go version``` in your console.

```bash
cd hornet && ./scripts/build_hornet_rocksdb.sh
```

### Building the genesis block

```!!!In this section, keys will be created. Please save those somewhere save. Without them you will lose control over your funds and the coordinator!!!!```

First we copy the default config file:

```bash
cp config_defaults.json config.json
```

After that we configure the protocol parameters by editing the protocol_parameters.json in the private_tangle folder with the editor of your choice:

```bash
nano private_tangle/protocol_parameters.json
```

Here we configure stuff like 
* bech32HRP - the prefix of the addresses where funds can be sent to
* minPoWScore - the difficulty for the pow (0 equal to none)
* tokenSupply - total token supply in the network
* etc.

After that we need to create an ed25519 key and get the corresponding address to it.
We can archive this by executing following command:
```bash
./hornet tools ed25519-key --hrp frank
```

Note, that we define with --hrp what bech32HRP we have. You should enter the value you defined in the protocol_parameters.json file or else the default value (rms) will be used

Your output should look something like this:

```
Your seed BIP39 mnemonic:  hurdle use eyebrow stem damage around salad six tribe mind mother text own merit piano pupil private autumn scout bind ghost anchor half funny

Your BIP32 path:           m/44'/4218'/0'/0'/0'
Your ed25519 private key:  c51f5aa697d2c3b17708d9b79e6d1daabbd554783e16ca245ca52e1132351e02676e7615ddb8c8ea2935d51ce3d94000c4ef594e20e64313c1b790bedc2924b4
Your ed25519 public key:   676e7615ddb8c8ea2935d51ce3d94000c4ef594e20e64313c1b790bedc2924b4
Your ed25519 address:      641f5e09b3f43404d3a0d0c3a3af6fdf3afe6a7ae53d2e84f45a23062a607b67
Your bech32 address:       frank1qpjp7hsfk06rgpxn5rgv8ga0dl0n4ln20tjn6t5y73dzxp32vpakw5f5ger
```

We will need the bech32 address from this output to send the initial funds to it.

We create now some folders, where we put all our stuff in later:

```bash
mkdir shimmer && mkdir shimmer/database && mkdir shimmer/snapshots
```

After that, with this command we create the genesis binary:

```bash
./hornet tools snap-gen --protocolParametersPath private_tangle/protocol_parameters.json --mintAddress smr1qpzdgq78ju8z7php2q6pysem6hfmtrfmhnlvd4wr4c2re0j5js7zjquskjv --outputPath shimmer/snapshots/genesis_snapshot.bin
```

Note, that we provide the path to the protocol_parameters.json, where we defined the rules for our private tangle and the address we generated the step before.

To make things easier in the later config the copy the file and name it ```full_snapshot.bin```

```bash
cp shimmer/snapshots/genesis_snapshot.bin shimmer/snapshots/full_snapshot.bin
```


### Building the coordinator initial state

Now we set up the coordinator. The coordinator runs as plugin in the hornet node and needs some configuring.

First we need some key/address pairs for the coordinator, so it can sign milestones into the network. We do that with the hornet tools, which we used before:

```bash
./hornet tools ed25519-key --hrp frank
```
Do not forget to set the right hrp.

You can do this process as often as you want. You sure ask why the coordinator needs several keys. In case of one key gets exposed, the coordinator can "rotate" keys so the old one won't be used anymore. Sadly, the nodes need to be reconfigured if this happens but the downtime of the coordinator is minimal

<details>
  <summary>I've created it 7 times</summary>

## 1
```
Your seed BIP39 mnemonic:  tail dry quiz eight way general lake light banner alone arctic shrug hawk check knife glad bridge blossom olympic gun roof brand fluid fee

Your BIP32 path:           m/44'/4218'/0'/0'/0'
Your ed25519 private key:  26211432de3c5a8839c2d21fe593ac6791298627cdf449278ec2fd11dc2a1756e28418dad96cd5645e9a683ab1a3c7af38c18e2e42e2c353c87326f0beb85ccd
Your ed25519 public key:   e28418dad96cd5645e9a683ab1a3c7af38c18e2e42e2c353c87326f0beb85ccd
Your ed25519 address:      dee911b8fb98b0d866e2648e9f3e76fe591d4e5f287cf92b0de65be239e75417
Your bech32 address:       frank1qr0wjydclwvtpkrxufjga8e7wml9j82wtu58e7ftphn9hc3eua2pw0x75hp
```
## 2
```
Your seed BIP39 mnemonic:  aspect gesture clean curtain office usage frame cattle fix horror arrange loyal silly cable agent cotton always order attract little average shield canal muffin

Your BIP32 path:           m/44'/4218'/0'/0'/0'
Your ed25519 private key:  ae1113879cff72edd7929069d2dc1047994b21551a1ecf6e77237ce698a809f19503b9aafe6f9b5c5ed0a3af3972e47ca477325f68091822cf0d0546f199187b
Your ed25519 public key:   9503b9aafe6f9b5c5ed0a3af3972e47ca477325f68091822cf0d0546f199187b
Your ed25519 address:      0862de9a69fa14b782869d93fece1a7c4b0052b68bf1fd6b44b9bc47bd512c94
Your bech32 address:       frank1qqyx9h56d8apfduzs6we8lkwrf7ykqzjk69lrlttgjumc3aa2ykfgwspgmq
```
## 3
```
Your seed BIP39 mnemonic:  tank balcony diesel service truck monster gain grunt apple elite right pledge cabin never near clean inch ethics acid all filter wash cement strike

Your BIP32 path:           m/44'/4218'/0'/0'/0'
Your ed25519 private key:  a17a0c8bf1980bd3f7dc71cde87afca799b9584cc66399bc63fbb0e9d014ddf8cd7c1a031b0110e480a17f52dc5e22555a8648c7bc6114b0374256f6e2333345
Your ed25519 public key:   cd7c1a031b0110e480a17f52dc5e22555a8648c7bc6114b0374256f6e2333345
Your ed25519 address:      183051c89a9f7426098519fa97d875d51e98196cb971e0e5e9785aa87aeda883
Your bech32 address:       frank1qqvrq5wgn20hgfsfs5vl497cwh23axqedjuhrc89a9u942r6ak5gx76wau7
```
## 4
```
Your seed BIP39 mnemonic:  page celery typical bike tuition hint elevator rack tilt sleep order bottom another marriage doll silk grain equal yard hover yard kite ask diary

Your BIP32 path:           m/44'/4218'/0'/0'/0'
Your ed25519 private key:  6e25598de2485bc5b5a72ab6d6f0c5f9e4270d9ef1e87f663616e383f4a5c8697da8a4319bec0a2bc7a7df9e4bdd48c3ec1ac373d694b789631fa48dcc7bbecc
Your ed25519 public key:   7da8a4319bec0a2bc7a7df9e4bdd48c3ec1ac373d694b789631fa48dcc7bbecc
Your ed25519 address:      1831f01932f27bd40fd811d02bace2877af043b550d8901cf666e4f3b3fa1889
Your bech32 address:       frank1qqvrruqexte8h4q0mqgaq2avu2rh4uzrk4gd3yqu7enwfuanlgvgjzrczt6
```
## 5
```
Your seed BIP39 mnemonic:  double hospital jungle prepare hamster mixed twist salon demise remember length survey grace breeze oil rail symbol two novel knock lumber crop unaware main

Your BIP32 path:           m/44'/4218'/0'/0'/0'
Your ed25519 private key:  0e9d0428f16d6cc6746e87fbba428b666fab0240f708b793af1f7b34dbb3b6212df4a3fdf8d6d02bad05f792f57d1bca8a3c7a8ebabc5948695087359f2b659a
Your ed25519 public key:   2df4a3fdf8d6d02bad05f792f57d1bca8a3c7a8ebabc5948695087359f2b659a
Your ed25519 address:      fd46c52132f493bafdfd5156059bbd0c3bde246dacd7be7240c8a8f088280a91
Your bech32 address:       frank1qr75d3fpxt6f8whal4g4vpvmh5xrhh3ydkkd00njgry23uyg9q9fz39peef
```
## 6
```
Your seed BIP39 mnemonic:  inform odor intact verb raw bind chalk clock rack iron key fold piece mean jaguar robot puzzle cake prosper winner barrel reduce atom business

Your BIP32 path:           m/44'/4218'/0'/0'/0'
Your ed25519 private key:  a5ec89f65e48c4d0dcec325ab637359db38df0d4fe60741df47d8ec08d5be9226a792615891c3c3223052a4b48dde4324e6c38827e10cfe0a22f3a67bc598b6a
Your ed25519 public key:   6a792615891c3c3223052a4b48dde4324e6c38827e10cfe0a22f3a67bc598b6a
Your ed25519 address:      5e4281bffdf1f6e9a7be46fef59d2f4fa2b43356204d0cca3a0b71b23d5db936
Your bech32 address:       frank1qp0y9qdllhcld6d8her0aava9a869dpn2csy6rx28g9hrv3atkunvc0q6h8
```
## 7
``` 
Your seed BIP39 mnemonic:  exit soul thunder evolve deal sword radio strong sauce upper deputy later vocal story happy shy mom urban junior spider uphold okay fade rhythm

Your BIP32 path:           m/44'/4218'/0'/0'/0'
Your ed25519 private key:  f5da2339b6c014dba4306f50ad88a3af44758abefa067d60032e602681017e59d11a223f38de24aa850a9be47252fec24e5f4f3c5be82dd7c17c898a58fab58d
Your ed25519 public key:   d11a223f38de24aa850a9be47252fec24e5f4f3c5be82dd7c17c898a58fab58d
Your ed25519 address:      db1b510c7a6b00cc6692e33604669f5a70e7e89b7a613dac68e31ea4babd5a8b
Your bech32 address:       frank1qrd3k5gv0f4spnrxjt3nvprxnad8pelgndaxz0dvdr33af96h4dgkv7ykzz
```
</details>

You can configure how many keys should be taken by setting the milestonePublicKeyCount parameter.

We save those private keys into an environment variable named COO_PRV_KEYS, where we put in each key seperated by a comma (,)

```bash
export COO_PRV_KEYS=26211432de3c5a8839c2d21fe593ac6791298627cdf449278ec2fd11dc2a1756e28418dad96cd5645e9a683ab1a3c7af38c18e2e42e2c353c87326f0beb85ccd,ae1113879cff72edd7929069d2dc1047994b21551a1ecf6e77237ce698a809f19503b9aafe6f9b5c5ed0a3af3972e47ca477325f68091822cf0d0546f199187b,a17a0c8bf1980bd3f7dc71cde87afca799b9584cc66399bc63fbb0e9d014ddf8cd7c1a031b0110e480a17f52dc5e22555a8648c7bc6114b0374256f6e2333345,6e25598de2485bc5b5a72ab6d6f0c5f9e4270d9ef1e87f663616e383f4a5c8697da8a4319bec0a2bc7a7df9e4bdd48c3ec1ac373d694b789631fa48dcc7bbecc,0e9d0428f16d6cc6746e87fbba428b666fab0240f708b793af1f7b34dbb3b6212df4a3fdf8d6d02bad05f792f57d1bca8a3c7a8ebabc5948695087359f2b659a,a5ec89f65e48c4d0dcec325ab637359db38df0d4fe60741df47d8ec08d5be9226a792615891c3c3223052a4b48dde4324e6c38827e10cfe0a22f3a67bc598b6a,f5da2339b6c014dba4306f50ad88a3af44758abefa067d60032e602681017e59d11a223f38de24aa850a9be47252fec24e5f4f3c5be82dd7c17c898a58fab58d
```

Next we need to edit the config_defaults.json to insert those public keys we just generated:

```bash
nano config.json
```

Find the "publicKeyRanges" section and insert your public keys.

In my case it looks like this:

```json
"publicKeyRanges": [
  {
    "key": "e28418dad96cd5645e9a683ab1a3c7af38c18e2e42e2c353c87326f0beb85ccd",
    "start": 0,
    "end": 0
  },
  {
    "key": "9503b9aafe6f9b5c5ed0a3af3972e47ca477325f68091822cf0d0546f199187b",
    "start": 0,
    "end": 0
  },
  {
    "key": "cd7c1a031b0110e480a17f52dc5e22555a8648c7bc6114b0374256f6e2333345",
    "start": 0,
    "end": 0
  },
  {
    "key": "7da8a4319bec0a2bc7a7df9e4bdd48c3ec1ac373d694b789631fa48dcc7bbecc",
    "start": 0,
    "end": 0
  },
  {
    "key": "2df4a3fdf8d6d02bad05f792f57d1bca8a3c7a8ebabc5948695087359f2b659a",
    "start": 0,
    "end": 0
  },
  {
    "key": "6a792615891c3c3223052a4b48dde4324e6c38827e10cfe0a22f3a67bc598b6a",
    "start": 0,
    "end": 0
  },
  {
    "key": "d11a223f38de24aa850a9be47252fec24e5f4f3c5be82dd7c17c898a58fab58d",
    "start": 0,
    "end": 0
  }
]
```

While we are there we set the targetNetworkName to the one defined in the protocol_parameters.json:
```json
"protocol": {
    "targetNetworkName": "frank_tangle",
    "milestonePublicKeyCount": 7,
    "baseToken": {
```

and enable inx at the end of the configuration file:

```json
  "inx": {
    "enabled": true,
    "bindAddress": "localhost:9029",
    "pow": {
      "workerCount": 0
    }
  },
```

We now create the initial coordinator state by executing this command:

```bash
./hornet tools bootstrap-private-tangle --snapshotPath=shimmer/snapshots/genesis_snapshot.bin --cooStatePath=coordinator.state --configFile=config.json --databasePath=shimmer/database
```

We now have a coordinator.state file and an initial database in shimmer/database

## Setting up the coordinator

Go and clone the <a href=https://github.com/iotaledger/inx-coordinator > inx-coordinator </a>
```bash
git clone https://github.com/iotaledger/inx-coordinator.git
```

  Cd into it and build it

```bash
cd inx-coordinator && ./scripts/build.sh
```

Now you need to set the coordinator private keys as environment variable if you not have done that before, or changed the terminal.

```bash
export COO_PRV_KEYS=26211432de3c5a8839c2d21fe593ac6791298627cdf449278ec2fd11dc2a1756e28418dad96cd5645e9a683ab1a3c7af38c18e2e42e2c353c87326f0beb85ccd,ae1113879cff72edd7929069d2dc1047994b21551a1ecf6e77237ce698a809f19503b9aafe6f9b5c5ed0a3af3972e47ca477325f68091822cf0d0546f199187b,a17a0c8bf1980bd3f7dc71cde87afca799b9584cc66399bc63fbb0e9d014ddf8cd7c1a031b0110e480a17f52dc5e22555a8648c7bc6114b0374256f6e2333345,6e25598de2485bc5b5a72ab6d6f0c5f9e4270d9ef1e87f663616e383f4a5c8697da8a4319bec0a2bc7a7df9e4bdd48c3ec1ac373d694b789631fa48dcc7bbecc,0e9d0428f16d6cc6746e87fbba428b666fab0240f708b793af1f7b34dbb3b6212df4a3fdf8d6d02bad05f792f57d1bca8a3c7a8ebabc5948695087359f2b659a,a5ec89f65e48c4d0dcec325ab637359db38df0d4fe60741df47d8ec08d5be9226a792615891c3c3223052a4b48dde4324e6c38827e10cfe0a22f3a67bc598b6a,f5da2339b6c014dba4306f50ad88a3af44758abefa067d60032e602681017e59d11a223f38de24aa850a9be47252fec24e5f4f3c5be82dd7c17c898a58fab58d
```

Keep in mind, that you have to paste in your own private keys.

The last thing we need is the coordinator state file, which we created earlier. Go and copy it into the root of the inx-coordinator direcotry.

!!!We assume, that you build the inx-coordinator in the same parent directory as the hornet repo
```bash
cp ../hornet/coordinator.state ./
```

The coordinator is ready. 

## Running the tangle

Now you just need to run hornet with our confit:

```bash
./hornet -c config.json
```

and run the coordinator parallel:

```bash
./inx-coordinator
```
Do not forget to set the COO_PRV_KEYS!

In the terminal of hornet and of the inx-coordinator you'll see that the coordinator is issuing milestones.
Now, this system is running, but if you would look at the health status of the node, you'd see that the node is unhealthy.
The cause for that is, that the node has no neighbours and is the only one in the network. So if we want to have a usable network you need to have at least one neighbour and probably some inx-plugins, like the dashboard or the indexer.

## Setting up inx-dashboard

Go and clone the inx-dashboard repo:

```bash
git clone https://github.com/iotaledger/inx-dashboard.git
```

cd into it and build it.

```bash
cd inx-dashboard && ./scripts/build.sh
```

Now go to your hornet directory and generate a password hash and salt with hornet tools:

```bash
./hornet tools pwd-hash
```

If you typed "admin" as password (the default one IF uses in their docker setup) you'll get this hash and salt pair:

```
Your hash: fdd06554618d7ae72b60340de8616056a8fb3a3fcaaef3cabacaea3b940a6ebe
Your salt: f6d710f3a0ccd5e72ad67c9c4ce89bb31375af76d2aa9e845a6888a90a93a8df
```

We need now those two to give them to our inx-dashboard to be able to login. We do that by passing those to parameters to inx-dashboard:

```bash
./inx-dashboard --dashboard.auth.passwordHash 74c6527f6a1f6c62b164858ee0aac9f8518c6731fca3cb8b51be7c04216b590b --dashboard.auth.passwordSalt a7836ec8bebc66cf6f358b26e83635f8d7e96cf631339bd0eb263b1d23500127
```

Now inx-dashboard should be executed and your dashboard should be available at ```localhost:8081```.
With ```./inx-dashboard --help --full``` you'll get a full list of parameters.
On default, inx-dashboard tries to connect to localhost:9029, which is the default inx port from hornet.
It also starts the dashboard on localhost:8081, so if you want to change stuff like that, look at the full parameters list of inx-dashboard.

## Setting up neighbours

To have a health node we need at least one neighbour for our coordinator.
To make things easier we copy our first hornet installation:

```bash
cp -r hornet hornet-1
```

We go now into our hornet-1 directory and delete and change some stuff:

```bash
cd hornet-1
```

* Remove the old p2p identity, so we do not have duplicates: 
    ```bash
    rm -r shimmer/p2pstore
    ```
* Since we run the second hornet on the same machine, change the api, gossip and inx ports. **You do not need that, if you run the second instance on another machine!**
  1. Gossip (e.g. 15600 -> 15601)
  2. Api (e.g. 14265 -> 14266)
  3. Inx (e.g. 9029 -> 9030)
* Run hornet once to delete old database and generate a new p2p identity: 
    ```bash 
    ./hornet -c config.json --deleteDatabase
    ```

Now run hornet tools to extract your p2p identity. Alternative you can extract this information from the boot-up of your hornet node:
```bash
./hornet tools p2pidentity-extract
```

It should look something like this:

```
Your p2p private key (hex):    3eed18825cf8eaf2ce3ab85cf82d819d9c3a5f5f88397c73331fc3542c964635d9726cf6611b1ce5583f4d426a2b8b16f58e2a8605e9425381fd3de3730e4343
Your p2p public key (hex):     d9726cf6611b1ce5583f4d426a2b8b16f58e2a8605e9425381fd3de3730e4343
Your p2p public key (base58):  Fdpfobssd6ieQGfjXC2g5QhvV98H6coJetmMgPBRtGGA
Your p2p PeerID:               12D3KooWQTBqYjTaozz8pABA9nn1rCnkyUAv8JsncV8P4NLKnCz6
```

Here you need to remember the PeerID.

Now there are many ways to add neighbours:
* Access the dashboard, login in and add a neighbour by click "add peer" in the peers section
* Giving it via parameter at startup
* Adding the peer in the peering.json

By doing one of the first two, the peer will automatically be added to the peering.json

This is the syntax for giving it via parameters:

```bash
./hornet -c config.json --p2p.peers /ip4/127.0.0.1/tcp/p2p/15600/12D3KooWQTBqYjTaozz8pABA9nn1rCnkyUAv8JsncV8P4NLKnCz6
```

Peers are being seperated by commas. Please keep in mind, that you give the correct port and id. In my case, I connect the hornet-1 to the coordinator node.

In the peering.json it would look something like that:

```json
{
  "peers": [
    {
      "multiAddress": "/ip4/127.0.0.1/tcp/15600/p2p/12D3KooWQTBqYjTaozz8pABA9nn1rCnkyUAv8JsncV8P4NLKnCz6",
      "alias": ""
    }
  ]
}
```

Now you should see two healthy nodes in the dashboard.

## Setting up inx-indexer and other plugins

You probably want more plugins running on your nodes, so you can enjoy the whole functionality of the tangle.
One essential plugin is the indexer plugin. This documentation is analog to the most inx-plugins you'll find.

Go and clone your plugin:

```bash
got clone https://github.com/iotaledger/inx-indexer.git
```

Go into the repo and look for a folder called scripts:

```bash
cd inx-indexer && ls 
```

If you find a scripts folder execute the containing build.sh from the root:

```bash
./scripts/build.sh
```

If you can't find a scripts folder you should be able to just run

```bash
go build
```