# Catalog

* [localtime](#localtime)
* [va_list, va_start, vsnprintf, va_end](#va_list-va_start-vsnprintf-va_end)
* [Volatile變數](#Volatile變數)
* [define](#define)
* [sys/stat.h](#sysstath)
* [folder content](#folder-content)
* [create folder](#create-folder)
* [blocking/non-blocking](#blockingnon-blocking)

***

## localtime

```cpp
QString getLocalDate()
{
    time_t now;

    time(&now);
    struct tm *localnow = localtime(&now);

    return QString::asprintf("%04d-%02d-%02d", 
                             localnow->tm_year + 1900, 
                             localnow->tm_mon + 1, 
                             localnow->tm_mday);
}
```
[Top](#Catalog)

***

## va_list, va_start, vsnprintf, va_end

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stdarg.h>
#include <string.h>

void PrintMsg(const char *format, ...){

    va_list args;
    char *buf;
    int length;

    va_start(args, format);
    length = vsnprintf(NULL, 0, format, args);
    va_end(args);

    va_start(args, format);
    buf=(char *)malloc(length + 1);
    vsnprintf(buf, length, format, args);
    va_end(args);
    
    printf("[xxx]: %s", buf);
    free(buf);
}
```
[Top](#Catalog)

***

## Volatile變數

在程式設計中，尤其是在C語言、C++、C#和Java語言中，使用volatile關鍵字聲明的變數或物件通常具有與最佳化、多執行緒相關的特殊屬性<br>
[Wiki Volatile變數](https://zh.wikipedia.org/wiki/Volatile%E5%8F%98%E9%87%8F)

[Top](#Catalog) 

***

## define

```cpp
#define PRINT_MSG(msg, args...)         do {printf(msg, ##args);} while(0)
#define PRINT_BUFFER(str, buf, len)     do { \
                                            unsigned long i; \
                                            unsigned char *ptr; \
                                            ptr = (unsigned char *)buf; \
                                            printf("[ %s ] len %ld\n", str, (unsigned long)len); \
                                            for(i = 0 ; i < len ; i++) printf("%02X", ptr[i]); printf("\r\n"); \
                                        } while(0)
```
[Top](#Catalog) 

***

## sys/stat.h

```cpp
#include <sys/stat.h>

char IsFileExist(char *filename) 
{
    struct stat buf;
    int ret;
    
    ret = stat(filename, &buf);
    if (ret) 
        return (0);	
    
    return (1);
}

unsigned long GetFileSize(char *filename) 
{
    struct stat buf;

    if (stat(filename, &buf) == -1)
        return 0;
        
    return buf.st_size;
}

long GetFileSize(char *filename)
{
    FILE *fp;
    long len;

    fp = fopen(filename, "rb");
    if(fp == NULL)
        return 0;

    fseek(fp, 0, SEEK_END);
    len = ftell(fp);
    fclose(fp);

    return len;
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
#include <dirent.h>

bool FolderContent(char *path)
{
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
            if(dir->d_type == DT_DIR)
                printf("folder: %s\n", dir->d_name);
            else
                printf("filename: %s\n", dir->d_name);
        }
        closedir(d);
        break;
    }
    return 0;
}
```
[Top](#Catalog) 

***

## create folder

```cpp
#include <dirent.h>

bool MakeDir(char *pFilePath)
{
    char DirName[256];
    strcpy(DirName, pFilePath);
    int i, len = strlen(DirName);

    if(strlen(pFilePath) == 0 || strcmp(pFilePath, "./") == 0)
        return true;

    if(pFilePath[0] != '.' || pFilePath[1] != '/')
        return false;


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
                    return false;
                }
            }
            DirName[i] = '/';
        }
    }

    return true;
}

```
[Top](#Catalog) 

***

## blocking/non-blocking
```cpp
    #include <fcntl.h>

    /* set socket as non-blocking */
    int flags = fcntl(socket, F_GETFL, 0); 
    fcntl(socket, F_SETFL, flags | O_NONBLOCK);

    /* set socket as blocking */
    int  flags  =  fcntl(socket,  F_GETFL,  0);
    fcntl(socket,  F_SETFL,  flags  |  ~O_NONBLOCK);
```
[Top](#Catalog) 
