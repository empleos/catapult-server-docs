# Install Catapult REST API

Login as catapult user

## Install Node.js 8
```
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y nodejs
```

## Install yarn
```
curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update
sudo apt-get install yarn
```

## Install catapult REST
```
git clone https://github.com/nemtech/catapult-rest.git
cd catapult-rest
./yarn_setup.sh
cd rest
yarn build
```

## Edit rest.json
```
vi resources/rest.json

# Replace clientPrivateKey and publicKey with keys from addresses.txt generated during catapult server configuration

"clientPrivateKey": "DD67B12B3BF019390B8589428F8F1365080AC5B7F3E3A25B8840B3A1DFED9AF2",

  "apiNode": {
    "host": "127.0.0.1",
    "port": 7900,
    "publicKey": "0032A2CE81728DDFB0E7874AF8B98EC040418195721C1AEE2DD82776CCEC4551"
  },
```

## Run REST server
```
cd _build
node index.js
```

## Test GET query
```
curl http://localhost:3000/block/1
```
