# ʹ��Heroku����Xray�����ܴ������ͨ��ws����� (vmess��vless��trojan shadowsocks��socks)��Э��

> ���ѣ� ���ÿ��ܵ����˻���BAN������ 

# 9��21���޸�

## ����

������ Heroku �ϲ��� vless+websocket+tls��ÿ�β����Զ�ѡ�����µ� alpine linux �� Xray core ��  
vless ���ܸ������㣬ռ����Դ���١�

* ʹ��[xray](https://github.com/XTLS/Xray-core)+caddyͬʱ����ͨ��ws�����vmess vless trojan shadowsocks socks��Э�飬��Ĭ�������ú�αװ��վ��
* ֧��tor���磬�ҿ�ͨ���Զ������������ļ�����xray��caddy���������ø��ֹ���  
* ֧�ִ洢�Զ����ļ�,Ŀ¼���˺������ΪUUID,�ͻ������ʹ��TLS����  
  **Heroku Ϊ�����ṩ����ѵ������������ǲ�Ӧ�������������Ա���Ŀ������Ϊ���ڷ�ǽʹ�á�**

## ����

�����񲻻���Ϊ����ռ����Դ������š�ע���Heroku�˺Ų���¼��,������水ť��ɲ���.

### �����

[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://dashboard.heroku.com/new?template=https://github.com/githubsunshine771dahi/XrayHeroku) 

���������ɫ`Deploy to Heroku`������ת��heroku app����ҳ�棬����Ӧ�õ����ơ�ѡ��ڵ�(������ŷ�޽ڵ㣬�����ڵ���Զ�ɾ��YouTube��������ޣ�)�������޸Ĳ��ֲ�����UUID��������`deploy`��ʼ��������Ӧ��  
����ִ��󣬿��Զೢ�Լ��Σ���������ɺ�ҳ��ײ�����ʾ`Your app was successfully deployed` 
  * ���Manage App����Settings�µ�Config Vars��**�鿴���������ò���**  
  * ���Open app��ת[��ӭҳ��](/etc/CADDYIndexPage.md)������Ϊheroku������������ʽΪ`xxx.herokuapp.com`�����ڿͻ���  
  * Ĭ��Э������Ϊ`24b4b1e1-7a89-45f6-858c-242cf53b5bdb`��WS·��Ϊ$UUID-[vmess|vless|trojan|ss|socks]��ʽ

### �ͻ���
* **����滻���е�`xxx.herokuapp.com`Ϊheroku�������Ŀ����**  
* **����滻���е�`24b4b1e1-7a89-45f6-858c-242cf53b5bdb`Ϊ����ʱ���õ�UUID,�������,��Ҫÿ���˶�һ��**  

**XRay ���ڲ���ʱ���Զ�ʵ�䰲װ`���°汾`��**

**���ڰ�ȫ����������ʹ�� CDN�������벻Ҫʹ���Զ�����������ʹ�� Heroku ����Ķ�����������ʵ�� XRay vless Websocket + TLS��**

<details>
<summary>V2rayN(Xray��V2ray)</summary>

```bash
* �ͻ������أ�https://github.com/2dust/v2rayN/releases
* ����Э�飺vless �� vmess
* ��ַ��xxx.herokuapp.com
* �˿ڣ�443
* Ĭ��UUID��24b4b1e1-7a89-45f6-858c-242cf53b5bdb
* vmess����id��0
* ���ܣ�none
* ����Э�飺ws
* αװ���ͣ�none
* αװ������xxx.workers.dev(CF Workers������ַ)
* ·����/24b4b1e1-7a89-45f6-858c-242cf53b5bdb-vless // Ĭ��vlessʹ��(/�Զ���UUID��-vless)��vmessʹ��(/�Զ���UUID��-vmess)
* �ײ㴫�䰲ȫ��tls
* ����֤����֤��false
```
</details>

<details>
<summary>Trojan-Go</summary>

```bash
* �ͻ�������: https://github.com/p4gefau1t/trojan-go/releases
{
    "run_type": "client",
    "local_addr": "127.0.0.1",
    "local_port": 1080,
    "remote_addr": "xxx.herokuapp.com",
    "remote_port": 443,
    "password": [
        "24b4b1e1-7a89-45f6-858c-242cf53b5bdb"
    ],
    "websocket": {
        "enabled": true,
        "path": "/24b4b1e1-7a89-45f6-858c-242cf53b5bdb-trojan",
        "host": "xxx.herokuapp.com"
    }
}
```
</details>

<details>
<summary>Shadowsocks</summary>

```bash
* �ͻ������أ�https://github.com/shadowsocks/shadowsocks-windows/releases/
* ��������ַ: xxx.herokuapp.com
* �˿�: 443
* ���룺24b4b1e1-7a89-45f6-858c-242cf53b5bdb
* ���ܣ�chacha20-ietf-poly1305
* �������xray-plugin_windows_amd64.exe  //�轫���https://github.com/shadowsocks/xray-plugin/releases���ؽ�ѹ�����shadowsocksͬĿ¼
* ���ѡ��: tls;host=xxx.herokuapp.com;path=/24b4b1e1-7a89-45f6-858c-242cf53b5bdb-ss
```
</details>

<details>
<summary>����ʹ��Cloudflare��Workers����ת��������֧��VLESS\VMESS\Trojan-Go��WSģʽ������Ϊ��</summary>

```js
const SingleDay = 'xxx.herokuapp.com'
const DoubleDay = 'xxx.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        if (nd.getDate()%2) {
            host = SingleDay
        } else {
            host = DoubleDay
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>

## OpenWrt��ѡIP�ű��Զ����£�

* [CloudflareST](https://github.com/Lbingyi/CloudflareST) `OpenWrt�Ƽ�-�ٶȽϿ�`
* [cf-autoupdate](https://github.com/Lbingyi/cf-autoupdate) `OpenWrt�Ƽ�`

> [����������������PR��ʹ�ý̳�](/tutorial)

## ����CFɸѡIP

* ��ο� [CloudflareSpeedTest](https://github.com/XIU2/CloudflareSpeedTest) `�Ƽ�`
* ��ο� [better-cloudflare-ip](https://github.com/badafans/better-cloudflare-ip)

### �ر��л ��

* [mixool](https://github.com/mixool/)
* [bclswl0827](https://github.com/bclswl0827/v2ray-heroku)
* [yxhit](https://github.com/yxhit)
* [badafans](https://github.com/badafans/better-cloudflare-ip/tree/20201208)
