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
> ---><https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.5.0-1_amd64.deb> &&
> sudo WAZUH_MANAGER='10.18.106.146'<br>
> WAZUH_AGENT_NAME='Ubuntu-pfSensor' dpkg -i ./wazuh-agent.deb<br>

3. Open /var/ossec/etc/ossec.conf file and add a locafile for pfsense<br>
  >---><https://github.com/ooeecctina/github-homework/blob/main/%E4%BD%9C%E6%A5%AD2/ossec.conf>
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
## Step3: Wazuh dashboard show the pfsense log message<br>
<br>
以下是pfsense log截圖的畫面

[![我的 Logo](https://github.com/ooeecctina/github-homework/blob/main/%E4%BD%9C%E6%A5%AD2/pfsense%20log%20%E6%88%AA%E5%9C%96%E7%95%AB%E9%9D%A2.png)](https://www.example.com)


以下是Wazuh 攔截pfsense log的畫面



[![我的 Logo](https://github.com/ooeecctina/github-homework/blob/main/%E4%BD%9C%E6%A5%AD2/Wazuh%E6%94%94%E6%88%AApfsense%E7%95%AB%E9%9D%A2.png)






本作業示範 pfSense 日誌送到 Wazuh 並成功在 Dashboard 顯示事件。
資料夾包含 pfSense 與 Wazuh 截圖與設定檔，成果如「Wazuh攔截pfSense畫面.png」所示。
