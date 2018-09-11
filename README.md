# PHP Windows ''performance'' builds #

----
These builds are based on original [PHP source from github](https://github.com/php/php-src) without any changes and including pecl modules xdebug & memcache.

Differences with official Windows build are:

- Performance optimization (with compiler flags) that may induce security issues (I use cli & cgi for personal use without any problem but not in industrial production ^^)  
- Latest MSVC & Windows Kit version  
- Static extension build, as possible  

Older versions are tagged as in php-src github...

----
**2018-09-11**

- [php 7.2.10 tag](https://github.com/php/php-src/tree/php-7.2.10)
- [memcache 3.0.9 NON_BLOCKING_IO_php7](https://github.com/websupport-sk/pecl-memcache/tree/NON_BLOCKING_IO_php7) _shared_
 - Patched with pull [#26](https://github.com/websupport-sk/pecl-memcache/pull/26/) to fix issue [#23](https://github.com/websupport-sk/pecl-memcache/issues/23#issuecomment-327702906) Failed to read session data with 7.1/7.2
- [Xdebug 2.6.1](https://github.com/xdebug/xdebug/tree/2.6.1) _shared_
- MSVC 15.8.3 / 19.15.26726
- Window Kit 10.0.17134.0
---
	-----------------------
	| Extension  | Mode   |
	-----------------------
	| bcmath     | static |
	| bz2        | static |
	| calendar   | static |
	| com_dotnet | static |
	| ctype      | static |
	| curl       | static |
	| date       | static |
	| dom        | static |
	| exif       | static |
	| fileinfo   | shared |
	| filter     | static |
	| gd         | shared |
	| hash       | static |
	| iconv      | static |
	| intl       | shared |
	| json       | static |
	| libxml     | static |
	| mbstring   | static |
	| memcache   | shared |
	| mysqli     | static |
	| mysqlnd    | static |
	| opcache    | shared |
	| openssl    | shared |
	| pcre       | static |
	| pdo        | static |
	| pdo_mysql  | static |
	| pdo_sqlite | static |
	| phar       | static |
	| readline   | static |
	| reflection | static |
	| session    | static |
	| simplexml  | static |
	| soap       | static |
	| sockets    | static |
	| spl        | static |
	| sqlite3    | static |
	| standard   | static |
	| tidy       | static |
	| tokenizer  | static |
	| xdebug     | shared |
	| xml        | static |
	| xmlreader  | static |
	| xmlwriter  | static |
	| zip        | static |
	| zlib       | static |
	-----------------------
	-------------
	| Sapi Name |
	-------------
	| cgi       |
	| cli       |
	-------------
	----------------------------------------------
	| Build type      | Release                  |
	| Compiler        | MSVC15 (Visual C++ 2017) |
	| Optimization    | PGO disabled             |
	| Static analyzer | disabled                 |
	----------------------------------------------`

**Dependencies**

- dll (non debug) from deps [x86](http://windows.php.net/downloads/php-sdk/deps/vc15/x86/) - [x64](http://windows.php.net/downloads/php-sdk/deps/vc15/x64/)
  - **Override libpng & libiconv by thoses provided in vc15\\%ARCH%\deps °**
- MSVC15 redist 14.15.26706 [x86](https://aka.ms/vs/15/release/VC_redist.x86.exe) - [x64](https://aka.ms/vs/15/release/VC_redist.x64.exe)

**CFLAGS add:** 

- [/GL](https://msdn.microsoft.com/en-us/library/0zza0de8.aspx)
- [/GS-](https://msdn.microsoft.com/en-us/library/8dbf701c.aspx)
- [/Oy-](https://msdn.microsoft.com/en-us/library/2kxx5t2c.aspx)
- **[/arch:AVX](https://msdn.microsoft.com/fr-fr/library/jj620901.aspx)**

**LDFLAGS add:** 

- [/LTCG ](https://msdn.microsoft.com/en-us/library/xbf3tbeh.aspx)
- [/NODEFAULTLIB](https://msdn.microsoft.com/en-us/library/3tz4da4a.aspx):[libcmt.lib ](https://msdn.microsoft.com/en-us/library/abx4dbyh.aspx)
- [/OPT:ICF](https://msdn.microsoft.com/en-us/library/bxwfs976.aspx)

° **see https://github.com/Microsoft/php-sdk-binary-tools/issues/36** *(self-compiled bins & working libs included in vc15\\%ARCH%\deps)*

**configure**

_--with-mp=8 \
--enable-object-out-dir=../build/ \
--disable-embed \
--disable-phpdbgs \
--disable-phpdbg \
--disable-cli-win32 \
--disable-test-ini \
--disable-debug \
--disable-debug-pack \
--disable-ipv6 \
--disable-phpdbg-webhelper \
--disable-crt-debug \
--disable-security-flags \
--without-enchant \
--without-imap \
--without-snmp \
--without-xmlrpc \
--without-xsl \
--without-gmp \
--without-wddx \
--without-libwebp \
--without-interbase \
--without-ldap \
--without-oci8 \
--without-pgsql \
--without-uncritical-warn-choke \
--enable-sockets \
--enable-mbstring \
--enable-exif \
--enable-intl \
--enable-memcache=shared \
--enable-pdo \
--enable-opcache \
--enable-soap \
--enable-fileinfo \
--enable-com-dotnet \
--enable-fd-setsize=2048 \
--enable-sanitizer \
--without-analyzer \
--with-curl \
--with-tidy \
--with-openssl \
--with-mysqli \
--with-pdo-mysql \
--with-bz2 \
--with-sqlite3 \
--with-pdo-sqlite \
--with-extra-includes="C:\Program Files (x86)\Windows Kits\NETFXSDK\4.7\Include\um";"C:\Program Files (x86)\Windows Kits\10\Include\10.0.17134.0\um" \
--with-extra-libs="C:\Program Files (x86)\Windows Kits\NETFXSDK\4.7\Lib\um\%PHP_SDK_ARCH%";"C:\Program Files (x86)\Windows Kits\10\Lib\10.0.17134.0\um\%PHP_SDK_ARCH%" \
--with-xdebug=shared \
%ZTS% \
\>> %LOGNAME% 2>&1_