# dumpDex
update 2015.10.27<br>
version 2.0<br>
dump2.py: 首先对代码整体结构进行了大修改，参考Dex Hunter不再对Dex文件结构进行全部解析，代码量一下就少了一半以上，代码有一处需要修改的地方就是DexFile的地址，可以通过1.0中提出的方案，当然也有更优雅的方案，见findcookie.py<br>
findcookie.py: 参考 [伪·MSC解题报告](http://bbs.pediy.com/showthread.php?t=197244) ,作者提出了一种基于gDvm获取cookie的办法，比起我1.0的办法更加直接方便。那么如何获取这个gDvm呢？我采用的办法是通过IDA的module模块找到libdvm.so->dvmlookupclass->dvmhashtablelookup,其中dvmhashtablelookup函数的第一个参数是gDvm的一个成员变量的地址，因此我们需要做的就是将这个地址记录并修改代码中的地址。<br>
说明：从上面获取的地址再偏移640字节可以得到gDvm的另一个成员变量useDexFiles，我们就是通过这个成员变量来发现所有已经加载的DexFile地址，然后将需要dump的DexFile地址填充到dump2.py中即可。<br>
version 1.0<br>
Source: dump.py modified the var 'addr'(cookie) in this file and run as python script in IDA for some information of the dex file loaded in memory. You can use zjdroid to figure out the value of the cookie.
Usage：代码中有一个addr需要手动修改，即cookie值，这个值你可以通过zjdroid工具来获取，在Github上可以找到，当然我也实现了相同功能纪录cookie值，见https://github.com/CvvT/DumpApk

经过一个星期的分析才总算明白xx聚的实现原理，又折腾了将近一个礼拜写了个工具来脱壳。 result/ 文件下是测试apk以及dump出来的dex文件。
简单说下脱壳工具实现原理： 可以参考zjdroid的实现，对内存直接进行dump，但是由于加固后dex文件并不是在内存中连续存放的，但依然满足dex的文件格式（通过将代码段移动到其他地方并将偏移量进行修改）。这里可以参考我的另一个项目DexParse，我把他移植到可在IDA上运行，那么即使在内存中并不连续存放也可以dump出来啦～
由于测试程序仅有一个，并不能保证没有Bug，如果你有什么发现可以告诉我喔～

