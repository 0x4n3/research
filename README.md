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

Further research suggests that this is defined as "FIONBIO" (Source: https://learn.microsoft.com/en-us/windows/win32/winsock/winsock-ioctls)

> FIONBIO
> Enable or disable non-blocking mode on socket s. The lpvInBuffer parameter points at an unsigned long (QoS), which is nonzero if non-blocking mode is to be enabled and zero if it is to be disabled. When a socket is created, it operates in blocking mode (that is, non-blocking mode is disabled). This is consistent with BSD sockets.
> The WSAAsyncSelect or WSAEventSelect routine automatically sets a socket to non-blocking mode. If WSAAsyncSelect or WSAEventSelect has been issued on a socket, then any attempt to use WSAIoctl to set the socket back to blocking mode will fail with WSAEINVAL. To set the socket back to blocking mode, an application must first disable WSAAsyncSelect by calling WSAAsyncSelect with the lEvent parameter equal to zero, or disable WSAEventSelect by calling WSAEventSelect with the lNetworkEvents parameter equal to zero.


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






