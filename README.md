# PHP Windows ''performance'' builds #

----
These builds are based on an original [php source from github](https://github.com/php/php-src) without any changes and including pecl modules xdebug & memcache.

Differences with official Windows build are:

- Performance optimization (with compiler flags) that may induce security issues (I use cli & cgi for personal use without any problem but not in industrial production ^^)  
- Latest MSVC & Windows Kit version  
- Static extension build, as possible  

Older versions are tagged as in php-src github...

----
**2018-03-01**

    Build type : Release
    Compiler : MSVC15 (Visual C++ 2017)
    Optimization : PGO disabled
    Static analyzer : disabled

- [php 7.2.3 branch](https://github.com/php/php-src/tree/PHP-7.2.3)
- [memcache 3.0.9 NON_BLOCKING_IO_php7](https://github.com/websupport-sk/pecl-memcache/tree/NON_BLOCKING_IO_php7) _shared_
 - Patched with pull [#26](https://github.com/websupport-sk/pecl-memcache/pull/26/) to fix issue [#23](https://github.com/websupport-sk/pecl-memcache/issues/23#issuecomment-327702906) Failed to read session data with 7.1/7.2
- [Xdebug 2.6.0](https://github.com/xdebug/xdebug/tree/2.6.0) _shared_
- MSVC 15.5.7 / 19.12.25835
- Window Kit 10.0.16299.0

**Dependencies**

- dll (non debug) from deps [x86](http://windows.php.net/downloads/php-sdk/deps/vc15/x86/) - [x64](http://windows.php.net/downloads/php-sdk/deps/vc15/x64/)
- MSVC15 redist 14.12.25810 [x86](https://aka.ms/vs/15/release/VC_redist.x86.exe) - [x64](https://aka.ms/vs/15/release/VC_redist.x64.exe)

**CFLAGS add:** 

- [/GL](https://msdn.microsoft.com/en-us/library/0zza0de8.aspx) 
- [/GS-](https://msdn.microsoft.com/en-us/library/8dbf701c.aspx)
- [/Oy-](https://msdn.microsoft.com/en-us/library/2kxx5t2c.aspx)

**Bench results** 
  Done with [Zend/micro_bench.php](https://github.com/php/php-src/blob/master/Zend/micro_bench.php)

- 5.086 [Official Windows build](http://windows.php.net/downloads/releases/php-7.2.3-nts-Win32-VC15-x64.zip)  
- **4.148** *(18% less cpu ;)*   

**LDFLAGS add:** 

- [/LTCG ](https://msdn.microsoft.com/en-us/library/xbf3tbeh.aspx)
- [/NODEFAULTLIB](https://msdn.microsoft.com/en-us/library/3tz4da4a.aspx):[libcmt.lib ](https://msdn.microsoft.com/en-us/library/abx4dbyh.aspx)
- [/OPT:ICF](https://msdn.microsoft.com/en-us/library/bxwfs976.aspx)

**configure**

*configure  \  
--with-mp=4  \  
--enable-object-out-dir=../build/  \  
--disable-embed  \  
--disable-phpdbgs  \  
--disable-phpdbg  \  
--disable-cli-win32  \  
--disable-test-ini  \  
--disable-debug  \  
--disable-debug-pack  \  
--disable-ipv6  \  
--disable-phpdbg-webhelper  \  
--disable-zts  \  
--disable-intl  \  
--disable-crt-debug  \  
--disable-security-flags  \  
--without-enchant  \  
--without-imap  \  
--without-snmp  \  
--without-xmlrpc  \  
--without-xsl  \  
--without-gmp  \  
--without-wddx  \  
--without-libwebp  \  
--without-interbase  \  
--without-ldap  \  
--without-oci8  \  
--without-pgsql  \  
--without-uncritical-warn-choke  \  
--enable-sockets  \  
--enable-mbstring  \  
--enable-exif  \  
--enable-memcache  \  
--enable-pdo  \  
--enable-opcache  \  
--enable-soap  \  
--enable-fileinfo  \  
--enable-com-dotnet  \  
--enable-fd-setsize=2048  \  
--enable-sanitizer  \  
--without-analyzer  \  
--with-curl  \  
--with-tidy  \  
--with-openssl  \  
--with-mysqli  \  
--with-pdo-mysql  \  
--with-bz2  \  
--with-xdebug=shared  \  
--with-extra-includes="C:\Program Files (x86)\Windows Kits\NETFXSDK\4.7\Include\um";"C:\Program Files (x86)\Windows Kits\10\Include\10.0.16299.0\um"  \  
--with-extra-libs="C:\Program Files (x86)\Windows Kits\NETFXSDK\4.7\Lib\um\x64";"C:\Program Files (x86)\Windows Kits\10\Lib\10.0.16299.0\um\x64"*