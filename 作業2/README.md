# 如何讓Wazuh能夠攔截pfsense
***
## Step1: install pfsense and setup rsyslog<br>
[Agent]
```c
sudo apt install -y rsyslog
sudo systemctl enable rsyslog
sudo systemctl start rsyslog
```
設定 pfSense 日誌接收 
[Agent]
```c
>sudo nano /etc/rsyslog.d/pfsense.conf
```
並新增如下
```c
$ModLoad imudp
$UDPServerRun 514

$template PFSENSELOG,"/var/log/pfsense.log"
. ?PFSENSELOG
& stop
```
[Agent]
```c
sudo systemctl restart rsyslog				# 重啟 rsyslog
```
## Step2: Wazuh manager add pfSensor agent (ex: Ubuntu)
1. Click "Agents" icon --> "Deploy new agent"<br>
 Choose the operating system: Ubunut<br>
 Choose the version: Ubuntu 15+<br>
 Choose the architecture: x86__64<br>
 Wazuh server address: 10.18.106.146			# fill wazuh manager IP<br>
 Assign an agent name: Ubuntu-pfSensor<br>

2. Install and enroll the pfSensor agent
[Agent]
```c
cd ~/Downloads
```
 curl -so wazuh-agent.deb<br>
> --->[https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.5.0-1_amd64.deb](url) &&
> sudo WAZUH_MANAGER='10.18.106.146'<br>
> WAZUH_AGENT_NAME='Ubuntu-pfSensor' dpkg -i ./wazuh-agent.deb<br>

3. Open /var/ossec/etc/ossec.conf file and add a locafile for pfsense
  
 ```c
 nano /var/ossec/etc
```
**Add follow**
   ```c
   <localfile>
     <log_format>syslog</log_format>
     <location>/var/log/pfsense.log</location>
   </localfile>
```

4. Re-start the agent
 [Agent]
```c
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```
## Step3: Wazuh dashboard show the pfsense log message 






本作業示範 pfSense 日誌送到 Wazuh 並成功在 Dashboard 顯示事件。
資料夾包含 pfSense 與 Wazuh 截圖與設定檔，成果如「Wazuh攔截pfSense畫面.png」所示。
