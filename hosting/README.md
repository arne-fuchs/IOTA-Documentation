# Hosting
Documentation about hosting.

## Hornet

### Ports

| Port        | Protocol | Description                                                         | Requiered |
|-------------|----------|---------------------------------------------------------------------|-----------|
| ```15600``` | TCP      | Gossip protocol port                                                | Yes       |
| ```14265``` | TCP      | REST HTTP API port                                                  | No        |
| ```14626``` | UDP      | Autopeering port                                                    | No        |
| ```8081```  | TCP      | <a href=https://github.com/iotaledger/inx-dashboard>Dashboard</a>   | No        |
| ```8091```  | TCP      | <a href=https://github.com/iotaledger/inx-faucet>Faucet website</a> | No        |
| ```1883```  | TCP      | <a href=https://github.com/iotaledger/inx-mqtt>MQTT</a>             | No        |

Please keep in mind, that some ports are inx-plugins and might need some additional configuration.