# Setup a running validator node for shardnet on GCP with telegram alert

### PART I - create resources

This instruction decribdes a process setuption of NEAR Validator node on GCP

first of all you need: 
- Account on Google Cloud Platform https://cloud.google.com/ - *you might create new account with free billing periods*
- install gcloud on your work machine - choose ypur OS platform on this page https://cloud.google.com/sdk/docs/install and install gcloud 
- and Telegram account 


You should create a private VPS network, with firewall rules for ssh and validator connect 
And create instansce n2-standart-4 with 500 Gb SSD and connect your new priate network and add firewall rules

This instance coast about 210$ per month 

#### If you use Google Cloud Console a first time, pleace follow this my [instruction](GCP-setting.md)

### PART II - Setup node 

Connect through ssh on your instance 

Update and Upgrade system: 

    sudo apt update && sudo apt upgrade -y

Install developer tools:

    sudo apt install -y git binutils-dev libcurl4-openssl-dev zlib1g-dev libdw-dev libiberty-dev cmake gcc g++ python docker.io protobuf-compiler libssl-dev pkg-config clang llvm cargo -y

Install python3 
    sudo apt install python3-pip

And set configuration 

    USER_BASE_BIN=$(python3 -m site --user-base)/bin
    export PATH="$USER_BASE_BIN:$PATH"

Install Building env

    sudo apt install clang build-essential make

Next, install Rust and Cargo 

    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
    source $HOME/.cargo/env

Next you must install `Node.js` and `NPM` package manager
    
    curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -  
    sudo apt install build-essential nodejs -y
    PATH="$PATH"

Check versions 

    node -v
    npm -v

You must have Node's version 18.6.0 and NPM's version 8.13.2 or above

Our next step it is install Near-CLI

    sudo npm install -g near-cli

I recommend to you read [Official Near Cli documentation](https://docs.near.org/docs/tools/near-cli) it is very important instrument

if you did evrithing right, you show this 
![11](/screenshots/11.png)

#### Connect to shardnet 

Connect to shardnet 

    export NEAR_ENV=shardnet

I recommend run this command to set the Near testnet Environment persistent

    echo 'export NEAR_ENV=shardnet' >> ~/.bashrc

Check this 
    near proposals

#### Install nearcore 

Clone the nearcore repository 

    git clone https://github.com/near/nearcore
    cd nearcore
    git fetch

Checkout to the branch needed. Please check the releases page on GitHub.

    git checkout <version>

Compile nearcore 

    cargo build -p neard --release --features shardnet

#### Initialize working directory

Generate the initial required working directory by running:

    ./target/release/neard --home ~/.near init --chain-id shardnet --download-genesis

Replace the config.json

    rm ~/.near/config.json
    wget -O ~/.near/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json

#### Get latest snapshot

Install AWS Cli

    sudo apt-get install awscli -y

Download last snapshot

    cd ~/.near
    aws s3 --no-sign-request cp s3://build.openshards.io/stakewars/shardnet_noarchive/data.tar.gz .  
    tar -xzvf data.tar.gz

If the above fails, AWS CLI may be oudated in your distribution repository. Instead, try:

    pip3 install awscli --upgrade

#### Run the node

To start your node simply run the following command:

    cd nearcore
    ./target/release/neard --home ~/.near run

### PART III Setup node as validator

First of all you must create a wallet on [shardnet](https://wallet.shardnet.near.org)  

you must remember you seed phrases (consist 12 words)

Also you get about 2000 tokens *this token not real*

#### Connect your wallet on near-cli 

    near login

Open link on your browser and grant Access to Near CLI

![12](/screenshots/12.png)
![13](/screenshots/13.png)

#### Check the validator_key.json

     cat ~/.near/validator_key.json


Note: If a validator_key.json is not present, follow these steps to create one

#### Create a validator_key.json

* Generate the Key file:


