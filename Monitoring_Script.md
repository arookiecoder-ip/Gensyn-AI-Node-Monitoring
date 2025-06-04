<div align="center"> 

# Gensyn AI Node Monitoring Script </div>

### This script gives you:

#### 100% auto interaction with login prompts
#### 5-minute timer for login page
#### Alerts if user doesn’t complete login
#### Self-restarting on crash
#### Intimating Via telegram and Gmail on Crashing and Stopping Of Program

## 🚀 Setup Guide

## 🔐 1. Set Up Gmail App Password (for sending email via msmtp)
* ⚠️ App Passwords only work if 2-Step Verification is enabled on your Google account.

### ✅ Step-by-step:
1 . Go to: https://myaccount.google.com/ <br>
2.Click “`Security`” from the left sidebar <br>
3.Under “```Signing in to Google```”, enable:<br>
* 2-Step Verification (if not already)<br>

4 . Then Go To https://myaccount.google.com/apppasswords <br>

5 .Choose:
* App Name : ```Mail```

* Click ```Create ```

Google will give you a 16-character password like:

` abcd efgh ijkl mnop`

<br>

* Save The Password Somewhere else google wont show it to u again

## 🤖 2. Set Up Telegram Bot + Get Chat ID
### ✅ Step-by-step to Create a Telegram Bot:<br>
1 . Open Telegram<br>
2. Search For @Botfather <br> https://t.me/BotFather
<br>
3 . Send <br>

```
/start 
/newbot 
```
4. BotFather will ask:
* Name: `Gensyn Monitor Bot`

It Will Give U a Bot Token :
` 0123456789:BAEabIDublzOy-2MMDSK8Nw14obBOMjDNEU ` (This is dummy bot token , dont use it)

Click on the new Created bot and press `start`

5. Get Your Chat Id: 
@userinfobot <br> https://t.me/userinfobot
Press `/start ` and u will get the chat id


## 👾Terminal/GCP Setup



### ✅ 1. Install dependencies
```
sudo apt update
sudo apt install expect msmtp curl
```

### ✅ 2. Set up msmtp (for email)

Create or edit ~/.msmtprc:

```
nano .msmtprc
```
Paste The below Code and then save the file by `CTRL+X ` ,press `y` then hit `enter`
```
defaults
auth on
tls on
tls_trust_file /etc/ssl/certs/ca-certificates.crt
logfile ~/.msmtp.log

account gmail
host smtp.gmail.com
port 587
from your_email@gmail.com
user your_email@gmail.com
password YOUR_APP_PASSWORD

account default : gmail
```
Replace `your_email@gmail.com` with your gmail<br>
Replace `YOUR_APP_PASSWORD` with your app password generated from google(Remove the spaces between the password)<br>

``` 
chmod 600 ~/.msmtprc 
```
Set the Permission

### ✅ 2. Setup .sh File

Navigate To  `rl-swarm ` directory by : 

```
cd rl-swarm 
```
Create a file named:
` run-and-alert.sh` by :
```
nano run-and-alert.sh
```
And paste the below code 

```
#!/bin/bash

# === CONFIGURATION ===
TELEGRAM_BOT_TOKEN="YOUR_BOT_TOKEN"             # Replace with your bot token
TELEGRAM_CHAT_ID="YOUR_CHAT_ID"                 # Replace with your chat ID
EMAIL_TO="your_email@email.com"                        # Replace with your email address
PROGRAM_COMMAND="/rl-swarm/run_rl_swarm.sh"     # Replace with your program/script

# === ALERT FUNCTIONS ===
send_crash_alert() {
  echo "[!] Program crashed or stopped. Sending crash alerts..."

  curl -s -X POST "https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage" \
    -d chat_id="$TELEGRAM_CHAT_ID" \
    -d text="💥 Program crashed on $(hostname) at $(date). Restarting..."

  echo -e "Subject: Program Crash Alert 💥\n\nProgram crashed on $(hostname) at $(date). Restarting..." \
    | msmtp "$EMAIL_TO"
}

send_manual_stop_alert() {
  trap - EXIT  # Prevent double alerts
  echo "[!] Program stopped manually (Ctrl+C). Sending stop alerts..."

  curl -s -X POST "https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage" \
    -d chat_id="$TELEGRAM_CHAT_ID" \
    -d text="🛑 Program manually stopped on $(hostname) at $(date). No auto-restart."

  echo -e "Subject: Program Manual Stop Alert 🛑\n\nProgram manually stopped on $(hostname) at $(date)." \
    | msmtp "$EMAIL_TO"

  exit 0
}

# === TRAPS ===
trap send_manual_stop_alert SIGINT   # Ctrl+C
trap send_crash_alert EXIT           # Any other exit

# === MAIN LOOP TO AUTO-RESTART ON CRASH ===
while true; do
  echo "🚀 Starting program at $(date)"
  bash "$PROGRAM_COMMAND"
  echo "❌ Program exited. Restarting in 5 seconds..."
  sleep 5
done
```
Replace `YOUR_BOT_TOKEN` with your telegram bot token<br>
Replace `YOUR_CHAT_ID` with your telegram chat id <br>
Set The Permission 
```
chmod +x run_and_alert.sh
```
#### Set The Parameters(The code will automatically run the program until the localhost page, so it is neccesary to setup before as per your pc requirement)

Need to change some value in the main file `run_rl_swarm.sh`

1. Make Sure you are in the `rl-swarm` directory
2. Go to the file `run_rl_swarm.sh` by 
```
nano run_rl_swarm.sh
```
Now Go to line no :126<br> 
You will find something like this :
```
pc=${pc:-0.5}  # Default to "0.5" if the user presses Enter
```
modify the value of `0.5` to whatever parameter u wish to keep .<br>
Then Save the file

Create And Activate Virtual Enviornment :
```
python3 -m venv .venv
source .venv/bin/activate
```
### Now Run The File by
```
run_and_alert.sh
```


## If You Face Any Error , Feel Free To ask 

