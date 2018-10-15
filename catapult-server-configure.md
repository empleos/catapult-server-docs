# Configure and run NEM 2 Catapult server

Go to _build directory

## Copy configuration files
```
cp ../resources/* resources/
```

## Generate 10 NEM addresses
```
bin/catapult.tools.address -g 10 --network mijin-test > addresses.txt
```

## Edit mijin-test.properties
```
cp ../tools/nemgen/resources/mijin-test.properties resources/
vi resources/mijin-test.properties

# Edit cppFile line.
# Replace existing address in the first line in [distribution>nem:xem] subsection with the first address in addresses.txt.

cppFile = ../seed/mijin-test/MockMemoryBasedStorage_data.h

[distribution>nem:xem]
SD564SIEFGMFSYMBAY3ARDSXYLMYI7T44VI5APUW = 409'090'909'000'000
```

## Configure harvesting
```
vi resources/config-harvesting.properties

# harvestKey - one of the private keys generated in addresses.txt
harvestKey = 093FE36D2DF378771AED437006CD0618E2EEF7B44E3646F7B9AC0BEA624BC414
isAutoHarvestingEnabled = true
```

## Configure server account
```
# Edit resources/config-user.properties
vi resources/config-user.properties

# Replace bootKey with the first private key in addresses.txt
bootKey = 093FE36D2DF378771AED437006CD0618E2EEF7B44E3646F7B9AC0BEA624BC414
```

## Create hashes.dat
```
mkdir -p seed/mijin-test/00000
echo -n 6b23c0d5f35d1b11f9b683f0b0a617355deb11277d91ae091d399c655b87940d3f39d5c348e5b79d06e842c114e6cc571583bbf44e4b0ebfda1a01ec05745d43 > seed/mijin-test/00000/hashes.dat
```

## Create Nemesis Block
```
cd bin
./catapult.tools.nemgen --nemesisProperties ../resources/mijin-test.properties
cd ..
```

## Copy data
```
mkdir -p data/00000
cp -r seed/mijin-test/* data/
```

## Data for API nodes (we don't have any API nodes yet)
```
# Edit resources/peers-api.json
vi resources/peers-api.json

# leave the array empty
{ 
  "_info": "this file contains a list of api peers",
  "knownPeers": [
  ]
}
```

## Data for the current Pear node
```
# Edit resources/peers-p2p.json
vi resources/peers-p2p.json

# Specify the first public key from addresses.txt
"publicKey": "F5B92FD7EF1B51D62073E0F1F844CFA3B4C9F82D27FFAE0E0496C0AFD40DFF90",

# Specify host's public IP
"host": "xxx.xxx.xxx.xxx",

# Specify node's name, for example:
"name": "node1",
```

## Run Catapult server
```
cd bin
./catapult.server
```
