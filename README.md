# XMR-Stak-CPU - Monero mining software

XMR-Stak is a universal Stratum pool miner. This is the CPU-mining version; there is also an [AMD GPU version](https://github.com/fireice-uk/xmr-stak-amd) and an [NVIDA GPU version](https://github.com/fireice-uk/xmr-stak-nvidia)

## HTML reports


## HTML and JSON API report configuraton

To configure the reports shown above you need to edit the httpd_port variable. Then enable wifi on your phone and navigate to [miner ip address]:[httpd_port] in your phone browser. If you want to use the data in scripts, you can get the JSON version of the data at url [miner ip address]:[httpd_port]/api.json

## Usage on Windows 
1) Edit the config.txt file to enter your pool login and password. 
2) Double click the exe file. 

XMR-Stak should compile on any C++11 compliant compiler.
```
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA256

sha1sum
d34a0ba0dd7b3b1f900a7e02772e197e974b4a73  libeay32.dll
2ee9966a0fc163da58408d91be36b84fa287c10b  ssleay32.dll
e4d8a974e58985214de163df0c1ed0f54250d7ee  xmr-stak-cpu.exe
ae0153ff98df82022b2c392d6a17c5f3614f6a50  xmr-stak-cpu-notls.exe

sha3sum
05003137a87313c81d6c348c9b96411c95d48dc22c35f36c39129747  libeay32.dll
133c065d9ef2c93396382e2ba5d8c3ca8c6a57c6beb0159cb9a4b6c5  ssleay32.dll
7bfc30b06524dc9139a3157e2661d2a6f5720738dde8e490f05cc8e2  xmr-stak-cpu.exe
005fb81fc3711a97b2ce65bad0ca97318d878dc793a8cba99c7d1f6f  xmr-stak-cpu-notls.exe

date
Wed 19 Jul 21:18:58 BST 2017
-----BEGIN PGP SIGNATURE-----
Version: GnuPG v2

iQEcBAEBCAAGBQJZb77XAAoJEPsk95p+1Bw0GU4H/26sBwJzYSeWoLwo0LdmOPk3
19n+svFYnz6NlxAjs+fvuTK992ilLMy2pa4PHKhot2oyZIgt2rRaFsvRADcHVraG
nsIh4Oq31T9epZI0WxIH5FJlDx30fdGkpMTu9xt6ta2JXsmkDiCoZxmETuljB7Rw
xvnKeHiuTccp73C6Nd7dkuiemsOw0FZA7XXS/Kmwqm7n8BtCztY70R6SVN7QFbCz
C49s0A9cT4UbAUPuu8KvxFozmJHA/wDBYHgkq95Y6n/q116+Sc9BpdF8j+qK4YzZ
uM+B10XY0g7Qv376UoJRYKokpVaBxF08nD+JXLdL+zfQvnEfKgrhTnjaTkWFfEY=
=jpgE
-----END PGP SIGNATURE-----
```
## Compile guides

- [Free BSD](FREEBSDCOMPILE.md)
- [Linux](LINUXCOMPILE.md)
- [Windows](WINCOMPILE.md)


#### CPU mining performance 

Performance is nearly identical to the closed source paid miners. Here are some numbers:

* **I7-2600K** - 266 H/s
* **I7-6700** - 276 H/s (with a separate GPU miner)
* **Dual X5650** - 466 H/s (depends on NUMA)
* **Dual E5640** - 365 H/s (same as above)


## Common Issues

**SeLockMemoryPrivilege failed**

Please see [config.txt](config.txt) under section **LARGE PAGE SUPPORT**

For Windows 7 pro, or Windows 8 and above see [this article](https://msdn.microsoft.com/en-gb/library/ms190730.aspx)  (make sure to reboot afterwards!).

For Windows 7 Home :

1) Download and install [Windows Server 2003 Resource Kit Tools](https://www.microsoft.com/en-us/download/details.aspx?id=17657).  Ignore incompatiablity warning during installation.

2) In cmd or power shell: `ntrights -u %USERNAME% +r SeLockMemoryPrivilege`  (where %USERNAME% is the user that will be running the program.  This command needs to be run as admin)

3) Reboot.

Reference: http://rybkaforum.net/cgi-bin/rybkaforum/topic_show.pl?pid=259791#pid259791

*Warning: do not download ntrights.exe from any other site other then the offical Microsoft download page.*

**VirtualAlloc failed**

If you set up the user rights properly (see above), and your system has 4-8GB of RAM (50%+ use), there is a significant chance that there simply won't be a large enough chunk of contiguous memory because Windows is fairly bad at mitigating memory fragmentation.

If that happens, disable all auto-staring applications and run the miner after a reboot.

**msvcp140.dll and vcruntime140.dll not available errors**

Download and install this [runtime package](https://go.microsoft.com/fwlink/?LinkId=746572) from Microsoft.  *Warning: Do NOT use "missing dll" sites - dll's are exe files with another name, and it is a fairly safe bet that any dll on a shady site like that will be trojaned.  Please download offical runtimes from Microsoft above.*


**Error: MEMORY ALLOC FAILED: mmap failed**

From [config.txt](config.txt):

On Linux you will need to configure large page support `sudo sysctl -w vm.nr_hugepages=128` and increase your
ulimit -l. To do do this you need to add following lines to /etc/security/limits.conf:

    * soft memlock 262144
    * hard memlock 262144
    
Save file.  You WILL need to log out and log back in for these settings to take affect on your user (no need to reboot, just relogin in your session).
   
You can also do it Windows-style and simply run-as-root, but this is NOT recommended for security reasons.

**Illegal instruction (core dumped)**

This typically means you are trying to run it on a CPU that does not have [AES](https://en.wikipedia.org/wiki/AES_instruction_set).  This only happens on older version of miner, new version gives better error message (but still wont' work since your CPU doesn't support the required instructions).


## Advanced Compile Options

The build system is CMake, if you are not familiar with CMake you can learn more [here](https://cmake.org/runningcmake/).

### Short Description

There are two easy ways to set variables for `cmake` to configure *xmr-stak-cpu*
- use the ncurses GUI
  - `ccmake .`
  - edit your options
  - end the GUI by pressing the key `c`(create) and than `g`(generate)
- set Options on the command line
  - enable a option: `cmake . -DNAME_OF_THE_OPTION=ON`
  - disable a option `cmake . -DNAME_OF_THE_OPTION=OFF`
  - set a value `cmake . -DNAME_OF_THE_OPTION=value`

After the configuration you need to call
`make install` for slow sequential build
or
`make -j install` for faster parallel build
and install.

### xmr-stak-cpu Compile Options
- `CMAKE_INSTALL_PREFIX` install miner to the home folder
  - `cmake . -DCMAKE_INSTALL_PREFIX=$HOME/xmr-stak-cpu`
  - you can find the binary and the `config.txt` file after `make install` in `$HOME/xmr-stak-cpu/bin`
- `CMAKE_LINK_STATIC` link libgcc and libstdc++ libraries static (default OFF)
  - disable with `cmake . -DCMAKE_LINK_STATIC=ON`
-`CMAKE_BUILD_TYPE` set the build type
  - valid options: `Release` or `Debug`
  - you should always keep `Release` for your productive miners
- `MICROHTTPD_ENABLE` allow to disable/enable the dependency *microhttpd*
  - by default enabled
  - there is no *http* interface available if option is disabled: `cmake . -DMICROHTTPD_ENABLE=OFF`
- `OpenSSL_ENABLE` allow to disable/enable the dependency *OpenSSL*
  - by default enabled
  - it is not possible to connect to a *https* secured pool if option is disabled: `cmake . -DOpenSSL_ENABLE=OFF`
- `HWLOC_ENABLE` allow to disable/enable the dependency *hwloc*
  - by default enabled
  - the config suggestion is not optimal if option is disabled: `cmake . -DHWLOC_ENABLE=OFF`

