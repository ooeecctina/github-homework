# MCP server 與 Claude desktop 安裝與執行
***
## MCP server 安裝與執行<br>
### 1.Download mcp server files
```c
git clone https://github.com/gbrigandi/mcp-server-wazuh.git
cd mcp-server-wazuh
sudo apt install cargo
```
### 2.Install Rust
```c
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
### 3.Build with stdio transport only (default)
#### openssl-sys 需要用 C 編譯工具鏈 來編譯 OpenSSL
```c
sudo apt update
sudo apt install -y build-essential pkg-config libssl-dev
source $HOME/.cargo/env
cargo build --release
```
### 4.Build with HTTP transport support
```c
cargo build --release --features http ''
```
***
## Claude desktop 安裝與執行<br>
### step1
```c
curl -fsSL https://claude.ai/install.sh | bash
```
### step2.download claude desktop deb package **"claude-desktop_1.0.3218_amd64.deb"** on websit<br>
  >--> <https://github.com/aaddrick/claude-desktop-debian/releases?utm_source=chatgpt.com><br>

**以下是要選擇的claude desktop deb file download link**
![claude desktop deb file download link](https://github.com/ooeecctina/github-homework/blob/main/%E4%BD%9C%E6%A5%AD1/claude%20desktop%20deb%20file%20download%20link.png)
```c
sudo dpkg -i claude-desktop_1.0.3218_amd64.deb
sudo apt --fix-broken install				# fix install fail issue
```
### step3.Modify claude_desktop_config.json
claude_desktop_connfig.json<br>
>--><https://github.com/ooeecctina/github-homework/blob/main/%E4%BD%9C%E6%A5%AD1/claude_desktop_config.json><br>
```c
gedit ~/.config/Claude/claude_desktop_config.json
```
```c
{
  "mcpServers": {
    "wazuh": {
      "command": "/home/tina888/Downloads/mcp-server-wazuh/mcp-server-wazuh/target/release/mcp-server-wazuh",		# your mcp-server-qazuh execute file
      "args": [],
      "env": {
        "WAZUH_API_HOST": "127.0.0.1",
        "WAZUH_API_PORT": "55000",
        "WAZUH_API_USERNAME": "admin",															# wazuh api username
        "WAZUH_API_PASSWORD": "SecretPassword",													# wazuh api password
        "WAZUH_INDEXER_HOST": "127.0.0.1",
        "WAZUH_INDEXER_PORT": "9200",
        "WAZUH_INDEXER_USERNAME": "admin",														# wazuh index username
        "WAZUH_INDEXER_PASSWORD": "SecretPassword",												# wazuh index username
        "WAZUH_VERIFY_SSL": "false",
        "WAZUH_PROTOCOL": "https",
        "WAZUH_ALERTS_FORMAT": "json",
        "WAZUH_ALERTS_LOG": "/var/ossec/logs/ossec.log",
        "WAZUH_ACTIVE_RESPONSE_LOG": "/var/ossec/logs/active-responses.log",
        "WAZUH_RULES_DIR": "/var/ossec/etc/rules",
        "WAZUH_DECODERS_DIR": "/var/ossec/etc/decoders",
        "WAZUH_MANAGER_ROLE": "standalone",
        "WAZUH_CLUSTER_ENABLED": "false",
        "RUST_LOG": "info"
      }
    }
  }
}
```
### step4.啟動 Claude Desktop
```c
claude-desktop
```
**以下是claude desktop 啟動成功的畫面**
![照片](https://github.com/ooeecctina/github-homework/blob/main/%E4%BD%9C%E6%A5%AD1/claude%20desktop%E5%95%9F%E5%8B%95%E6%88%90%E5%8A%9F%E6%88%AA%E5%9C%96.png)






本作業示範 MCP Server 與 Claude Desktop 的安裝與執行。
作業流程請依照各圖片檔名說明查看。
最終成果為「claude桌面啟動成功截圖.png」。
