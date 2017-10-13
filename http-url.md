## HTTP 请求 & URL

### 跳转重定向

参数|注释|类型|默认值
-|-|-|-
$url|跳转地址|sting|-
$headers|http 头信息|array|null

返回值|注释|类型
-|-|-
void|-|-

```php
public function redirect($url, $headers = null) {
    if (!empty($url)) {
        if ($headers) {
            if (!is_array($headers)) {
                $headers = array($headers);
            }
            foreach ($headers as $header) {
                header($header);
            }
        }
        header('Location: ' . $url);
        exit;
    }
}
```

### 清理URL中的 http 头信息

参数|注释|类型|默认值
-|-|-|-
$url|跳转地址|sting|-
$cleanall|是否清除所有|boolean|true

返回值|注释|类型
-|-|-
$url|跳转地址|sting

```php
public function cleanUrl($url, $cleanall = true) {
    if (strpos($url, 'http://') !== false) {
        if ($cleanall) {
            return '/';
        } else {
            return str_replace('http://', '', $url);
        }
    }
    return $url;
}
```

### 获取当前域名

参数|注释|类型|默认值
-|-|-|-
$http|是否返回http协议|boolean|false
$entities|是否转义|boolean|false

返回值|注释|类型
-|-|-
$host|当前域名|sting

```php
public function getHttpHost($http = false, $entities = false) {
    $host = (isset($_SERVER['HTTP_X_FORWARDED_HOST']) ? $_SERVER['HTTP_X_FORWARDED_HOST'] : $_SERVER['HTTP_HOST']);

    if ($entities) {
        $host = htmlspecialchars($host, ENT_COMPAT, 'UTF-8');
    }

    if ($http) {
        //这个方法在下文 getCurrentUrlProtocolPrefix
        $host = getCurrentUrlProtocolPrefix() . $host;
    }

    return $host;
}
```

### 获取当前URL协议

返回值|注释|类型
-|-|-
-|http协议(https\|http)|sting

```php
public function getCurrentUrlProtocolPrefix() {
    //这个方法在下文 usingSecureMode
    if (usingSecureMode()) {
        return 'https://';
    } else {
        return 'http://';
    }
}
```

### 判断是否使用了HTTPS

返回值|注释|类型
-|-|-
-|是否使用了HTTPS|boolean

```php
public function usingSecureMode() {
    if (isset($_SERVER['HTTPS'])) {
        return ($_SERVER['HTTPS'] == 1 || strtolower($_SERVER['HTTPS']) == 'on');
    }

    if (isset($_SERVER['SSL'])) {
        return ($_SERVER['SSL'] == 1 || strtolower($_SERVER['SSL']) == 'on');
    }

    return false;
}
```

### 获取当前服务器名

返回值|注释|类型
-|-|-
-|当前服务器名|string

```php
public function getServerName() {
    if (isset($_SERVER['HTTP_X_FORWARDED_SERVER']) && $_SERVER['HTTP_X_FORWARDED_SERVER']) {
        return $_SERVER['HTTP_X_FORWARDED_SERVER'];
    }

    return $_SERVER['SERVER_NAME'];
}
```

### 获取用户IP地址

返回值|注释|类型
-|-|-
-|用户IP地址|string

```php
public function getRemoteAddr() {
    if (isset($_SERVER['HTTP_X_FORWARDED_FOR']) && $_SERVER['HTTP_X_FORWARDED_FOR'] && (!isset($_SERVER['REMOTE_ADDR']) || preg_match('/^127\..*/i', trim($_SERVER['REMOTE_ADDR'])) || preg_match('/^172\.16.*/i', trim($_SERVER['REMOTE_ADDR'])) || preg_match('/^192\.168\.*/i', trim($_SERVER['REMOTE_ADDR'])) || preg_match('/^10\..*/i', trim($_SERVER['REMOTE_ADDR'])))) {
        if (strpos($_SERVER['HTTP_X_FORWARDED_FOR'], ',')) {
            $ips = explode(',', $_SERVER['HTTP_X_FORWARDED_FOR']);
            return $ips[0];
        } else
            return $_SERVER['HTTP_X_FORWARDED_FOR'];
    }
    return $_SERVER['REMOTE_ADDR'];
}
```

### 获取用户来源地址

返回值|注释|类型
-|-|-
-|用户来源地址|string

```php
public function getReferer() {
    if (isset($_SERVER['HTTP_REFERER'])) {
        return $_SERVER['HTTP_REFERER'];
    } else {
        return null;
    }
}
```

### 获取POST或GET的指定字段内容

参数|注释|类型|默认值
-|-|-|-
$key|需要获取的字段值|string|-
$defaultValue|字段默认值|boolean|false

返回值|注释|类型
-|-|-
$ret|字段对应值|sting\|boolean

```php
public function getValue($key, $defaultValue = false) {
    if (!isset($key) || empty($key) || !is_string($key)) {
        return false;
    }
    $ret = (isset($_POST[$key]) ? $_POST[$key] : (isset($_GET[$key]) ? $_GET[$key] : $defaultValue));

    if (is_string($ret) === true) {
        $ret = trim(urldecode(preg_replace('/((\%5C0+)|(\%00+))/i', '', urlencode($ret))));
    }

    return !is_string($ret) ? $ret : stripslashes($ret);
}
```

### 判断是否为提交操作

返回值|注释|类型
-|-|-
-|是否是提交操作|boolean

```php
public function isSubmit($submit) {
    return (isset($_POST[$submit]) || isset($_POST[$submit . '_x']) || isset($_POST[$submit . '_y']) || isset($_GET[$submit]) || isset($_GET[$submit . '_x']) || isset($_GET[$submit . '_y']));
}
```
