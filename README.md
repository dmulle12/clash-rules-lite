<p align="center">
  <img width="100px" src="https://user-images.githubusercontent.com/35565811/214613019-6fd702b7-445e-4663-8471-f47005241724.png" align="center" alt="GitHub Readme Stats" />
  <h2 align="center">Clash Rules Lite</h2>
 
  <p align="center">🍒Customize proxy rules and streamline matching rules 。</p>
 
  <p align="center">
    <a href="https://github.com/zhanyeye/clash-rules-lite/blob/master/.github/workflows/release.yml">
    <img src="https://github.com/zhanyeye/clash-rules-lite/actions/workflows/release.yml/badge.svg" />
    </a>
  </p>
 
  <p align="center">
    <a href="https://github.com/zhanyeye/clash-rules-lite/blob/main/proxy-rules.txt">Proxy rules list</a> |
    <a href="https://github.com/zhanyeye/clash-rules-lite/blob/main/microsoft-rules.txt">Microsoft service rules list</a> |
    <a href="https://github.com/zhanyeye/clash-rules-lite/blob/main/blacklist-rules.txt">Blacklist rules list</a>
  </p>

</p>

<p>
  <pre align="center">
  https://cdn.jsdelivr.net/gh/zhanyeye/clash-rules-lite@release/proxy-rules.txt    
  https://cdn.jsdelivr.net/gh/zhanyeye/clash-rules-lite@release/microsoft-rules.txt
  https://cdn.jsdelivr.net/gh/zhanyeye/clash-rules-lite@release/blacklist-rules.txt</pre>
</p>
<p align="center">
Proxy service recommendation used stably for 2 years: https://inet.ssp.lol/#/register?code=p5BXMYcJ
</p>

### Tool introduction
  Clash’s default GFW proxy rules contain too much content, and there is obvious delay during use.
  The idea of this tool is to add proxy rules as you use them. After all, the websites we visit should be very limited.
  The purpose of this tool is to delete unnecessary proxy rules and facilitate users to customize the content of the proxy
  The proxy rules are placed in the github repository to facilitate multi-device synchronization. Just edit [rules.txt](https://github.com/zhanyeye/clash-rules-lite/blob/main/rules.txt)
  When users update rules, use Github Actions to automatically cache the rules to a free CDN
  After the user updates the rules on github, click refresh on the providers of clash to pull the updates


### How to customize
1. Fork this repository: [Fork zhanyeye/clash-rules-lite](https://github.com/zhanyeye/clash-rules-lite/fork) 
2. Trigger the `Generate Rules for Clash` workflow in GitHub Actions
3. Edit `xx-rules.txt` to customize rules
4. Refresh the configuration file in Clash

<div align="center">
  <center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://user-images.githubusercontent.com/35565811/184524456-e956ef59-4577-44e9-9b99-4a8684b77e40.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">Pipeline startup diagram</div>
  </center>
</div>


Tips:
> a. You can verify access via `https://cdn.jsdelivr.net/gh/{your-github-username}/clash-rules-lite@release/`   
> c. **All files ending with `rules.txt` in this repository will be cached to jsDelivr CDN, so you can customize them!**    


### Apply in Clash Desktop

1. Right-click the subscribed configuration file and choose "Copy", then name the copied file `local` (because updating the subscription link will overwrite your changes)

<div align="center">
  <center>
    <img width="800" style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://user-images.githubusercontent.com/35565811/184479698-dbc0f06b-7313-4448-a694-cad3d9d5dbe3.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">Copy a subscribed configuration file</div>
  </center>
</div>



2. In the copied `local` configuration, update settings as below. Replace `proxies`, `proxy-groups`, and `{YOUR-GITHUB-USERNAME}` with your own configuration (highlighted parts)


<pre><code> 
mixed-port: 7890
allow-lan: true
bind-address: '*'
mode: rule
log-level: silent
external-controller: '127.0.0.1:9090'
proxies:
    <b>- { name: '1-Hong Kong', type: *, server: **, port: *, cipher: **, password: **, udp: true }</b>
    <b>- { name: '2-Hong Kong', type: *, server: **, port: *, cipher: **, password: **, udp: true }</b>
    <b>- ...</b>
proxy-groups:
    <b>- { name: '🔰 Node Selection', type: select, proxies: ['1-Hong Kong', '2-Hong Kong'] }</b>
    <b>- { name: '🎯 Global Direct', type: select, proxies: ['DIRECT'] }</b>
    <b>- { name: '🛑 Global Block', type: select, proxies: ['REJECT'] }</b>
    <b>- { name: 'Ⓜ️ Microsoft Service', type: select, proxies: ['🎯 Global Direct', ] }</b>
    <b>- { name: '🐟 Final Fallback', type: select, proxies: ['🔰 Node Selection'] }</b>
    <b>- ...</b>
rules:
  - RULE-SET,Backlist,🛑 Global Block
  - RULE-SET,Proxy,🔰 Node Selection
  - RULE-SET,Microsoft,Ⓜ️ Microsoft Service
  - GEOIP,CN,🎯 Global Direct
  - MATCH,🐟 Final Fallback
rule-providers:
  Proxy:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/zhanyeye/clash-rules-lite@release/proxy-rules.txt"
    path: ./providers/rule-proxy.yaml
    interval: 86400
  Microsoft:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/zhanyeye/clash-rules-lite@release/microsoft-rules.txt"
    path: ./providers/rule-microsoft.yaml
    interval: 86400
  Backlist:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/zhanyeye/clash-rules-lite@release/blacklist-rules.txt"
    path: ./providers/rule-backlist.yaml
    interval: 86400 

</code></pre>


3. Run the modified `local` configuration, then switch to `Rule` mode

<div align="center">
  <center>
    <img width="800" style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://user-images.githubusercontent.com/35565811/184479791-6e2c12ca-d28f-4009-839a-e9a06bdcff00.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">Run the modified local configuration</div>
  </center>
</div>

### Apply in OpenClash on OpenWrt
> OpenWrt is a soft-router system. If you do not use it, please ignore this section.

You need to update `rules` and `rule-providers` in your config file. Note:
+ Replace the username with your own!!!
+ Replace the group in `rules` with your own `proxy-groups`!!!
```
rules:
  - RULE-SET,Backlist,🛑 Global Block
  - RULE-SET,Proxy,🔰 Node Selection
  - RULE-SET,Microsoft,Ⓜ️ Microsoft Service
  - GEOIP,CN,🎯 Global Direct
  - MATCH,🐟 Final Fallback
rule-providers:
  Proxy:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/zhanyeye/clash-rules-lite@release/proxy-rules.txt"
    path: ./providers/rule-proxy.yaml
    interval: 86400
  Microsoft:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/zhanyeye/clash-rules-lite@release/microsoft-rules.txt"
    path: ./providers/rule-microsoft.yaml
    interval: 86400
  Backlist:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/zhanyeye/clash-rules-lite@release/blacklist-rules.txt"
    path: ./providers/rule-backlist.yaml
    interval: 86400 
```





### Custom proxy rules
+ Just modify files ending with `rule.txt` in this repository. You can also add new files ending with `rule.txt`, and they will all take effect.
+ After modification, refresh in Clash and restart Clash for changes to apply.

> Refresh operation in Clash Desktop
<div align="center">
  <center>
    <img width="800" style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://user-images.githubusercontent.com/35565811/184480450-c24dd895-2b8a-4cfb-8f9e-77843c3df5af.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">Refresh providers in Clash client and restart Clash</div>
  </center>
</div>

> Refresh operation in OpenClash

Configuration File Management -> Rule Set File List -> Delete all files -> Back to Overview -> Apply configuration
<div align="center">
  <center>
    <img width="800" style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://user-images.githubusercontent.com/35565811/214744014-f348b5af-477f-465c-842d-e40d36d4a92b.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">Delete rule set files in OpenClash and re-apply configuration</div>
  </center>
</div>


+ What if jsDelivr CDN cache is not updated?

> This is caused by jsDelivr CDN caching. Usually it refreshes every 24 hours, but that is too slow!   
> jsDelivr CDN also provides a manual cache refresh method:
```
# Suppose your file URL is:
https://cdn.jsdelivr.net/xxx/xxx...

# Replace the domain from cdn to purge:
https://purge.jsdelivr.net/xxx/xxx...
```
Then access the new URL and it will show that refresh succeeded!

