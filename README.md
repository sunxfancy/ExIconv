# ExIconv

[中文文档](./README-cn.md)

It's an encoding auto-detection library in C

For cross-platform and C++ version, please using the estring which support Conan package manager:

<https://github.com/sunxfancy/estring>

It combines libiconv and libcharsetdetect to auto-detect encoding and directly transform to UTF-32.

Example: 

```C
#include "stdio.h"
#include "exiconv.h"
#define BUFFER_SIZE 4096

int main(int argc, const char * argv[]) {
    FILE* f = fopen(argv[1], "r");
    size_t outsize;
    echar_t* str = autoreadfile(f, &outsize);
    printf("echar_len = %d\n", estrlen(str));
    char* utf8_str = conv2utf8(str, &outsize);
    printf("utf8:%s\n", utf8_str);
    fclose(f);
    return 0;
}
```

Functions:
```C

typedef uint32_t echar_t;
struct FILE;


/**
 * @brief translate UTF-32 string to utf8 for output
 * 
 * @param data UTF-32 array
 * @param outsize length of the string after transform
 * 
 * @return It will auto malloc space for the new string, please free it after use
 */
extern char* 
conv2utf8 (const echar_t* data, size_t* outsize);


/**
 * @brief translate utf8 to UTF-32 array
 * 
 * @param data utf8 string array 
 * @param outsize length of the string after transform
 * 
 * @return It will auto malloc space for the new string, please free it after use
 */
extern echar_t*
utf8conv2echar (const char* data, size_t* outsize);




/**
 * @brief translate UTF-32 string to a special encode
 * 
 * @param data UTF-32 array
 * @param outsize length of the string after transform
 * 
 * @return It will auto malloc space for the new string, please free it after use
 */
extern char* 
echar2code (const echar_t* data, size_t* outsize, const char* encode);


/**
 * @brief translate a special encode to UTF-32 string 
 * 
 * @param data string array
 * @param outsize length of the string after transform
 * 
 * @return It will auto malloc space for the new string, please free it after use
 */
extern echar_t*
code2echar (const char* data, size_t* outsize, const char* encode);




/**
 * @brief Read the file and auto-detect encoding, then translate into UTF-32 array
 * 
 * @param f FILE pointer (binary read mode)
 * @param outsize length of the string after transform
 * 
 * @return It will auto malloc space for the new string, please free it after use
 */
extern echar_t*
autoreadfile (FILE* f, size_t* outsize);


/**
 * @brief auto-detect encoding, then translate into UTF-32 array
 * 
 * @param data string data
 * @param outsize length of the string after transform
 * 
 * @return It will auto malloc space for the new string, please free it after use
 */
extern echar_t*
autoreadchar (const char* data, size_t* outsize);


/**
 * @brief detect encoding
 * 
 * @param data string data end with '\0'
 * @return name of the encoding, NULL if it's failure
 */
extern const char*
encodedetect (const char* data);

/**
 * @brief detect encoding for the file
 * 
 * @param data FILE pointer (binary read mode)
 * @return name of the encoding, NULL if it's failure
 */
extern const char*
fileencodedetect (FILE* f);


/**
 * @brief get the length of string
 * 
 * @param str string data end with '\0'
 * @return length
 */
extern size_t
estrlen (const echar_t* str);


/**
 * Free the data
 */
#define FreeStr(p) free_str((void**)&p)

extern void
free_str(void** p);

```
