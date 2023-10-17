# Overview

* The function `sub_41B354` calls `WSAStartup`. 
* The function `sub_406E90` creates the network connection.

`WS2_32!ioctlsocket` called with the following arguments: 

```cpp
int ioctlsocket(
  [in]      SOCKET s,
  [in]      long   cmd,
  [in, out] u_long *argp
);
```

Where `cmd` is `8004667Eh`

