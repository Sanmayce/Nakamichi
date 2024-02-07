# Nakamichi
    
Nakamichi 'Dragoneye' highlights:  
    
- The latest Zennish LZSS Microdeduplicator;  
- File-to-File [de]compressor;  
- Superfast decompression rates, superslow compression rates;  
- On big (1000++MB) textual data, second only to Oodle and Hamid's LzTurbo 29, ratiowise, resourcewise and speedwise - TRIPLE TRUMP :P;  
- Single-threaded Non-SIMD console tool written in plain C, compileable under Windows and Linux;  
- An LZSS (Lempel–Ziv–Storer–Szymanski) implementation with Greedy Parsing and 1TB Sliding Window;  
- Ability to deduplicate (as little as) 64 bytes long chunks 1TB backwards;  
- Targets huge textual datasets (mainly English), weak-'n'-slow on binary data;  
- One goal is to boost traversing (full-text parsing) of the whole XML dump of Wikipedia being ~64GB strong via TRANSPARENT decompression;  
- The first matchfinder using both the fastest memmem() Railgun ‘Trolldom’ and B-trees;  
- The first parser using both Internal or External RAM, decided by a single command line option - 'i' or 'e';  
- Hashpot/hashpool (residing in Physical RAM) could be tuned via command line parameter, thus lessening the B-trees heights/attempts;  
- The B-trees form the second layer, the first being HASH table handled by FNV1A-Pippip;  
- The Leprechaunesque (Internal/External) B-trees order 3 (2 keys MAX) are highly-optimized;  
- DEPRECIATED (too slow): To keep LEAF’s footprint small, keys 36/64 bytes long are hashed by SHA3-224, otherwise left intact;  
- The building of B-trees is done in 128 PASSES, thus LOCALITY/LOCALIZATION leads to cache-friendliness, for example, instead of confusing/blinding
  the SSD controller with building 2^27 ~= 128M B-trees at a time, 'PASSES' revision lowers the "noise/mayhem" 128 times by processing 1M B-trees at a time;  
- 100% FREE;  
- Trivially to return building B-trees in System RAM in passes - thus saving the SSD from trashing - ONLY SEQUENTIAL DUMPS - and much faster also;  
- 2019-Aug-15: INCOMING! Trivially to skip inserting UNIQUE KEYS into B-trees - thus saving big_time and big_space;  
- SCALABLE! Gets faster when more Physical or/and External RAM is available, on servers with 1TB RAM (or desktops with 64GB and 1TB Optane SSD) it will dance...  
- 2020-Dec-08: At last, here comes Nakamichi 'Dragoneye' Double-Deuce, it needs "only" roughly 2N+(29N = 28,495MB) or 32N to compress N=~1GB.  
    
![Nlogo](https://github.com/Sanmayce/Nakamichi/assets/14062548/5b1eb2a5-72e3-4989-bb01-f14e77b1b777)
    
Homepage:  
http://www.sanmayce.com/Nakamichi/  
    
Benchmarking 'TDELCC' a.k.a. The-Definitive-English-Language-Compression-Corpus, a smashdown, https://github.com/Sanmayce/Nakamichi  
Benchmarking 'TDJLCC' a.k.a. The-Definitive-Japanese-Language-Compression-Corpus, a smashdown, https://github.com/Sanmayce/Nakamichi  
Benchmarking 'ISTA9' a.k.a. INTERNET_SACRED_TEXT_ARCHIVE_DVD-ROM_9-Compression-Corpus, a smashdown, https://github.com/Sanmayce/Nakamichi  
Benchmarking 'llvm-project' a.k.a. CLANG-Compression-Corpus, a smashdown, https://github.com/Sanmayce/Nakamichi  
    
Another iteration of Sanmayce's decompression showdown 'FULG', revision 4+, all performers are included in the package.
    
Always, it is good to get the picture how the latest compressors fare in TEXTUAL realm.
The name of the game is: applying maximum compression strength, aiming at maximum decompression ... speed, heh-heh.
    
Included compressors:
```
RAR v.7.00beta3 by Alexander Roshal, Russia;
BR, Brotli v.1.1.0 by Jyrki Alakuijala, Finland;
ZPAQ v.7.15 by Matt Mahoney, America;
GZ, 7zip's GZ v.23.01 by Igor Pavlov, Russia;
BZ2, 7zip's BZ2 v.23.01 by Igor Pavlov, Russia;
7Z, 7zip's 7Z v.23.01 by Igor Pavlov, Russia;
ZSTD v.1.5.5 by Yann Collet aka Cyan, France;
BSC v.3.3.3 by Ilya Grebnov aka Gribok, Russia;
LZSSE by Conor Stokes, Australia;
Satanichi, Sanmayce's texttoy, Bulgaria;
BriefLZ v.1.3.0 by Joergen Ibsen, Denmark.
```
    
Compression command lines:
```
/bin/time -v ./brotli_1.1.0 -q 11 --large_window=30 "$1" 
/bin/time -v ./rarlinux-x64-700b3 a -m5 -md2g "$1".rar "$1"
/bin/time -v ./7zzs a "$1".7z -mx9 -myx9 -m0=LZMA2:d1536m "$1"
/bin/time -v ./7zzs a -tbzip2 -mx=9 "$1.bz2" "$1"
/bin/time -v ./7zzs a -tgzip -mx=9 "$1.gz" "$1"
/bin/time -v ./BSC_3.3.3_AVX2_CLANG_17.0.4_dynamic.elf e "$1" "$1.bsc" -p -b2047 -m0 -e2
/bin/time -v ./zstd-v1.5.5 --ultra -22 --long=31 --zstd=wlog=31,clog=30,hlog=30,slog=26 "$1" -o "$1.zst"
/bin/time -v ./LZSSE_avx2_CLANG.elf -2 -l17 "$1" "$1.lzsse2"
/bin/time -v ./BriefLZ_1.3.0_CLANG_17.0.4_64bit.elf --optimal -b3g "$1" "$1.blz"
/bin/time -v ./zpaq715_sse4.1.elf add "$1.zpaq" "$1" -method 511 -threads 4
/bin/time -v ./"Satanichi_Nakamichi_Vanilla_LITE_DD-128AES_CLANG(17.0.4)_64bit.elf" "$1" "$1.Nakamichi" 20 111000 i
```
    
Decompression command lines:
```
perf stat -d ./brotli_1.1.0 -d -k "$1".br 
perf stat -d ./rarlinux-x64-700b3 x "$1".rar
perf stat -d ./7zzs e "$1.7z"
perf stat -d ./7zzs e "$1.bz2"
perf stat -d ./7zzs e "$1.gz"
perf stat -d ./BSC_3.3.3_AVX2_CLANG_17.0.4_dynamic.elf d "$1.bsc" "$1"
perf stat -d ./zstd-v1.5.5 -f --priority=rt -d --long=31 "$1.zst"
perf stat -d ./LZSSE_avx2_CLANG.elf -d "$1.lzsse2" "$1" 
perf stat -d ./BriefLZ_1.3.0_CLANG_17.0.4_64bit.elf -d -b3g "$1.blz" "$1"
perf stat -d ./zpaq715_sse4.1.elf  x "$1.zpaq" -threads 4
perf stat -d ./"Satanichi_Nakamichi_Vanilla_LITE_DD-128AES_CLANG(17.0.4)_64bit.elf" $1.Nakamichi>$1.NKMCH
```
    
Corpus #1:
Testmachinette: Laptop 'Dzvertcheto' Thinkpad L490, Intel i7-8565U (8) @ 4.600GHz, 64GB, Linux Fedora 39
Testdatafile: sha1: 8326b48e3a315f4f656013629226c319fefd483e SUPRAPIG_Delphi_Classics_Complete_Works_of_128_authors.tar (1,576,788,480 bytes)
```
+-----------------------------+-----------------+-----------[ Sorted by Walltime ]-+------------------+---------------------+
| Compressor                  | Compressed size | Walltime / Usertime / Systemtime | Memory footprint |     CPU utilization |
+---------------[ SuperFAST ]-+-----------------+----------------------------------+------------------+---------------------+
| BSC_3.3.3_AVX2_CLANG_17.0.4 |     304,827,632 |             1:10.1 / 514.9 / 6.6 |     7,710,336 KB |                743% |
+--------------------[ FAST ]-+-----------------+----------------------------------+------------------+---------------------+
| LZSSE_avx2_CLANG            |     572,282,023 |             3:04.8 / 183.3 / 1.1 |     3,331,200 KB |                 99% |
| rarlinux-x64-700b3          |     399,313,787 |            3:24.9 / 1388.2 / 3.3 |     7,658,240 KB |                678% |
+------------------[ Normal ]-+-----------------+----------------------------------+------------------+---------------------+
| 7zzs_23.01's bz2            |     414,301,737 |            8:03.3 / 3766.3 / 0.9 |        77,824 KB |                779% |
| 7zzs_23.01's gz             |     544,531,970 |           20:09.7 / 1207.4 / 0.3 |         5,376 KB |                 99% |
| 7zzs_23.01's 7z             |     366,878,089 |           24:45.1 / 1810.0 / 7.2 |    15,963,904 KB |                122% |
| zstd-v1.5.5                 |     374,058,071 |           31:29.5 / 1883.6 / 3.3 |    10,314,220 KB |                 99% |
+--------------------[ SLOW ]-+-----------------+----------------------------------+------------------+---------------------+
| BriefLZ_1.3.0_CLANG_17.0.4  |     476,307,190 |          49:20.2 / 2945.3 / 10.0 |    32,803,328 KB |                 99% |
| zpaq715_sse4.1              |     289,466,679 |           1:04:35 / 3860.0 / 9.0 |    19,023,136 KB |                 99% |
| brotli_1.1.0                |     370,294,709 |          1:07:48 / 4057.3 / 4.02 |     9,974,512 KB |                 99% |
+---------------[ UltraSLOW ]-+-----------------+----------------------------------+------------------+---------------------+
| Satanichi_CLANG_17.0.4      |     474,713,658 |        209,045 / 54,455 / 19,461 |            64+GB | 0.359 CPUs utilized |            
+-----------------------------+-----------------+----------------------------------+------------------+---------------------+
```
Note01a: Nakamichi thrashes the virtual RAM (since it needs ~(61-(Source-Buffer + Target-Buffer = 2 + 3)-67)=-11 gigabytes more than 64GB), seen by the 6h systemtime.
Note01b: Satanichi monstrously devours physical RAM, like 3TB, in order to flex its muscles. ! RAM needed to house B-trees (relative to the file being ripped): 44N = 66,224MB; RAM needed to build B-trees IN ONE PASS: (Target-Buffer = 2,503 MB) x 64 passes = 160,192MB ! So, drastically reduced time for compression if 230 GB are available. In case of all indexes fit in RAM, the encoding speed is 100 KB/s.
    
Corpus #1:
Testmachinette: Laptop 'Dzvertcheto' Thinkpad L490, Intel i7-8565U (8) @ 4.600GHz, 64GB, Linux Fedora 39
Testdatafile: sha1: 8326b48e3a315f4f656013629226c319fefd483e SUPRAPIG_Delphi_Classics_Complete_Works_of_128_authors.tar (1,576,788,480 bytes)
```
+-----------------------------+-----------------+-----------[ Sorted by Walltime ]-+-----------------------+--------------------+----------------------------------+ 
| Decompressor                | Compressed size | Walltime / Usertime / Systemtime |       CPU utilization |       Instructions |      LLC-loads / LLC-load-misses |
+---------------[ UltraFAST ]-+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
| LZSSE_avx2_CLANG            |     572,282,023 |                  0.8 / 0.3 / 0.4 |   1.000 CPUs utilized |      5,276,911,595 |                563,939 / 151,203 |
| LZSSE_avx2_GCC              |     572,282,023 |                  0.8 / 0.3 / 0.4 |   0.999 CPUs utilized |      5,316,121,126 |                545,723 / 140,118 |
+---------------[ SuperFAST ]-+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
| Satanichi_GCC_13.2.1        |     474,713,658 |                  2.7 / 1.9 / 0.8 |   1.000 CPUs utilized |      4,211,049,650 |         177,744,185 / 57,211,272 |
| Satanichi_CLANG_17.0.4      |     474,713,658 |                  2.7 / 1.9 / 0.8 |   1.000 CPUs utilized |      4,243,632,674 |         179,001,727 / 57,137,600 |
| zstd-v1.5.5                 |     374,058,071 |                  2.9 / 2.5 / 0.8 |   1.175 CPUs utilized |     19,913,312,819 |           49,593,392 / 6,507,280 |
+--------------------[ FAST ]-+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
| brotli_1.1.0                |     370,294,709 |                  6.1 / 5.2 / 0.8 |   1.000 CPUs utilized |     17,065,934,394 |         171,518,735 / 82,851,994 |
| rarlinux-x64-700b3          |     399,313,787 |                  6.7 / 9.3 / 0.9 |   1.531 CPUs utilized |     37,354,158,966 |         166,230,623 / 85,640,325 |
| BriefLZ_1.3.0_CLANG_17.0.4  |     476,307,190 |                  6.9 / 6.0 / 0.8 |   1.000 CPUs utilized |     27,125,792,763 |          88,295,646 / 31,016,221 |
| BriefLZ_1.3.0_GCC_13.2.1    |     476,307,190 |                  8.1 / 7.2 / 0.8 |   1.000 CPUs utilized |     31,513,004,141 |          90,967,111 / 32,762,390 |
| 7zzs_23.01's gz             |     544,531,970 |                  8.8 / 8.4 / 0.3 |   1.000 CPUs utilized |     60,531,034,012 |              1,131,330 / 129,222 |
| 7zzs_23.01's 7z             |     366,878,089 |                14.5 / 13.5 / 0.8 |   1.000 CPUs utilized |     76,506,480,464 |         143,437,881 / 68,732,482 |
| 7zzs_23.01's bz2            |     414,301,737 |                19.2 / 28.3 / 0.4 |   1.509 CPUs utilized |    132,876,974,414 |       1,340,889,710 / 11,315,495 |
| BSC_3.3.3_AVX2_CLANG_17.0.4 |     304,827,632 |               29.8 / 213.1 / 4.1 |   7.347 CPUs utilized |    604,969,912,535 |    2,348,629,362 / 1,233,981,644 |
+--------------------[ SLOW ]-+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
| zpaq715_sse4.1              |     289,466,679 |            4031.6 / 4000.1 / 9.6 |   1.000 CPUs utilized | 24,939,199,778,486 | 136,354,757,447 / 28,877,270,011 |
+-----------------------------+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
```
Note01: The Walltime includes LOAD-DECOMPRESS-DUMP times, that is, external-RAM -> internal-RAM -> external-RAM.
Note02: The decompression is done on RamDisk of size 32GB, both the compressed and the decompressed files are on it.
Note03: Comparison was made, each decompressed file was compared with the original.
Note04a: The last column is quite informative, latencywise, the Last-Level-Cache misses value is indicative how much physical RAM (and cache hierarchy) stalls the CPU.
Note04b: For instance, every 177,744,185 / 57,211,272 = 3.1rd attempt to load from Last-Level-Cache is denied, it says, that with bigger L3 (i7-8565U has 8 MB), Nakamichi's main bottleneck has less impact.
Note05: Decompression times are the fastest of three runs, enforcing sleeping for 7 seconds in between in order to cool off.
Note06: Another useful measure is DIPB which stands for Decompression-Instructions-Per-Byte, since Nakamichi is simplistic and uses no entropy stage it has the lowest 4,211,049,650 / 1,576,788,480 = 2.67 DIPB;
Note07: The whole Read-Decompress-Write trio is done on RAM disk, created as follows:
```
sudo mkdir /tmp/ramdisk
sudo chmod 777 /tmp/ramdisk
sudo mount -t tmpfs -o size=32G myramdisk /tmp/ramdisk
#sudo umount /tmp/ramdisk/
```
Note08: Joergen's BriefLZ was compiled with these lines:
```
gcc -O3 -fomit-frame-pointer -fstrict-aliasing -o BriefLZ_1.3.0_GCC_13.2.1_64bit.elf -I../include blzpack.c parg.c ../src/brieflz.c ../src/depack.c ../src/depacks.c -D_N_HIGH_PRIORITY
clang -O3 -fomit-frame-pointer -fstrict-aliasing -o BriefLZ_1.3.0_CLANG_17.0.4_64bit.elf -I../include blzpack.c parg.c ../src/brieflz.c ../src/depack.c ../src/depacks.c -D_N_HIGH_PRIORITY
```
    
Corpus #2:
Testmachinette: Laptop 'Dzvertcheto' Thinkpad L490, Intel i7-8565U (8) @ 4.600GHz, 64GB, Linux Fedora 39
Testdatafile: sha1: a8b1df94bfb88e5cc005367ad3597ad292c07922 SUPRAPIG_Last_century_5109_Japanese_TXT_Books_Shift-JIS_encoding.tar (1,550,303,744 bytes)
```
+-----------------------------+-----------------+-----------[ Sorted by Walltime ]-+------------------+---------------------+
| Compressor                  | Compressed size | Walltime / Usertime / Systemtime | Memory footprint |     CPU utilization |
+---------------[ SuperFAST ]-+-----------------+----------------------------------+------------------+---------------------+
| BSC_3.3.3_AVX2_CLANG_17.0.4 |     354,154,306 |            1:20.67 / 580.7 / 7.1 |     7,581,292 KB |                728% |
+--------------------[ FAST ]-+-----------------+----------------------------------+------------------+---------------------+
| LZSSE_avx2_CLANG            |     653,943,920 |            2:45.17 / 163.8 / 1.1 |     3,339,520 KB |                 99% |
| rarlinux-x64-700b3          |     459,327,014 |           3:22.23 / 1383.2 / 3.3 |     7,658,624 KB |                685% |
+------------------[ Normal ]-+-----------------+----------------------------------+------------------+---------------------+
| 7zzs_23.01's bz2            |     476,543,611 |           7:30.24 / 3508.3 / 0.8 |        77,824 KB |                779% |
| 7zzs_23.01's gz             |     627,263,006 |          16:53.81 / 1011.9 / 0.3 |         5,504 KB |                 99% |
| 7zzs_23.01's 7z             |     412,941,160 |          19:55.41 / 1506.9 / 6.5 |    15,730,944 KB |                126% |
| zstd-v1.5.5                 |     424,549,781 |          26:56.34 / 1610.7 / 3.3 |    10,454,768 KB |                 99% |
+--------------------[ SLOW ]-+-----------------+----------------------------------+------------------+---------------------+
| zpaq715_sse4.1              |     327,138,192 |          58:39.83 / 3505.1 / 7.5 |    15,999,804 KB |                 99% |
| brotli_1.1.0                |     422,063,712 |           1:05:18 / 3908.1 / 3.8 |     9,908,260 KB |                 99% |
| BriefLZ_1.3.0_CLANG_17.0.4  |     531,603,242 |           2:30:58 / 9032.7 / 9.6 |    32,313,964 KB |                 99% |
+---------------[ UltraSLOW ]-+-----------------+----------------------------------+------------------+---------------------+
| Satanichi_CLANG_17.0.4      |     544,028,165 |             45,340 / 45,084 / 15 |       ~54,545 MB | 1.000 CPUs utilized |            
+-----------------------------+-----------------+----------------------------------+------------------+---------------------+
```
Note01a: Nakamichi fits (Source-Buffer + Target-Buffer + HASH memory + B-trees pool = 1,478 MB + 2,478 MB + 142,606,401 bytes + 50,447 MB ~= 54,545 MB) in the physical RAM thus not thrashing the virtual RAM, seen by the 15 seconds systemtime.
Note01b: Satanichi monstrously devours physical RAM, like 3TB, in order to flex its muscles. ! RAM needed to house B-trees (relative to the file being ripped): 34N = 50,447MB; RAM needed to build B-trees IN ONE PASS: (Target-Buffer = 2,478 MB) x 64 passes = 158,592MB !
    
Corpus #2:
Testmachinette: Laptop 'Dzvertcheto' Thinkpad L490, Intel i7-8565U (8) @ 4.600GHz, 64GB, Linux Fedora 39
Testdatafile: sha1: a8b1df94bfb88e5cc005367ad3597ad292c07922 SUPRAPIG_Last_century_5109_Japanese_TXT_Books_Shift-JIS_encoding.tar (1,550,303,744 bytes)
```
+-----------------------------+-----------------+-----------[ Sorted by Walltime ]-+-----------------------+--------------------+----------------------------------+ 
| Decompressor                | Compressed size | Walltime / Usertime / Systemtime |       CPU utilization |       Instructions |      LLC-loads / LLC-load-misses |
+---------------[ UltraFAST ]-+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
| LZSSE_avx2_CLANG            |     653,943,920 |               0.90 / 0.40 / 0.49 |   1.000 CPUs utilized |      6,076,928,213 |              2,840,828 / 174,205 |
| LZSSE_avx2_GCC              |            N.A. |                             N.A. |                  N.A. |               N.A. |                             N.A. |
+---------------[ SuperFAST ]-+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
| Satanichi_GCC_13.2.1        |     544,028,165 |                  2.9 / 2.0 / 0.8 |   1.000 CPUs utilized |      5,060,020,726 |         168,714,587 / 45,072,161 |
| Satanichi_CLANG_17.0.4      |     544,028,165 |                  2.9 / 2.0 / 0.8 |   1.000 CPUs utilized |      5,062,802,223 |         168,898,391 / 45,094,160 |
| zstd-v1.5.5                 |     424,549,781 |                  3.1 / 2.7 / 0.9 |   1.170 CPUs utilized |     23,095,590,431 |           50,585,595 / 6,344,792 |
+--------------------[ FAST ]-+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
| brotli_1.1.0                |     422,063,712 |                  6.7 / 5.9 / 0.7 |   1.000 CPUs utilized |     20,254,268,738 |         177,906,005 / 82,355,259 |
| rarlinux-x64-700b3          |     459,327,014 |                 7.3 / 10.6 / 0.9 |   1.580 CPUs utilized |     42,881,124,929 |         166,394,756 / 81,482,519 |
| BriefLZ_1.3.0_CLANG_17.0.4  |     531,603,242 |                  7.4 / 6.5 / 0.8 |   1.000 CPUs utilized |     29,079,334,052 |          92,068,764 / 31,116,402 |
| BriefLZ_1.3.0_GCC_13.2.1    |     531,603,242 |                  8.8 / 7.9 / 0.8 |   1.000 CPUs utilized |     34,112,544,792 |          94,205,769 / 32,212,752 |
| 7zzs_23.01's gz             |     627,263,006 |                  9.9 / 9.4 / 0.3 |   1.000 CPUs utilized |     65,675,266,878 |              1,686,456 / 138,433 |
| 7zzs_23.01's 7z             |     412,941,160 |                15.7 / 14.8 / 0.8 |   1.000 CPUs utilized |     86,315,213,859 |         146,222,789 / 65,743,254 |
| 7zzs_23.01's bz2            |     476,543,611 |                20.4 / 31.1 / 0.4 |   1.557 CPUs utilized |    135,470,579,050 |       1,287,816,007 / 31,184,416 |
| BSC_3.3.3_AVX2_CLANG_17.0.4 |     354,154,306 |               32.5 / 228.2 / 3.9 |   7.211 CPUs utilized |    674,130,036,838 |    2,596,309,424 / 1,146,081,701 |
+--------------------[ SLOW ]-+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
| zpaq715_sse4.1              |     327,138,192 |            3521.3 / 3493.8 / 7.9 |   1.000 CPUs utilized | 22,421,410,663,967 | 121,361,845,952 / 25,669,929,198 |
+-----------------------------+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
```
Note01: The Walltime includes LOAD-DECOMPRESS-DUMP times, that is, external-RAM -> internal-RAM -> external-RAM.
Note02: The decompression is done on RamDisk of size 32GB, both the compressed and the decompressed files are on it.
Note03: Comparison was made, each decompressed file was compared with the original.
Note04a: The last column is quite informative, latencywise, the Last-Level-Cache misses value is indicative how much physical RAM (and cache hierarchy) stalls the CPU.
Note04b: For instance, every 168,714,587 / 45,072,161 = 3.7rd attempt to load from Last-Level-Cache is denied, it says, that with bigger L3 (i7-8565U has 8 MB), Nakamichi's main bottleneck has less impact.
Note05: Decompression times are the fastest of three runs, enforcing sleeping for 7 seconds in between in order to cool off.
Note06: Another useful measure is DIPB which stands for Decompression-Instructions-Per-Byte, since Nakamichi is simplistic and uses no entropy stage it has the lowest 5,060,020,726 / 1,550,303,744 = 3.2 DIPB;
Note07: The whole Read-Decompress-Write trio is done on RAM disk, created as follows:
```
sudo mkdir /tmp/ramdisk
sudo chmod 777 /tmp/ramdisk
sudo mount -t tmpfs -o size=32G myramdisk /tmp/ramdisk
#sudo umount /tmp/ramdisk/
```
Note08: Joergen's BriefLZ was compiled with these lines:
```
gcc -O3 -fomit-frame-pointer -fstrict-aliasing -o BriefLZ_1.3.0_GCC_13.2.1_64bit.elf -I../include blzpack.c parg.c ../src/brieflz.c ../src/depack.c ../src/depacks.c -D_N_HIGH_PRIORITY
clang -O3 -fomit-frame-pointer -fstrict-aliasing -o BriefLZ_1.3.0_CLANG_17.0.4_64bit.elf -I../include blzpack.c parg.c ../src/brieflz.c ../src/depack.c ../src/depacks.c -D_N_HIGH_PRIORITY
```
    
Corpus #3:
Testmachinette: Laptop 'Dzvertcheto' Thinkpad L490, Intel i7-8565U (8) @ 4.600GHz, 64GB, Linux Fedora 39
Testdatafile: sha1: 7c2e32a76716e184d302e5542b96c16e95047002 SUPRAPIG_INTERNET_SACRED_TEXT_ARCHIVE_DVD-ROM_9_(English_140479_htm_files).tar (2,037,880,832 bytes)
```
+-----------------------------+-----------------+-----------[ Sorted by Walltime ]-+------------------+---------------------+
| Compressor                  | Compressed size | Walltime / Usertime / Systemtime | Memory footprint |     CPU utilization |
+---------------[ SuperFAST ]-+-----------------+----------------------------------+------------------+---------------------+
| BSC_3.3.3_AVX2_CLANG_17.0.4 |     267,814,670 |             1:17.9 / 570.7 / 8.2 |     9,962,076 KB |                743% |
+--------------------[ FAST ]-+-----------------+----------------------------------+------------------+---------------------+
| rarlinux-x64-700b3          |     350,711,057 |            3:03.3 / 1181.9 / 3.6 |     7,657,984 KB |                646% |
| LZSSE_avx2_CLANG            |     521,597,750 |             7:00.0 / 418.0 / 1.1 |     3,322,368 KB |                 99% |
+------------------[ Normal ]-+-----------------+----------------------------------+------------------+---------------------+
| 7zzs_23.01's bz2            |     353,704,135 |            9:13.6 / 4312.8 / 0.9 |        77,056 KB |                779% |
| 7zzs_23.01's 7z             |     321,907,843 |           13:57.7 / 1446.0 / 8.6 |    20,860,288 KB |                173% |
| 7zzs_23.01's gz             |     488,604,010 |           25:06.6 / 1503.1 / 0.4 |         6,016 KB |                 99% |
| zstd-v1.5.5                 |     326,798,131 |           1:02:29 / 3738.4 / 3.4 |    10,695,680 KB |                 99% |
+--------------------[ SLOW ]-+-----------------+----------------------------------+------------------+---------------------+
| brotli_1.1.0                |     319,982,698 |           1:13:33 / 4400.6 / 3.9 |    10,429,760 KB |                 99% |
| zpaq715_sse4.1              |     238,923,738 |           1:16:26 / 4566.6 / 9.7 |    20,007,888 KB |                 99% |
| BriefLZ_1.3.0_CLANG_17.0.4  |     405,702,359 |         3:41:15 / 13232.2 / 12.6 |    42,190,464 KB |                 99% |
+---------------[ UltraSLOW ]-+-----------------+----------------------------------+------------------+---------------------+
| Satanichi_CLANG_17.0.4      |     432,873,383 |        566,317 / 62,504 / 39,265 |            64+GB | 0.185 CPUs utilized |            
+-----------------------------+-----------------+----------------------------------+------------------+---------------------+
```
Note01a: Nakamichi thrashes the virtual RAM (since it needs ~(61-(Source-Buffer + Target-Buffer = 2 + 3)-76)=-20 gigabytes more than 64GB), seen by the 11h systemtime.
Note01b: Satanichi monstrously devours physical RAM, like 3TB, in order to flex its muscles. ! RAM needed to house B-trees (relative to the file being ripped): 38N = 75,724MB; RAM needed to build B-trees IN ONE PASS: (Target-Buffer = 2,943 MB) x 64 passes = 188,352MB !
    
Corpus #3:
Testmachinette: Laptop 'Dzvertcheto' Thinkpad L490, Intel i7-8565U (8) @ 4.600GHz, 64GB, Linux Fedora 39
Testdatafile: sha1: 7c2e32a76716e184d302e5542b96c16e95047002 SUPRAPIG_INTERNET_SACRED_TEXT_ARCHIVE_DVD-ROM_9_(English_140479_htm_files).tar (2,037,880,832 bytes)
```
+-----------------------------+-----------------+-----------[ Sorted by Walltime ]-+-----------------------+--------------------+----------------------------------+ 
| Decompressor                | Compressed size | Walltime / Usertime / Systemtime |       CPU utilization |       Instructions |      LLC-loads / LLC-load-misses |
+---------------[ UltraFAST ]-+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
| LZSSE_avx2_CLANG            |     521,597,750 |                  0.9 / 0.3 / 0.5 |   1.000 CPUs utilized |      5,446,417,257 |                391,149 / 146,845 |
| LZSSE_avx2_GCC              |     521,597,750 |                  0.9 / 0.3 / 0.5 |   0.999 CPUs utilized |      5,485,240,527 |              2,472,893 / 149,586 |
+---------------[ SuperFAST ]-+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
| Satanichi_GCC_13.2.1        |     432,873,383 |                  2.6 / 1.6 / 1.0 |   1.000 CPUs utilized |      4,006,019,521 |         137,592,039 / 37,626,730 |
| Satanichi_CLANG_17.0.4      |     432,873,383 |                  2.6 / 1.6 / 1.0 |   0.999 CPUs utilized |      4,048,561,234 |         137,508,111 / 37,838,639 |
| zstd-v1.5.5                 |     326,798,131 |                  2.8 / 2.3 / 1.0 |   1.207 CPUs utilized |     18,953,888,238 |           35,860,333 / 4,471,114 |
+--------------------[ FAST ]-+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
| brotli_1.1.0                |     319,982,698 |                  5.1 / 4.2 / 0.8 |   1.000 CPUs utilized |     16,688,165,788 |         134,135,869 / 57,539,639 |
| rarlinux-x64-700b3          |     350,711,057 |                  5.9 / 8.3 / 1.0 |   1.606 CPUs utilized |     36,354,413,105 |         123,776,918 / 55,145,545 |
| BriefLZ_1.3.0_CLANG_17.0.4  |     405,702,359 |                  6.0 / 4.9 / 1.0 |   1.000 CPUs utilized |     23,086,259,385 |          66,334,306 / 20,713,229 |
| BriefLZ_1.3.0_GCC_13.2.1    |     405,702,359 |                  7.1 / 6.0 / 1.0 |   1.000 CPUs utilized |     26,710,299,630 |          68,268,256 / 21,800,075 |
| 7zzs_23.01's gz             |     488,604,010 |                  8.6 / 8.1 / 0.4 |   1.000 CPUs utilized |     59,616,405,462 |                915,238 / 132,888 |
| 7zzs_23.01's 7z             |     321,907,843 |                 9.8 / 11.8 / 0.9 |   1.317 CPUs utilized |     76,506,480,464 |          96,269,869 / 41,505,146 |
| 7zzs_23.01's bz2            |     353,704,135 |                21.7 / 29.7 / 0.5 |   1.400 CPUs utilized |    141,996,631,030 |       1,395,292,593 / 11,185,167 |
| BSC_3.3.3_AVX2_CLANG_17.0.4 |     267,814,670 |               28.8 / 203.5 / 5.3 |   7.303 CPUs utilized |    553,604,817,977 |    2,847,560,264 / 1,364,667,779 |
+--------------------[ SLOW ]-+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
| zpaq715_sse4.1              |     238,923,738 |            4583.8 / 4551.0 / 9.8 |   1.000 CPUs utilized | 32,252,231,612,296 | 157,765,885,275 / 26,776,989,764 |
+-----------------------------+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
```
Note01: The Walltime includes LOAD-DECOMPRESS-DUMP times, that is, external-RAM -> internal-RAM -> external-RAM.
Note02: The decompression is done on RamDisk of size 32GB, both the compressed and the decompressed files are on it.
Note03: Comparison was made, each decompressed file was compared with the original.
Note04a: The last column is quite informative, latencywise, the Last-Level-Cache misses value is indicative how much physical RAM (and cache hierarchy) stalls the CPU.
Note04b: For instance, every 137,592,039 / 37,626,730 = 3.6rd attempt to load from Last-Level-Cache is denied, it says, that with bigger L3 (i7-8565U has 8 MB), Nakamichi's main bottleneck has less impact.
Note05: Decompression times are the fastest of three runs, enforcing sleeping for 7 seconds in between in order to cool off.
Note06: Another useful measure is DIPB which stands for Decompression-Instructions-Per-Byte, since Nakamichi is simplistic and uses no entropy stage it has the lowest 4,006,019,521 / 2,037,880,832 = 1.96 DIPB;
Note07: The whole Read-Decompress-Write trio is done on RAM disk, created as follows:
```
sudo mkdir /tmp/ramdisk
sudo chmod 777 /tmp/ramdisk
sudo mount -t tmpfs -o size=32G myramdisk /tmp/ramdisk
#sudo umount /tmp/ramdisk/
```
Note08: Joergen's BriefLZ was compiled with these lines:
```
gcc -O3 -fomit-frame-pointer -fstrict-aliasing -o BriefLZ_1.3.0_GCC_13.2.1_64bit.elf -I../include blzpack.c parg.c ../src/brieflz.c ../src/depack.c ../src/depacks.c -D_N_HIGH_PRIORITY
clang -O3 -fomit-frame-pointer -fstrict-aliasing -o BriefLZ_1.3.0_CLANG_17.0.4_64bit.elf -I../include blzpack.c parg.c ../src/brieflz.c ../src/depack.c ../src/depacks.c -D_N_HIGH_PRIORITY
```
    
Corpus #4:
Testmachinette: Laptop 'Dzvertcheto' Thinkpad L490, Intel i7-8565U (8) @ 4.600GHz, 64GB, Linux Fedora 39
12097d13d39fc8c1058ab457c52d2d0193e5fe6f llvm-project-llvmorg-17.0.6.tar (1,591,029,760 bytes)
```
+-----------------------------+-----------------+-----------[ Sorted by Walltime ]-+------------------+---------------------+
| Compressor                  | Compressed size | Walltime / Usertime / Systemtime | Memory footprint |     CPU utilization |
+---------------[ SuperFAST ]-+-----------------+----------------------------------+------------------+---------------------+
| BSC_3.3.3_AVX2_CLANG_17.0.4 |     116,470,438 |             0:49.0 / 359.2 / 5.7 |     7,780,192 KB |                744% |
+--------------------[ FAST ]-+-----------------+----------------------------------+------------------+---------------------+
| rarlinux-x64-700b3          |     127,457,279 |             1:05.9 / 357.0 / 3.1 |     7,664,768 KB |                546% |
| 7zzs_23.01's 7z             |     116,894,728 |             5:06.8 / 408.6 / 5.2 |    16,088,960 KB |                134% |
+------------------[ Normal ]-+-----------------+----------------------------------+------------------+---------------------+
| 7zzs_23.01's bz2            |     144,583,792 |            6:44.3 / 3136.6 / 0.6 |        86,528 KB |                775% |
| 7zzs_23.01's gz             |     181,720,077 |            15:41.1 / 939.4 / 0.2 |         5,888 KB |                 99% |
| zstd-v1.5.5                 |     117,661,414 |            15:48.7 / 944.2 / 3.0 |    10,173,688 KB |                 99% |
+--------------------[ SLOW ]-+-----------------+----------------------------------+------------------+---------------------+
| brotli_1.1.0                |     112,986,823 |           43:44.4 / 2616.9 / 3.2 |     9,889,556 KB |                 99% |
| zpaq715_sse4.1              |      84,682,636 |           50:40.6 / 3025.2 / 8.4 |    19,048,548 KB |                 99% |
| LZSSE_avx2_CLANG            |     226,951,857 |          4:26:29 / 15954.9 / 1.0 |     3,313,152 KB |                 99% |
| BriefLZ_1.3.0_CLANG_17.0.4  |     145,491,157 |        47:33:58 / 170918.7 / 9.0 |    32,771,712 KB |                 99% |
+---------------[ UltraSLOW ]-+-----------------+----------------------------------+------------------+---------------------+
| Satanichi_CLANG_17.0.4      |     188,393,093 |    196:09:34 / 41839.2 / 40334.8 |            64+GB |                 11% |            
+-----------------------------+-----------------+----------------------------------+------------------+---------------------+
```
Note01a: Nakamichi thrashes the virtual RAM (since it needs ~(61-(Source-Buffer + Target-Buffer = 2 + 3)-91)=-35 gigabytes more than 64GB), seen by the 11h systemtime.
Note01b: Satanichi monstrously devours physical RAM, like 3TB, in order to flex its muscles. ! RAM needed to house B-trees (relative to the file being ripped): 60N = 91,236MB; RAM needed to build B-trees IN ONE PASS: (Target-Buffer = 2,517 MB) x 32 passes = 80,544MB !
    
Corpus #4:
Testmachinette: Laptop 'Dzvertcheto' Thinkpad L490, Intel i7-8565U (8) @ 4.600GHz, 64GB, Linux Fedora 39
12097d13d39fc8c1058ab457c52d2d0193e5fe6f llvm-project-llvmorg-17.0.6.tar (1,591,029,760 bytes)
```
+-----------------------------+-----------------+-----------[ Sorted by Walltime ]-+-----------------------+--------------------+----------------------------------+ 
| Decompressor                | Compressed size | Walltime / Usertime / Systemtime |       CPU utilization |       Instructions |      LLC-loads / LLC-load-misses |
+---------------[ UltraFAST ]-+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
| LZSSE_avx2_CLANG            |     226,951,857 |                  0.6 / 0.2 / 0.4 |   1.000 CPUs utilized |      3,222,477,036 |                 209,064 / 74,791 |
| LZSSE_avx2_GCC              |     226,951,857 |                  0.6 / 0.2 / 0.4 |   0.999 CPUs utilized |      3,317,830,555 |               1,270,675 / 80,838 |
+---------------[ SuperFAST ]-+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
| Satanichi_GCC_13.2.1        |     188,393,093 |                  1.3 / 0.6 / 0.7 |   0.999 CPUs utilized |      1,836,625,675 |           28,855,272 / 6,903,526 |
| Satanichi_CLANG_17.0.4      |     188,393,093 |                  1.3 / 0.6 / 0.7 |   1.000 CPUs utilized |      1,889,317,708 |           29,828,663 / 7,112,914 |
| zstd-v1.5.5                 |     117,661,414 |                  1.4 / 1.0 / 0.7 |   1.302 CPUs utilized |      8,534,370,499 |            9,672,650 / 1,975,249 |
+--------------------[ FAST ]-+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
| BriefLZ_1.3.0_CLANG_17.0.4  |     145,491,157 |                  2.3 / 1.6 / 0.7 |   1.000 CPUs utilized |      7,703,552,442 |           13,228,551 / 3,693,984 |
| brotli_1.1.0                |     112,986,823 |                  2.4 / 1.6 / 0.7 |   0.994 CPUs utilized |      8,233,086,933 |          42,817,876 / 19,184,585 |
| rarlinux-x64-700b3          |     127,457,279 |                  2.5 / 3.4 / 0.8 |   1.730 CPUs utilized |     17,628,730,001 |           26,857,253 / 9,257,837 |
| BriefLZ_1.3.0_GCC_13.2.1    |     145,491,157 |                  2.6 / 1.8 / 0.7 |   1.000 CPUs utilized |      8,949,711,285 |           13,994,851 / 3,813,098 |
| 7zzs_23.01's gz             |     181,720,077 |                  3.9 / 3.5 / 0.3 |   0.999 CPUs utilized |     29,854,266,082 |                 484,803 / 59,805 |
| 7zzs_23.01's 7z             |     116,894,728 |                  5.2 / 4.4 / 0.7 |   1.000 CPUs utilized |     36,683,547,724 |           15,930,581 / 5,714,606 |
| BSC_3.3.3_AVX2_CLANG_17.0.4 |     116,470,438 |               16.7 / 106.5 / 4.1 |   6.673 CPUs utilized |    264,802,909,956 |      1,657,277,994 / 628,861,530 |
| 7zzs_23.01's bz2            |     144,583,792 |                17.6 / 20.7 / 0.4 |   1.206 CPUs utilized |     88,497,518,406 |         757,384,043 / 46,842,848 |
+--------------------[ SLOW ]-+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
| zpaq715_sse4.1              |      84,682,636 |            3031.7 / 3007.1 / 8.8 |   1.000 CPUs utilized | 24,793,303,929,656 |  111,818,506,686 / 9,730,114,070 |
+-----------------------------+-----------------+----------------------------------+-----------------------+--------------------+----------------------------------+
```
Note01: The Walltime includes LOAD-DECOMPRESS-DUMP times, that is, external-RAM -> internal-RAM -> external-RAM.
Note02: The decompression is done on RamDisk of size 32GB, both the compressed and the decompressed files are on it.
Note03: Comparison was made, each decompressed file was compared with the original.
Note04a: The last column is quite informative, latencywise, the Last-Level-Cache misses value is indicative how much physical RAM (and cache hierarchy) stalls the CPU.
Note04b: For instance, every 28,855,272 / 6,903,526 = 4.1th attempt to load from Last-Level-Cache is denied, it says, that with bigger L3 (i7-8565U has 8 MB), Nakamichi's main bottleneck has less impact.
Note05: Decompression times are the fastest of three runs, enforcing sleeping for 7 seconds in between in order to cool off.
Note06: Another useful measure is DIPB which stands for Decompression-Instructions-Per-Byte, since Nakamichi is simplistic and uses no entropy stage it has the lowest 1,836,625,675 / 1,591,029,760 = 1.15 DIPB;
Note07: The whole Read-Decompress-Write trio is done on RAM disk, created as follows:
```
sudo mkdir /tmp/ramdisk
sudo chmod 777 /tmp/ramdisk
sudo mount -t tmpfs -o size=32G myramdisk /tmp/ramdisk
#sudo umount /tmp/ramdisk/
```
Note08: Joergen's BriefLZ was compiled with these lines:
```
gcc -O3 -fomit-frame-pointer -fstrict-aliasing -o BriefLZ_1.3.0_GCC_13.2.1_64bit.elf -I../include blzpack.c parg.c ../src/brieflz.c ../src/depack.c ../src/depacks.c -D_N_HIGH_PRIORITY
clang -O3 -fomit-frame-pointer -fstrict-aliasing -o BriefLZ_1.3.0_CLANG_17.0.4_64bit.elf -I../include blzpack.c parg.c ../src/brieflz.c ../src/depack.c ../src/depacks.c -D_N_HIGH_PRIORITY
```
    
Bottomlines:
- Conor's nifty tool LZSSE reigns (hate to miss LZTurbo (wonderful BWTSatan as well) and Oodle) supreme, fully (Read-Decompress-Write) decompresses at 1,576,788,480 bytes / 0.8 seconds = 1879 MiB/s and 2,037,880,832 bytes / 0.9 seconds = 2159 MiB/s, being a single-threaded bonbon; should LZSSE be threaded, it would scream insanely;
- Excellent work, Gribok, thank you for your superuseful tool – a multi-threaded bonboniera;
- Matt Mahoney’s ZPAQ is the OG, never outside the ... MIX;
- RARwise, Roshal brothers never disappoint, as far as I see, they aim at speed mostly, very fast all around;
- Satanichi (being the latest Nakamichi) fares well, being from 8.8/2.7=3.2x to 9.9/2.9=3.3x faster than 7zzs_23.01's gz, however, Igor Pavlov’s implementation is inferior to Eric Biggers’ libdeflate, which in some cases is even faster than my toy, couldn’t include it;
- And, regarding the impact of Last-Level-Cache, surely BSC will scream even louder with those huge 3D cache CPUs, those 1+ billion LLC misses are a drag;
    
Obviously, WhiskeyLake rocks, being only 25W.
    
Oh, wanted to include the Fabrice Bellard's superthrasher NNCP... somenight.
    
2024-Feb-05,
Kaze (sanmayce@sanmayce.com)    
    
P.S.
    
```
// Satanichi's 'Decompress' function disassembly (clang version 17.0.4), 174 (with one 'callq') instructions mainloop:
	.globl	Decompress                      
	.p2align	4, 0x90
	.type	Decompress,@function
Decompress:                             
	.cfi_startproc
	pushq	%r15
	.cfi_def_cfa_offset 16
	pushq	%r14
	.cfi_def_cfa_offset 24
	pushq	%r12
	.cfi_def_cfa_offset 32
	pushq	%rbx
	.cfi_def_cfa_offset 40
	pushq	%rax
	.cfi_def_cfa_offset 48
	.cfi_offset %rbx, -40
	.cfi_offset %r12, -32
	.cfi_offset %r14, -24
	.cfi_offset %r15, -16
	movq	%rdi, %rbx
	movabsq	$2025524839297716754, %rax      
	movq	%rax, (%rsp)
	movq	%rdi, %r15
	testq	%rdx, %rdx
	jle	.LBB43_22
	movq	%rdx, %r14
	movq	%rsi, %r12
	addq	%rsi, %r14
	movq	%rbx, %r15
	jmp	.LBB43_2
	.p2align	4, 0x90
.LBB43_17:                              
	movl	%eax, %edx
	andl	$3, %edx
	je	.LBB43_18
	leal	(,%rdx,8), %ecx
	movl	%eax, %esi
	andl	$12, %esi
	movl	$4294967295, %edi               
	shrq	%cl, %rdi
	movl	$16, %ecx
	subl	%esi, %ecx
	andl	%edi, %eax
	shrl	$4, %eax
	movq	%r15, %rsi
	subq	%rax, %rsi
	vmovups	(%rsi), %ymm0
	vmovups	%ymm0, (%r15)
	addq	%rcx, %r15
	movl	$4, %eax
	subl	%edx, %eax
	addq	%rax, %r12
.LBB43_21:                              
	cmpq	%r14, %r12
	jae	.LBB43_22
.LBB43_2:                               
	movl	(%r12), %eax
	movl	%eax, %ecx
	andl	$15, %ecx
	cmpl	$12, %ecx
	je	.LBB43_15
	cmpl	$3, %ecx
	jne	.LBB43_17
	movl	%eax, %ecx
	shrl	$4, %ecx
	andl	$15, %ecx
	cmpl	$7, %ecx
	ja	.LBB43_9
	testl	%ecx, %ecx
	je	.LBB43_6
	movq	1(%r12), %rax
	movq	%rax, (%r15)
	movl	%ecx, %eax
	addq	%rax, %r15
	addq	%rax, %r12
	incq	%r12
	jmp	.LBB43_21
	.p2align	4, 0x90
.LBB43_15:                              
	movl	%eax, %ecx
	shrl	$3, %ecx
	andl	$8, %ecx
	movl	%ecx, %esi
	shrl	$3, %esi
	movl	$16777215, %edx                 
	shrq	%cl, %rdx
	subq	%rsi, %r12
	addq	$2, %r12
	andl	%eax, %edx
	shrl	$7, %edx
	negq	%rdx
	shrl	%eax
	andl	$30, %eax
	movq	%r12, %rcx
	.p2align	4, 0x90
.LBB43_16:                              
	vmovups	(%r15,%rdx), %ymm0
	vmovups	%ymm0, (%r15)
	addq	%rax, %r15
	leaq	1(%rcx), %r12
	cmpb	$-125, 1(%rcx)
	movq	%r12, %rcx
	je	.LBB43_16
	jmp	.LBB43_21
.LBB43_9:                               
	cmpl	$10, %ecx
	jb	.LBB43_13
	shrl	$8, %eax
	movq	%r15, %rsi
	subq	%rax, %rsi
	cmpl	$15, %ecx
	jne	.LBB43_11
	movl	$512, %edx                      
	movq	%r15, %rdi
	vzeroupper
	callq	memcpy@PLT
	addq	$512, %r15                      
	addq	$4, %r12
	jmp	.LBB43_21
.LBB43_18:                              
	leal	(%rax,%rax), %ecx
	andl	$24, %ecx
	movl	%ecx, %edx
	shrl	$3, %edx
	xorl	$3, %edx
	addq	%rdx, %r12
	movl	$4294967295, %edx               
	shrq	%cl, %rdx
	andl	%edx, %eax
	shrl	$4, %eax
	negq	%rax
	movq	%r12, %rcx
	.p2align	4, 0x90
.LBB43_19:                              
	vmovups	(%r15,%rax), %ymm0
	vmovups	%ymm0, (%r15)
	addq	$24, %r15
	leaq	1(%rcx), %r12
	cmpb	$-125, 1(%rcx)
	movq	%r12, %rcx
	je	.LBB43_19
	jmp	.LBB43_21
.LBB43_13:                              
	movq	-2(%r12), %rax
	shrq	$24, %rax
	addq	$5, %r12
	negq	%rax
	leaq	32(%r15), %rcx
	movq	%r12, %rdx
	.p2align	4, 0x90
.LBB43_14:                              
	vmovups	(%r15,%rax), %ymm0
	vmovups	%ymm0, (%r15)
	vmovups	(%rax,%rcx), %ymm0
	vmovups	%ymm0, 32(%r15)
	addq	$64, %r15
	leaq	1(%rdx), %r12
	addq	$64, %rcx
	cmpb	$-125, 1(%rdx)
	movq	%r12, %rdx
	je	.LBB43_14
	jmp	.LBB43_21
.LBB43_6:                               
	shrl	$8, %eax
	movl	%eax, %esi
	andl	$7, %esi
	andl	$3, %eax
	leaq	3(%rax), %rcx
	movq	%r12, %rdx
	subq	%rcx, %rdx
	movq	1(%rdx), %rdx
	shll	$3, %ecx
	orb	$3, %cl
	shrq	%cl, %rdx
	movl	$5, %ecx
	subl	%eax, %ecx
	addq	%r12, %rcx
	negq	%rdx
	movzbl	(%rsp,%rsi), %eax
	.p2align	4, 0x90
.LBB43_7:                               
	vmovups	(%r15,%rdx), %ymm0
	vmovups	%ymm0, (%r15)
	addq	%rax, %r15
	leaq	1(%rcx), %r12
	cmpb	$-125, 1(%rcx)
	movq	%r12, %rcx
	je	.LBB43_7
	jmp	.LBB43_21
.LBB43_11:                              
	vmovups	(%rsi), %ymm0
	vmovups	%ymm0, (%r15)
	addl	%ecx, %ecx
	movl	$36, %eax
	subl	%ecx, %eax
	addq	%rax, %r15
	addq	$4, %r12
	jmp	.LBB43_21
.LBB43_22:
	subq	%rbx, %r15
	movq	%r15, %rax
	addq	$8, %rsp
	.cfi_def_cfa_offset 40
	popq	%rbx
	.cfi_def_cfa_offset 32
	popq	%r12
	.cfi_def_cfa_offset 24
	popq	%r14
	.cfi_def_cfa_offset 16
	popq	%r15
	.cfi_def_cfa_offset 8
	vzeroupper
	retq
.Lfunc_end43:
	.size	Decompress, .Lfunc_end43-Decompress
	.cfi_endproc
```
    
// Satanichi's 'Decompress' function disassembly (GCC version 13.2.1), 193 instructions mainloop:
```
	.type	Decompress, @function
Decompress:
	.cfi_startproc
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	leaq	(%rsi,%rdx), %r9
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	pushq	%r14
	pushq	%r13
	pushq	%r12
	pushq	%rbx
	andq	$-32, %rsp
	.cfi_offset 14, -24
	.cfi_offset 13, -32
	.cfi_offset 12, -40
	.cfi_offset 3, -48
	movq	.LC331(%rip), %rax
	movq	%rax, -8(%rsp)
	cmpq	%r9, %rsi
	jnb	.L2196
	movq	%rdi, %r11
	movq	%rsi, %r8
	movq	%rdi, %rax
	movl	$-1, %r10d
	movl	$16, %r12d
	movl	$4, %ebx
	movl	$3, %r13d
	jmp	.L2195
	.p2align 4,,10
	.p2align 3
.L2183:
	cmpl	$12, %ecx
	je	.L2203
	testb	$3, %dl
	je	.L2204
	leal	0(,%rdx,8), %ecx
	movl	%r10d, %esi
	movq	%rax, %rdi
	andl	$24, %ecx
	shrl	%cl, %esi
	shrl	$3, %ecx
	andl	%edx, %esi
	andl	$12, %edx
	shrl	$4, %esi
	subq	%rsi, %rdi
	movl	%r12d, %esi
	vmovdqu	(%rdi), %ymm5
	subl	%edx, %esi
	movl	%esi, %edx
	vmovdqu	%ymm5, (%rax)
	addq	%rdx, %rax
	movl	%ebx, %edx
	subl	%ecx, %edx
	addq	%rdx, %r8
.L2187:
	cmpq	%r9, %r8
	jnb	.L2205
.L2195:
	movl	(%r8), %edx
	movl	%edx, %ecx
	andl	$15, %ecx
	cmpl	$3, %ecx
	jne	.L2183
	testb	$-128, %dl
	jne	.L2184
	testb	$-16, %dl
	je	.L2206
	movq	1(%r8), %rcx
	shrl	$4, %edx
	andl	$15, %edx
	movq	%rcx, (%rax)
	movl	%edx, %ecx
	addl	$1, %edx
	addq	%rdx, %r8
	addq	%rcx, %rax
	cmpq	%r9, %r8
	jb	.L2195
.L2205:
	subq	%r11, %rax
	vzeroupper
	leaq	-32(%rbp), %rsp
	popq	%rbx
	popq	%r12
	popq	%r13
	popq	%r14
	popq	%rbp
	.cfi_remember_state
	.cfi_def_cfa 7, 8
	ret
	.p2align 4,,10
	.p2align 3
.L2184:
	.cfi_restore_state
	movl	%edx, %ecx
	shrl	$4, %ecx
	andl	$15, %ecx
	cmpl	$9, %ecx
	jbe	.L2188
	shrl	$8, %edx
	movq	%rax, %rsi
	addq	$4, %r8
	subq	%rdx, %rsi
	cmpl	$15, %ecx
	je	.L2189
	movl	$18, %edx
	vmovdqu	(%rsi), %ymm6
	subl	%ecx, %edx
	addl	%edx, %edx
	vmovdqu	%ymm6, (%rax)
	movl	%edx, %edx
	addq	%rdx, %rax
	jmp	.L2187
	.p2align 4,,10
	.p2align 3
.L2204:
	leal	(%rdx,%rdx), %ecx
	movl	%r13d, %esi
	andl	$24, %ecx
	movl	%ecx, %edi
	shrl	$3, %edi
	subl	%edi, %esi
	movl	%r10d, %edi
	shrl	%cl, %edi
	addq	%rsi, %r8
	andl	%edi, %edx
	shrl	$4, %edx
	negq	%rdx
	.p2align 4,,10
	.p2align 3
.L2194:
	vmovdqu	(%rax,%rdx), %ymm0
	addq	$1, %r8
	addq	$24, %rax
	vmovdqu	%ymm0, -24(%rax)
	cmpb	$-125, (%r8)
	je	.L2194
	jmp	.L2187
	.p2align 4,,10
	.p2align 3
.L2203:
	movl	%edx, %ecx
	movl	$2, %esi
	shrl	$3, %ecx
	andl	$8, %ecx
	movl	%ecx, %edi
	shrl	$3, %edi
	subl	%edi, %esi
	addq	%rsi, %r8
	movl	$16777215, %esi
	sarl	%cl, %esi
	movl	%esi, %ecx
	andl	%edx, %ecx
	shrl	%edx
	shrl	$7, %ecx
	andl	$30, %edx
	negq	%rcx
	.p2align 4,,10
	.p2align 3
.L2192:
	vmovdqu	(%rax,%rcx), %ymm1
	addq	$1, %r8
	vmovdqu	%ymm1, (%rax)
	addq	%rdx, %rax
	cmpb	$-125, (%r8)
	je	.L2192
	jmp	.L2187
	.p2align 4,,10
	.p2align 3
.L2206:
	shrl	$8, %edx
	movq	%r8, %rcx
	movl	%edx, %edi
	andl	$7, %edx
	andl	$3, %edi
	leal	3(%rdi), %esi
	subq	%rsi, %rcx
	movq	%rsi, %r14
	movq	1(%rcx), %rsi
	movl	$5, %ecx
	subl	%edi, %ecx
	movzbl	-8(%rsp,%rdx), %edi
	addq	%rcx, %r8
	leal	3(,%r14,8), %ecx
	shrq	%cl, %rsi
	movq	%rsi, %rdx
	negq	%rdx
	.p2align 4,,10
	.p2align 3
.L2186:
	vmovdqu	(%rax,%rdx), %ymm2
	addq	$1, %r8
	vmovdqu	%ymm2, (%rax)
	addq	%rdi, %rax
	cmpb	$-125, (%r8)
	je	.L2186
	jmp	.L2187
	.p2align 4,,10
	.p2align 3
.L2188:
	movq	-2(%r8), %rdx
	addq	$5, %r8
	shrq	$24, %rdx
	negq	%rdx
	.p2align 4,,10
	.p2align 3
.L2190:
	vmovdqu	(%rax,%rdx), %ymm3
	addq	$1, %r8
	addq	$64, %rax
	vmovdqu	%ymm3, -64(%rax)
	vmovdqu	-32(%rax,%rdx), %ymm4
	vmovdqu	%ymm4, -32(%rax)
	cmpb	$-125, (%r8)
	je	.L2190
	jmp	.L2187
	.p2align 4,,10
	.p2align 3
.L2189:
	movq	(%rsi), %rdx
	leaq	8(%rax), %rdi
	andq	$-8, %rdi
	movq	%rdx, (%rax)
	movq	504(%rsi), %rdx
	movq	%rdx, 504(%rax)
	movq	%rax, %rdx
	addq	$512, %rax
	subq	%rdi, %rdx
	leal	512(%rdx), %ecx
	subq	%rdx, %rsi
	shrl	$3, %ecx
	rep movsq
	jmp	.L2187
.L2196:
	leaq	-32(%rbp), %rsp
	xorl	%eax, %eax
	popq	%rbx
	popq	%r12
	popq	%r13
	popq	%r14
	popq	%rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
```
