```
/**
 * 采集远程文件
 * @param string $remote 远程链接
 * @param string $local 本地保存文件名，不含后缀
 *
 * @return string
 */
function curlDownload($remote, $local)
{
    $cp = curl_init($remote);
    if(($headers=get_headers($remote, 1))!==false){
        // 获取响应的类型
        $type=$headers['Content-Type'];

        if($type == 'image/jpeg'){
            $local = $local.".jpg";
        }elseif($type == 'image/png'){
            $local = $local.".png";
        }
    }


    $fp = fopen($local, "w");
    curl_setopt($cp, CURLOPT_FILE, $fp);
    curl_setopt($cp, CURLOPT_HEADER, 0);
    curl_exec($cp);
    curl_close($cp);
    fclose($fp);

    return $local;
}
```

此功能一般在于下载用户的微信头像，区别在于需要识别png和jpg，识别的方式在于response中的Content-Type。

此功能函数位于：/Common/Common/function.php中

