# Genshin-Tools
获取原神Cookies等信息的方式归纳

## 获取米游社Cookie
打开你的浏览器,进入无痕/隐身模式  
打开 https://bbs.mihoyo.com/ys/ 并进行登入操作  
按下键盘上的F12或右键检查，打开开发者工具，点击Console(控制台)  
输入  
```var cookie=document.cookie;var ask=confirm('Cookie:'+cookie+'\n\nDo you want to copy the cookie to the clipboard?');if(ask==true){copy(cookie);msg=cookie}else{msg='Cancel'}```  
回车执行，并在确认无误后点击确定。  
此时Cookie已经复制到你的粘贴板上了。  

## 获取stuid和stoken
**stuid**就是你的**米游社账号**ID
**stoken**需要浏览器打开 https://user.mihoyo.com/ 并登录  
按下键盘上的F12或右键检查，打开开发者工具，点击Console(控制台)  
复制如下内容并粘贴输入，回车执行 
```
function getCookieMap(cookie) {
   let cookiePattern = /^(\S+)=(\S+)$/;
   let cookieArray = cookie.split("; ");
   let cookieMap = new Map();
   for (let item of cookieArray) {
      let entry = cookiePattern.exec(item);
      cookieMap.set(entry[1], entry[2]);
   }
   return cookieMap;
}

const map = getCookieMap(document.cookie);
const loginTicket = map.get("login_ticket");
const loginUid = map.get("login_uid");
const url = "https://api-takumi.mihoyo.com/auth/api/getMultiTokenByLoginTicket?login_ticket=" + loginTicket + "&token_types=3&uid=" + loginUid;
fetch(url, {
   "headers": {
      "x-rpc-device_id": "zxcvbnmasadfghjk123456",
      "Content-Type": "application/json;charset=UTF-8",
      "x-rpc-client_type": "",
      "x-rpc-app_version": "",
      "DS": "",
      "User-Agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 14_0_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) miHoYoBBS/%s",
      "Referer": "cors",
      "Accept-Encoding": "gzip, deflate, br",
      "x-rpc-channel": "appstore",
   },
   "method": "GET"
}).then(
        function (response) {
           if (response.status !== 200) {
              return;
           }
           response.json().then(function (data) {
              console.log(data);
           });
        }
).catch(function (err) {
   console.log('Fetch Error :-S', err);
});
```
按照如图位置找到**stoken**，复制即可
![截图](https://cdn.jsdelivr.net/gh/Immortal68/SSZ-Index/sns/example.png)
