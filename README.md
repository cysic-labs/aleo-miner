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

> if everything goes well, you will see the files bellow:
> 
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
wget https://github.com/cysic-labs/aleo-miner/releases/download/v0.1.18/cysic-aleo-prover-v0.1.18.tgz
tar -xf cysic-aleo-prover-v0.1.18.tgz 
cd ./cysic-aleo-prover-v0.1.18
ls
```


2. **Install the prover**

Run the following command to install :

Before running the command,please change the params to yours:
 `--address`: your aleo address
 `--name`: the  customized name of your machine
 `--agent`: your agent server_ip and port

```
./install.sh --pool tls://asia.aleopool.cysic.xyz:16699 --address aleo18xe6qxxxxh --name machine_name_1  --agent your_agent_ip:port

timeout 1 systemctl status cysic-aleo-prover
```
> you will see the output bellow:
> ```
> ● cysic-aleo-prover.service - cysic-aleo-prover
>      Loaded: loaded (/etc/systemd/system/cysic-aleo-prover.service; enabled; vendor preset: enabled)
>      Active: active (running) since Wed 2024-08-21 06:18:32 UTC; 45s ago
>    Main PID: 2807808 (start.sh)
>       Tasks: 21 (limit: 629145)
>      Memory: 133.5M
>         CPU: 1min 19.964s
>      CGroup: /system.slice/cysic-aleo-prover.service
>              ├─2807808 /bin/bash /opt/cysic/prover/start.sh
>              └─2807809 ./cysic-aleo-prover -l /opt/cysic/prover/prover.log -a 0.0.0.0:9000 -w aleo18xe6
> ```
You can also check the prover log via command:

```
tail -f  /opt/cysic/prover/prover.log
```
> *Attention: 
> The **prover will start automaticlly** after you run the install command,if you want to modify the parameters again, you can the **./install.sh** command with new parameters.If you want to **stop** or **start** the prover,you can check the commands in the following chapters.

3.**Stop the prover**

Run command to **stop** the running prover:
```
systemctl stop  cysic-aleo-prover
```

4.**Start the prover**

Run command to **start** the prover,if your prover is not running:
```
systemctl start  cysic-aleo-prover
```
5.**Restart the prover** 

```
systemctl restart  cysic-aleo-prover
```
You can enter your address and check your machines on the pool website after you started your machines(It takes 1-3 minutes ),the official pool website is :

**[https://aleopool.cysic.xyz](https://aleopool.cysic.xyz/)**
