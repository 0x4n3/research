# Overview

* The function `sub_41B354` calls `WSAStartup`. 
* The function `sub_406E90` creates the network connection.
* The function `sub_403CF0` creates the log messages found in `log.txt`

# Detailed Analysis

## WS2_32!bind

`WS2_32!bind` is called with the following arguments: 

```cpp
int bind(
  [in] SOCKET         s,
       const sockaddr *addr,
  [in] int            namelen
);
```

Where

```
s = 0x304
*addr = 04c3f9f8
namelen = 0x10
```

Dereferencing `04c3f9f8` gives us the following: 

```
0:004> dq 04c3f9f8 L1
04c3f9f8  00000000`9b370002
```

## WS2_32!ioctlsocket

`WS2_32!ioctlsocket` is called with the following arguments: 

```cpp
int ioctlsocket(
  [in]      SOCKET s,
  [in]      long   cmd,
  [in, out] u_long *argp
);
```

Where `cmd` is `8004667Eh`

## WS2_32!setsockopt

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






