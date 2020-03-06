# Catalog
* [blocking/non-blocking](#blockingnon-blocking)

## blocking/non-blocking
```cpp
    #include <fcntl.h>

    /* 將socket設定為non-blocking */
    int flags = fcntl(socket, F_GETFL, 0); 
    fcntl(socket, F_SETFL, flags | O_NONBLOCK);

    /* 將socket設定為blocking */
    int  flags  =  fcntl(socket,  F_GETFL,  0);
    fcntl(socket,  F_SETFL,  flags  |  ~O_NONBLOCK);
```
[Top](#Catalog) 
