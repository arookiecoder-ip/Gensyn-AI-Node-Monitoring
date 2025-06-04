<div align="center"> 

# Guide </div>
> [!IMPORTANT]
>  Node Supervisor with Auto-Restart and Crash Alerts
> Automatically restart and monitor any shell script or program with Telegram and email alerts

First Go To The ` GensynAi` Node Run Setup [GensynAI Guide](https://github.com/Mayankgg01/Gensyn-ai-Rl-Swarm_Guide)

> [!NOTE]
> Follow The Next Steps Only After Setting Up the node .

> [!CAUTION]
> Dont Run The Node Before Completing The Next Steps.

Go to [Monitoring_Script](https://github.com/arookiecoder-ip/Gensyn-AI-Node-Monitoring/blob/02708e522f984fe68582669d79ced05f4764fc05/Monitoring_Script.md)
and Follow Step By Step
<br>


### If U Want the values of the inputs in the program to be prefilled in `Gensyn AI` , Then Follow the Step: 

```
curl -L https://raw.githubusercontent.com/arookiecoder-ip/Gensyn-AI-Node-Monitoring/main/run_rl_swarm.sh -o ~/rl-swarm/run_rl_swarm.sh

```


#### If u Want To Modify The Parameters(0.5,1.5,7 etc)
Then type:
```
nano ~/rl-swarm/run_rl_swarm.sh
```
Find The Line `PARAM_B=1.5` or Go To Line No `102` if running in Any Code Editor Like VSCode etc

* Change The Value of `1.5` to whatever u wish to change the parameter to.

Make it executable by 
```
chmod +x ~/rl-swarm/run_rl_swarm.sh
```

> Now Go Back to the [Monitoring_Script](https://github.com/arookiecoder-ip/Gensyn-AI-Node-Monitoring/blob/02708e522f984fe68582669d79ced05f4764fc05/Monitoring_Script.md)
and Follow Step 