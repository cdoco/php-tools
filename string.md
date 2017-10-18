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
public function strlen($str, $encoding = 'UTF-8') {
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

### 转换成小写字符

参数|注释|类型|默认值
-|-|-|-
$str|需要转换的字符串|string|-

返回值|注释|类型
-|-|-
-|转换后的字符串|string|bool

```php
public function strtolower($str) {
    if (is_array($str)) {
        return false;
    }

    if (function_exists('mb_strtolower')) {
        return mb_strtolower($str, 'utf-8');
    }

    return strtolower($str);
}
```

### 转换成大写字符串

参数|注释|类型|默认值
-|-|-|-
$str|需要转换的字符串|string|-

返回值|注释|类型
-|-|-
-|转换后的字符串|string|bool

```php
public static function strtoupper($str) {
    if (is_array($str)) {
        return false;
    }

    if (function_exists('mb_strtoupper')) {
        return mb_strtoupper($str, 'utf-8');
    }

    return strtoupper($str);
}
```