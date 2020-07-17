<img src="https://raw.githubusercontent.com/HandyMiner/HandyGuide/72303a89968942dc945e05588db5db2a6610c539/logo/cobra.svg" width="150" height="150" />

HANDYMINER
2020 Alex Smith <alex.smith@earthlab.tech>

### [🐍HandyMiner Quick Start Guide🐍](https://handyminer.github.io/HandyGuide/)

**HandyMiner Team Donation Address (HNS): hs1qwfpd5ukdwdew7tn7vdgtk0luglgckp3klj44f8**
**HandyMiner Team Donation Address (BTC): bc1qk3rk4kgek0hzpgs8qj4yej9j5fs7kcnjk7kuvt**

**Quick Links**
- [Download Latest from Releases](https://github.com/HandyMiner/HandyMiner-Goldshell-CLI/releases)

- [Prerequisites and Installation](#buildInstructions)

- [Fullnode Solo-Mining Setup](#runFullnode)

- [Pool Settings](#poolParameters)

- [FAQ](#faq)

**HandyMiner Social Channels:**

[HandshakeTalk Telegram](https://t.me/handshaketalk); [Handshake Mining/HNS Discord](https://discord.gg/VMUneym)




A simple CLI interface for HSD Mining with the Goldshell HS1
to communicate with Handshake HSD via Stratum Mining. 
# Easy Installation
       Easily installed within minutes.
       Simple ASIC Configuration setup.

### HandyMiner Running with Dashboard Enabled

![Imgur](https://i.imgur.com/0Y3Q5UZ.jpg)

<a id="buildInstructions"></a>
## PREREQUISITES

[Node.js](https://nodejs.org) v10.4 - v11+-ish (whatever one has bigint support)

(**Windows Users**) [Git Bash](https://git-scm.com/downloads) A handy bash terminal, **install in Program Files/Git**

(**Windows Users**) [Download STM32 Virtual COM PORT driver from Goldshell](https://github.com/goldshellminer/HS1/tree/master/miner/serial_driver) OR [Download STM32 Virtual COM PORT driver from STMicroelectronics](https://www.st.com/en/development-tools/stsw-stm32102.html)

(optional) [Docker](#dockerReminders) if you want to run your own fullnode to mine to with the provided utilities

Linux: Dependencies install can be found in [./linux_installation.md](./linux_installation.md)


## INSTALLATION

#### Download & Install the Prebuilt ZIP :

[Download Latest from Releases](https://github.com/HandyMiner/HandyMiner-Goldshell-CLI/releases)

**Note: un-zipping the full contents may take a bit.**

#### OR BUILD YOURSELF 

#### Build on the Command Line (mac/linux/windows) :

```npm install``` in this directory or 

#### Windows (non-prebuilt only install) : 

double-click ```install.windows.bat``` (and make sure to run this as administrator). Windows: This will probably take 10 minutes to build.

Windows folks: If you didnt double click ```install.windows.bat``` youll need to run the following commands in the repo root: 
```npm install```

<a id="running"></a>
## THE POINT

First, make sure to plug AC power into the Goldshell HS1 and connect the USB from the HS1 to your computer. 

Non-terminal users: simply double click ```(windows) dashboard.windows.bat``` or ```(mac) dashboard.mac.command``` or ```(linux) dashboard.sh``` files.

**Note:** The first time you launch will run you thru the [miner configurator](#minerConfigurator).

Launch the miner in terminal **in a bash terminal (windows: git-bash)** like:
```
cd (into the repo base)
npm start (runs the CLI miner)

Most terminals:
npm run dashboard //runs the CLI dashboard+miner
OR
./dashboard.sh //same

Note: many windows terminals dont do text coloring or dashboards right with npm run commands.. So if it's an issue (and you didnt double click) you can launch the proper dashboard like:

node --max-old-space-size=8196 ./miner/dashboard.js
```

[Mac FAQs](#macFAQ)

#### Mine blocks!

(Ctrl+C, Q, or ESC to stop the dashboard miner)

<a id="minerConfigurator"></a>
## MINER CONFIGURATOR

The first time you run the miner, you will run through a configurator which will write a config to ./goldshell.json. Should you want to reconfigure in the future, you can just run ```node configure.js``` in this directory. Alternately delete goldshell.json and (re)start the miner.

Required items to have ready for configuration:

0. Host or IP address of the pool or solo node you mine to (127.0.0.1 for your local fullnode)

Optional configuration items (just leave blank and hit enter if you dont know) :
1. Pool or solo node port (probably 3008)
2. The stratum password (optional)

<a id="poolParameters"></a>
## POOL SETTINGS

#### F2POOL

**stratum host**: hns.f2pool.com
**stratum_port**: 6000

non-registered: 
**username**: wallet.workerName
**password**: anything

registered:
**username**: username.workerName
**password**: anything

#### 6block

**stratum host**: handshake.6block.com
**stratum_port**: 7701

**username**: username.workerName
**password**: anything

#### PoolFlare

**stratum host**: hns-us.ss.poolflare.com
**stratum_port**: 3355

**username**: wallet.workerName
**password**: anything

#### hnspool

**stratum host**: stratum-us.hnspool.com
**stratum_port**: 3001

**username**: hnspool_registered_username
**password**: hnspool_registered_password

<a id="advancedOptions"></a>
## Advanced Options:

**App/API developers**: You can run like ```HANDYRAW=true node mine.js``` or set the environment variable ```HANDYRAW=true``` and the miner executable will output raw JSON. The dashboard application is built using the raw CLI JSON output.

**Multiple Configs!**: If you want to use different worker names or pools with the same miner executable, you can pass in your custom config.json into the miner like:
```node mine.js myCustomConfigName.json```
Note the miner will default to config.json without this argument.

**ASIC per Config**: If you are using multiple configs, you may want to group multiple Goldshell HS1's as you see fit. Within ```goldshell.json``` the line ```asics:"-1"``` can change to: ```asics:"COM1,COM2"```. basically the COM* values are the ports displayed initially when you start the miner with ```node mine.js```

<a id="hangryInfo"></a>
**Hangry Mode Update 3/6/2020**::
We released Hangry Mode for Vega56/64 and AMD 5700s. Hangry Mode is enabled by default for Vega 56/64, but can only be enabled for the AMD 5700 as an option (for an extra 25MH bump). Hangry Mode is more-or-less stable for 5700 but can use more power. If you are close to capacity on your PSU it will probably fall over. To enable Hangry Mode for AMD5700:: 
1. You can remove your config.json and run ```node configure.js``` to auto-generate your config.json file with hangry mode enabled, or 
2. add the following line to the end of your existing config.json like::
```
...
"poolDifficulty": -1,
"muteWinningFanfare": false,
"enableHangryMode": true
```

<a id="faq"></a>
## Mining FAQ:

1. I started the dashboard and it says connection to 127.0.0.1 is timed out and trying again in 20s. 
This means your fullnode is not running. Please [launch a fullnode](#runFullnode)  or mine to a pool IP address.

2. I dont see all my GPUs listed in the configurator
Use your arrow keys, the list is scrollable. Hit space to multi-select and Enter to goto the next step. Also ensure that you're updated to the latest version AMD/Nvidia drivers, as older versions may not be recognized.

3. We do not auto-start the fullnode for you here like we do in HandyMiner-GUI. However we made it easy here and its a [double click to start it](#runFullnode). 

4. Windows may also need to add the following two items added to the ```Path``` environment variable:
```
C:\Program Files\nodejs\node_modules\npm\bin
```
<a id="macFAQ"></a>

#### MAC FAQ:

![img](https://i.imgur.com/teYpH3Y.png)

If you have a user-permission error, you'll need to run these commands to get the dashboard running on MacOS Mojave/Catalina. CD into your HandyMiner directory (the folder you downloaded), then run:

```
chmod 755 dashboard.mac.command

Double click dashboard.mac.command
```
To run the miner CLI without the dashboard, you can simply run this command from your HandyMiner folder:
```
sudo node mine.js
```

<a id="runFullnode"></a>
### Running an HSD Fullnode for solo mining

There are some handy docker utilities in the folder ```./fullnode_utils``` which you can double click on mac/windows/linux to create, launch, stop or nuke a stratum-enabled fullnode on your local machine that you can mine to. Any utilities specific to the network (main|testnet|simnet) are noted in the command names. 

<a id="dockerReminders"></a>
#### Docker Fullnode Reminders:
1. Make sure you installed docker. PSA: you dont need to login to docker either, dont let them fool you [mac, hunt for the dmg link](https://docs.docker.com/docker-for-mac/release-notes/) [windows, hunt for the exe link](https://docs.docker.com/docker-for-windows/release-notes/) ). Even after you install it: you dont need to login in order to run this goodness.
2. Make sure that you added your wallet address you'd like paid at to ```run.mac.command OR run.windows.bat OR run.sh (for production)``` or ```run.powng.mac.command OR run.powng.windows.bat OR run.powng.sh (for simnet)``` in the end of the command that looks like: ```"./run.sh hs1qwfpd5ukdwdew7tn7vdgtk0luglgckp3klj44f8"```

#### Docker Fullnode FAQs:

1. I get an error:: ``` Error response from daemon: driver failed programming external connectivity on endpoint earthlabHSD (...) Error starting userland proxy: mkdir /port/tcp:0.0.0.0:14038:tcp:172.17.0.2:14038: input/output error Error: failed to start containers: earthlabHSD ``` 
Right click the docker icon in your taskbar and 'restart docker'. It might take a couple minutes to fully restart.

2. Can I access the hsd docker fullnode? 
Yes. Port 15937 will be open for RPC calls to the node per usual, else ssh into the container's hsd directory with::
```docker exec -it earthlabHSD bash```
Windows:
```winpty docker exec -it earthlabHSD bash```

3. I'd like to import my wallet into this fullnode, How do I set my fullnode's API password and/or stratum password to something more meaningful? 

Edit the file you run the fullnode with, ```run.mac.command``` or ```run.windows.bat``` for example, and add the 3rd and 4th parameters into run.sh like:::
```docker start earthlabHSD && docker exec -i earthlabHSD sh -c "./run.sh hs1qwfpd5ukdwdew7tn7vdgtk0luglgckp3klj44f8 main my_hsd_api_password_here my_stratum_password_here"``` 
and pay attention that the passwords are inside that double quote ^^

4. If you don't want to run a Dockerized fullnode, you can feel free to check the docs in ```fullnode_native_readme.md``` to see how to launch your own native hsd fullnode.


```
       _.-._        _.-._
     _| | | |      | | | |_
    | | | | |      | | | | |
    | | | | |      | | | | |
    | _.-'  | _  _ |  '-._ |
    ;_.-'.-'/`/  \`\`-.'-._;
    |   '    /    \    '   |
     \  '.  /      \  .`  /
      |    |        |    |

```

EPIC Thanks to chjj and the entire Handshake Project

EPIC Thanks to Steven McKie for being my mentor/believing in me
