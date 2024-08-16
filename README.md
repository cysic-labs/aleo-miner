

# Welcome

# [Pool Website](https://aleopool.cysic.xyz/)

# QuickStart
##  Requirements
1. GPU: Nvidia Graphic cards and ram  2GB or above
2. RAM: 4G or above (8 cards)
3. CPU: 2 cores or above (8 cards)
4. Disk: For the prover 1GB (you need to remove the log file periodically if you turned the `log` on and your rigs are lack of disk space,the log file will be overwritten after you restarting the prover )
5. CUDA: Version-12.2 or above

## Ubuntu
### Requirements
1. System: Ubuntu-22.04 or above 

### Run the agent server
You need to set up  an individual machine in the LAN for running the agent server,and make sure that all the prover machines can connect to this machine.

#### 1.Download the latest agent binary from github releases
```
https://github.com/cysic-labs/aleo-miner/releases
```
`run command` :
```
wget https://github.com/cysic-labs/aleo-miner/releases/download/v0.1.15/cysic-prover-agent-v0.1.15.tgz

tar -xf cysic-prover-agent-v0.1.15.tgz
cd ./cysic-prover-agent-v0.1.15
ls
```
if everthing goes well,you will see the files bellow:
```
cysic-prover-agent  start.sh
```


#### 2.Run the agent server

Before you start the agent make sure the port is accessible,`run command`：
```
sudo ufw allow 9000/tcp
sudo ufw status | grep 9000
```

> *If you want to customize your agent host ip and port,modify them in start.sh
> 
> ```
> #!/bin/bash
> 
> AGENT_HOST=your_ip:your_port
> NOTIFY_HOST=notify.asia.aleopool.cysic.xyz:38883
> 
> ./cysic-prover-agent -l $AGENT_HOST -notify $NOTIFY_HOST > agent.log 2>&1 &
> ```

##### Start the agent server
`run command:`


```
bash start.sh 
```
run `telnet` to check the agent status:
```
timeout 1 telnet 0.0.0.0 9000
```

the agent server has been satred successfully if you see the output bellow:
```
Trying 0.0.0.0...
Connected to 0.0.0.0.
Escape character is '^]'.
```

Now let's head to the `prover`.

### Run the prover
1.Download the latest prover from github releases into your prover machine:
```
https://github.com/cysic-labs/aleo-miner/releases
```
`run command to download`:
```
wget https://github.com/cysic-labs/aleo-miner/releases/download/v0.1.15/cysic-aleo-prover-v0.1.15.tgz
tar -xf cysic-aleo-prover-v0.1.15.tgz 
cd ./cysic-aleo-prover-v0.1.15
ls
```
2.Modify the `AGENT_HOST`,`ADDRESS`,`WORKER_NAME` in `start.sh`

`AGENT_HOST`: the `agent`  ip and port 

`ADDRESS`: your `Aleo` wallet address for mining

`WORKER_NAME`: the customized name of your machine


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
##### Start the prover

`command:`
```
bash start.sh
```

you can check the log via command:
```
tail -f  ./prover.log
```


You can enter your address and check your machines on the pool website after you started your machines(It takes 1-3 minutes ),the official pool website is :

**[https://aleopool.cysic.xyz](https://aleopool.cysic.xyz/)**



