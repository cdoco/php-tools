# HTTP 请求 & URL 处理

## 跳转重定向

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

## 清理URL中的 http 头信息

参数|注释|类型|默认值
-|-|-|-
$url|跳转地址|sting|-
$cleanall|是否清除所有|boolean|true

返回值|注释|类型
-|-|-
$url|跳转地址|sting

```php
public static function cleanUrl($url, $cleanall = true) {
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

## 获取当前域名

参数|注释|类型|默认值
-|-|-|-
$http|-|boolean|false
$entities|-|boolean|false

返回值|注释|类型
-|-|-
$host|当前域名|sting

```php
public static function getHttpHost($http = false, $entities = false) {
    $host = (isset($_SERVER['HTTP_X_FORWARDED_HOST']) ? $_SERVER['HTTP_X_FORWARDED_HOST'] : $_SERVER['HTTP_HOST']);

    if ($entities) {
        $host = htmlspecialchars($host, ENT_COMPAT, 'UTF-8');
    }

    if ($http) {
        $host = self::getCurrentUrlProtocolPrefix() . $host;
    }

    return $host;
}
```