应对一个小cc



1.防火墙	

2.创建防火墙规则

3.编辑表达式



```shell
(not ip.geoip.country in {"CN" "HK" "TW" "JP" "SG" "US" "KR"} and not cf.client.bot) or (http.request.uri.path contains "登录|注册|support|xmlrpc|admin|login|vpn|.zip|.rar|.tar.gz|search") or (not http.request.method in {"GET" "POST"}) or (ip.geoip.asnum in {12816 12786 18450 197540 24961 26496 35908 46606 54600 60068 22773 18978 7922 61317 6079 397391 46562 22616 26347 45916 22394 202594 40676 398101 396362 6167 54290 135981 21686 7303 138997 22418 140224 46475 20001 43959 41378 29802 10013 9824 4766 209 43260 7565 40676 3786 28438 13287 3786 24641} and not cf.client.bot) or (ip.geoip.asnum in {20473 2914 16276 24940 8100 45102 36352 135377 54994 3462 35908 12876 16276} and not cf.client.bot) or (cf.threat_score ge 5)
```

编辑完成后，选择操作 - JS质询或者质询(Captcha)



<br>

<br>



规则创建完成后，将攻击模式打开即可.









