# Catalog
* [sys/stat.h](#sysstath)
* [folder content](#folder-content)
* [create folder](#create-folder)
* [blocking/non-blocking](#blockingnon-blocking)
***

## sys/stat.h

```cpp
#include <sys/stat.h>

char IsFileExist(char *filename) 
{
    struct stat buf;
    int ret;

    errno = 0;
    ret = stat(filename, &buf);
    if (ret) 
        return (0);	
    
    return (1);
}

unsigned long GetFileSize(char *filename) 
{
    struct stat buf;

    if (stat(f, &buf) == -1)
        return 0;
        
    return buf.st_size;
}

char IsFolderExist(char *filename) 
{
    struct stat buf;
    int ret;
    
    ret = stat(filename, &buf);
    if(ret || !S_ISDIR(buf.st_mode))
        return (0);
        
    return(1);
}
```
[Top](#Catalog) 
***

## folder content

```cpp
    DIR *d;
    struct dirent *dir;
    d = opendir(path);
    if(d == NULL)
        return 1;
        
    while(1)
    {
        while((dir = readdir(d)) != NULL)
        {
            if(strcmp(dir->d_name, ".") == 0 || strcmp(dir->d_name, "..") == 0)
            {
                continue;
            }
            printf("filename: %s\n", dir->d_name);
        }
        closedir(d);
        break;
    }
    return 0;
```
[Top](#Catalog) 
***

## create folder

```cpp
#include <dirent.h>

BOOL MakeDir(BYTE *pFilePath)
{
    char DirName[256];
    strcpy(DirName, pFilePath);
    int i, len = strlen(DirName);

    if(strlen(pFilePath) == 0 || strcmp(pFilePath, "./") == 0)
        return TRUE;

    if(pFilePath[0] != '.' || pFilePath[1] != '/')
        return FALSE;


    if(DirName[len - 1] != '/')
    {
        strcat(DirName, "/");
    }

    len = strlen(DirName);
    for(i = 2 ; i < len ; i++)
    {
        if(DirName[i] == '/')
        {
            DirName[i]   =   0;
            if(mkdir(DirName, 0755) == -1)
            {
                if(errno != EEXIST)
                {
                    return FALSE;
                }
            }
            DirName[i] = '/';
        }
    }

    return TRUE;
}

```
[Top](#Catalog) 
***

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
