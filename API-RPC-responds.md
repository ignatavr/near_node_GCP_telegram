
<POOl_ID> - name your pool without networkname - example `pool_of_bob`

<publick_key> - publick key your validator - example `ed25519:E5vaCBMGp7nnoyGZSf5BVe2zz4yEqRMsSGKXVeLPt4uc`

### Method 'validator'

##### Current proposals 

Current proposals all validators 

`CURL`

    curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq '.result.current_proposals[]'



![30](/screenshots/30.png)


Current proposals your validator, if you kmow pool id or part of pool id

`CURL`

    curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq '.result.current_proposals[] | select(.account_id | contains ("<POOl_ID> "))'

![31](/screenshots/31.png)


Current proposals your validator, if you kmow publick key or part of publick key 

`CURL`

    curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq '.result.current_proposals[] | select(.public_key | contains ("ed25519:E5vaCBMGp7nnoyGZSf5BVe2zz4yEqRMsSGKXVeLPt4uc"))'

Pesrson Kickout

`CURL`

    curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq '.result.prev_epoch_kickout[] | select(.account_id | contains ("<POOL_ID>") )'

##### Current validators

List all current validators

    curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq

`JQ`
    
    .result.current_validators[].account_id

you validator stake 

`JQ` 

.result.current_validators[] | select(.account_id | contains ("<POOL_ID>")) | .stake


```yml
  {
    "account_id": "boot2.near",
    "is_slashed": false,
    "num_expected_blocks": 25235,
    "num_expected_chunks": 41245,
    "num_produced_blocks": 25232,
    "num_produced_chunks": 41166,
    "public_key": "ed25519:ECGWXzUTXx4o2aqB6V3g3G9WMQdC741qPqtSUNb2Gvyq",
    "shards": [
      0
    ],
    "stake": "2000000000000000000000000000000"
  },
  {
    "account_id": "boot1.near",
    "is_slashed": false,
    "num_expected_blocks": 12425,
    "num_expected_chunks": 41245,
    "num_produced_blocks": 12424,
    "num_produced_chunks": 41184,
    "public_key": "ed25519:AHhWwLjDc9ohGmxACaFWUdBw1CSaubTyPU5YmnUD2DmP",
    "shards": [
      1
    ],
    "stake": "1000000000000000000000000000000"
  }
  ```

### Method 'status'


curl request 

    curl -s -d '{"jsonrpc": "2.0", "method": "status", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030

Your validator account id 
`JQ`
    .result.validator_account_id

result 
```yml
"spin.factory.shardnet.near"
```
all validators 

`JQ`

    .result.validators[] | .account_id



curl -s -d '{"jsonrpc": "2.0", "method": "block", "id": "dontcare", "params": {"block_id": 1006810}}' -H 'Content-Type: application/json' 127.0.0.1:3030 




    curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' https://rpc.shardnet.near.org | jq '.result.prev_epoch_kickout[] | select(.account_id | contains ("spin") )'
