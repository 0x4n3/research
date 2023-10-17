# Overview

* The function `sub_41B354` calls `WSAStartup`. 
* The function `sub_406E90` creates the network connection.
* The function `sub_403CF0` creates the log messages found in `log.txt`

# Detailed Analysis

`WS2_32!ioctlsocket` called with the following arguments: 

```cpp
int ioctlsocket(
  [in]      SOCKET s,
  [in]      long   cmd,
  [in, out] u_long *argp
);
```

Where `cmd` is `8004667Eh`

Then, `WS2_32!setsockopt` is called: 

```cpp
int setsockopt(
  [in] SOCKET     s,
  [in] int        level,
  [in] int        optname,
  [in] const char *optval,
  [in] int        optlen
);
```

Where

```
s = 0000032c
level = 0FFFFh
optname = 1006h
*optval = 045ef478
optlet = 4
```

Dumping the pointer at `045ef478` reveals the following: 

```
0:003> dqs 045ef478 L1
045ef478  fffffffe`00002710
```






