# Tutorial

## [Pool website](https://aleopool.cysic.xyz/)



## Hardware Requirements

1. GPU: Nvidia Graphic cards and RAM 2GB or above.
2. RAM: 4GB or above (8 cards).
3. CPU: 2 cores or above (8 cards).
4. Disk: 1GB or above (you need to remove the log files periodically if you turned the `log` on. Otherwise your rigs will be lack of disk space. The log file will be overwritten after you restarting the prover)
5. CUDA: Version-12.2 or above.



## Ubuntu

### Supported Distributions

Ubuntu 22.04 or above

### Run the agent server

You need to set up an individual machine in the LAN for running the agent server, and make sure that all the prover machines can connect to agent server.

1. **Download the latest agent release from GitHub**

```
https://github.com/cysic-labs/aleo-miner/releases
```

Run the following command

```
wget https://github.com/cysic-labs/aleo-miner/releases/download/v0.1.15/cysic-prover-agent-v0.1.15.tgz

tar -xf cysic-prover-agent-v0.1.15.tgz
cd ./cysic-prover-agent-v0.1.15
ls
```

if everything goes well, you will see the files bellow:

```
cysic-prover-agent  start.sh
```

2. **Run the agent server**

Before you start the agent, please make sure the port is accessible using the following command:

```
sudo ufw allow 9000/tcp
sudo ufw status | grep 9000
```

If you want to customize your agent host ip and port, modify it in `start.sh`:

```
#!/bin/bash

AGENT_HOST=your_ip:your_port
NOTIFY_HOST=notify.asia.aleopool.cysic.xyz:38883

./cysic-prover-agent -l $AGENT_HOST -notify $NOTIFY_HOST > agent.log 2>&1 &
```

3. **Start the agent server**

Now let's start the agent server

```
bash start.sh 
```

then use `telnet` to check the agent status:

```
timeout 1 telnet 0.0.0.0 9000
```

The agent has been started successfully if you see the output below:

```
Trying 0.0.0.0...
Connected to 0.0.0.0.
Escape character is '^]'.
```

The agent server part is done. Now let's move on the  prover.



### Run the prover

1. **Download the latest prover release from GitHub**

```
https://github.com/cysic-labs/aleo-miner/releases
```

Run the following command

```
wget https://github.com/cysic-labs/aleo-miner/releases/download/v0.1.16/cysic-aleo-prover-v0.1.16.tgz
tar -xf cysic-aleo-prover-v0.1.16.tgz 
cd ./cysic-aleo-prover-v0.1.16
ls
```

2. **Modify the `AGENT_HOST`,`ADDRESS`,`WORKER_NAME` in start.sh**&#x20;

`AGENT_HOST:` the `agent_ip` and `port`

`ADDRESS:`Your Aleo mining address

`WORKER_NAME:`The customized name of your machine

```
AGENT_HOST
#!/bin/bash
POOL_HOST=asia.aleopool.cysic.xyz:16699
AGENT_HOST=172.16.100.1:9000
ADDRESS=aleo13hfl04x923c6k6n6uytc5tdav5xfcym8j3m04mv7c94k7krktyrswvsnvh
WORKER_NAME=machinex172x16x100x8

ADDRESS_WORKER=$ADDRESS.$WORKER_NAME
export LD_LIBRARY_PATH=./:$LD_LIBRARY_PATH
./cysic-aleo-prover -tls=true -p $POOL_HOST -a $AGENT_HOST -w $ADDRESS_WORKER > prover.log 2>&1 &
```

3. **Start the prover**

Run the following command:

```
bash start.sh
```

You can also check the log via command:

```
tail -f  ./prover.log
```

You can enter your address and check your machines on the pool website after you started your machines(It takes 1-3 minutes ),the official pool website is :

**[https://aleopool.cysic.xyz](https://aleopool.cysic.xyz/)**
