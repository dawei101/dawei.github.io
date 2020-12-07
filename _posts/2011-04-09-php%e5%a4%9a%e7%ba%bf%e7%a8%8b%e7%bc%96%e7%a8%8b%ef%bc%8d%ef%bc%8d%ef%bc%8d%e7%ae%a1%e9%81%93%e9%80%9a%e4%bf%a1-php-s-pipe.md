---
id: 68
title: 'PHP多线程编程－－－管道通信 | php "s pipe'
date: 2011-04-09T11:03:42+00:00
author: dawei
layout: post
guid: http://bookwikiup.com/blog/?p=68
permalink: /2011/04/09/php%e5%a4%9a%e7%ba%bf%e7%a8%8b%e7%bc%96%e7%a8%8b%ef%bc%8d%ef%bc%8d%ef%bc%8d%e7%ae%a1%e9%81%93%e9%80%9a%e4%bf%a1-php-s-pipe/
categories:
  - 未分类
---
管道通信：  
1. 管道可以认为是一个队列，不同的线程都可以往里面写东西，也都可以从里面读东西。写就是  
在队列末尾添加，读就是在队头删除。
2. 管道一般有大小，默认一般是4K，也就是内容超过4K了，你就只能读，不能往里面写了。
3. 默认情况下，管道写入以后，就会被阻止，直到读取他的程序读取把数据读完。而读取线程也会被阻止，   
直到有进程向管道写入数据。当然，你可以改变这样的默认属性，用stream\_set\_block 函数，设置成非阻断模式。

下面是我分装的一个管道的类（这个类命名有问题，没有统一，没有时间改成统一的了，我一般先写测试代码，最后分装，所以命名上可能不统一）：  

```
<?php  

class Pipe  {   

    public $fifoPath;   
    private $w_pipe;   
    private $r_pipe;   
    /**   
    * 自动创建一个管道   
    *   
    * @param string $name 管道名字   
    * @param int $mode 管道的权限，默认任何用户组可以读写   
    */   
    function __construct($name = "pipe", $mode = 0666)   
    {   
        $fifoPath = "/tmp/$name." . posix_getpid();   
        if (!file_exists($fifoPath)) {   
            if (!posix_mkfifo($fifoPath, $mode)) {   
                error("create new pipe ($name) error.");   
                return false;   
            }   
        } else {   
            error( "pipe ($name) has exit.");   
            return false;   
        }   
        $this->fifoPath = $fifoPath;   
    }

    // 写管道函数开始  
    function open_write() {   
        $this->w_pipe = fopen($this->fifoPath, "w");   
        if ($this->w_pipe == NULL) {   
            error("open pipe {$this->fifoPath} for write error.");   
            return false;   
        }   
        return true;   
    }

    function write($data)   
    {   
        return fwrite($this->w_pipe, $data);   
    }   

    function write_all($data) {   
        $w_pipe = fopen($this->fifoPath, "w");   
        fwrite($w_pipe, $data);   
        fclose($w_pipe);
    }   
    function close_write() {   
        return fclose($this->w_pipe);   
    }  

    // 读管道相关函数开始  
    function open_read() {   
        $this->r_pipe = fopen($this->fifoPath, "r");   
        if ($this->r_pipe == NULL) {   
            error("open pipe {$this->fifoPath} for read error.");   
            return false;   
        }   
        return true;   
    }   

    function read($byte = 1024) {   
        return fread($this->r_pipe, $byte);   
    }   

    function read_all() {   
        $r_pipe = fopen($this->fifoPath, "r");   
        $data = "";   
        while (!feof($r_pipe)) {   
            //echo "read one Kn";   
            $data .= fread($r_pipe, 1024);   
        }   
        fclose($r_pipe);   
        return $data;   
    }   

    function close_read() {   
        return fclose($this->r_pipe);   
    }  

    ////////////////////////////////////////////////////   
    /**   
    * 删除管道   
    *   
    * @return boolean is success   
    */   
    function rm_pipe() {   
        return unlink($this->fifoPath);   
    }  
}  
?>
```

有了这个类，就可以实现简单的管道通信了，因为这个教程是多线程编程系列教程的一个部分。  
这个管道类的应用部分，将放到第三部分。
