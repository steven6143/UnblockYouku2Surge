# UnblockYouku2Surge
A `Surge for iOS` config file to let people outside of China access media contents exclusive to China. E.g., Bilibili Bangumi, Netease Cloud Music
# Credits
This file is based on the DNS server provided by [The Unblock Youku Project](https://github.com/uku/Unblock-Youku). By using this, you agree to accept their License and/or User Agreement.

You have to have `Surge for iOS` installed in order to use this config file.
# Usage
Just put it under the `Surge` folder. Bang, you are good to go.

If it fails, run this Python 3 script to generate a new one.
```
import requests
import re
# import clipboard
from bs4 import BeautifulSoup

unblock_youku_dns='158.69.209.100'

proxy_page=requests.get('http://cn-proxy.com').text
proxy_soup_bowl=BeautifulSoup(proxy_page, 'html.parser')
ip_soup_bowl=proxy_soup_bowl.find('tbody').find('tr').find_all('td')
ip=ip_soup_bowl[0].contents[0]
port=ip_soup_bowl[1].contents[0]
pac=requests.get('http://pac.uku.im/pac.pac').text
rexp='\,\"([a-z].+?[a-z])\"\:'
sites=re.findall(rexp,pac)
conf=''
for site in sites:
	rule=site+' = %s \n'%unblock_youku_dns
	if rule not in conf:
		if 'bilibili' not in rule:
			conf+=rule
conf+='bangumi.bilibili.com = %s'%unblock_youku_dns

header='''[General]
loglevel = notify
bypass-system = true
skip-proxy = 192.168.0.0/16, 10.0.0.0/8, 172.16.0.0/12, 127.0.0.1, localhost, *.local

[Proxy]
CN = http, %s, %s,,

[Rule]
IP-CIDR,103.211.228.142/22,CN
FINAL,DIRECT

[Host]
'''%(ip, port)
result=header+conf
# clipboard.set(result)
print(result)
```
# One more thing
Enjoy your freedom, welcome to China.
