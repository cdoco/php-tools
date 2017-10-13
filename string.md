## 字符串

### 计算字符串长度

参数|注释|类型|默认值
-|-|-|-
$str|需要计算的字符串|string|-
$encoding|字符串编码|string|UTF-8

返回值|注释|类型
-|-|-
-|字符串长度|integer

```php
public static function strlen($str, $encoding = 'UTF-8') {
    if (is_array($str) || is_object($str)) {
        return false;
    }

    $str = html_entity_decode($str, ENT_COMPAT, 'UTF-8');
    if (function_exists('mb_strlen')) {
        return mb_strlen($str, $encoding);
    }

    return strlen($str);
}
```