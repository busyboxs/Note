# 日常笔记

## Windows 数据类型

### 重要名词缩写

* BLOB: binary large object
* COM: Component Object Modek
* DFS: Distributed File System
* GUID: globally unique identifier
* UUID: universally unique identifier

### 数据类型

* Interger type
  * `__int8`
    * `signed __int8`: -128 ~ 127
    * `unsigned __int8`: 0 ~ 255
  * `__int16`
    * `signed __int16`: -32768 ~ 32767
    * `unsigned __int16`: 0 ~ 65535
  * `__int32`
    * `signed __int32`: -2147483648 ~ 2147483647
    * `unsigned __int32`: 0 ~ 4294967295
  * `__int64`
    * `signed __int64`: –9223372036854775808 ~ 9223372036854775807
    * `unsigned __int64`: 0 ~ 18446744073709551615
  * `hyper` : can be `signed` or `unsigned`

* `octet` : an 8-bit data item

* `wchar_t` : `typedef unsigned short wchar_t;` -> unicode character

* `BOOL` : `typedef int BOOL, *PBOOL, *LPBOOL;`
* `BOOLEAN`: `typedef BYTE BOOLEAN, *PBOOLEAN;`
* `BSTR` : `typedef WCHAR* BSTR;`
* `BYTE` : `typedef unsigned char BYTE, *PBYTE, *LPBYTE`
* `CHAR` : `typedef char CHAR, *PCHAR`
* `typedef` : `typedef double DOUBLE`
* `DWORD` : `typedef unsigned long DWORD, *PDWORD, *LPWORD`
* `DWORD_PTR` : `typedef ULONG_PTR DWORD_PTR`
* `DWORD32` : `typedef unsigned int DWORD32`
* `DWORD64` : `typedef unsigned __int64 DWORD64, *PDWORD64`
* `DWORDLONG` : `typedef ULONGLONG DWORDLONG, *PDWORDLONG`
* `error_status_t` : `typedef unsigned long error_status_t`
* `FLOAT` : `typedef float FLOAT`
* `HANDLE` : `typedef void* HANDLE`
* `HCALL` : `typedef DWORD HCALL`
* `HRESULT` : `typedef LONG HRESULT`
* `INT` : `typedef int INT`
* `INT8` : `typedef signed char INT8`
* `INT16` : `typedef signed short INT16`
* `INT32` : `typedef signed int INT32`
* `INT64` : `typedef signed __int64 INT64`
* `LDAP_UDP_HANDLE` : `typedef void* LDAP_UDP_HANDLE;`
* `LMSSTR` : `typedef const wchar_t* LMSSTR`
* `LMSTR` : `typedef WCHAR* LMSTR`
* `LONG` : `typedef long LONG, *PLONG, *LOLONG`
* `LONGLONG` : `typdef signed __int64 LONGLONG`
* `LONG_PTR` : `typedef __int3264 LONG_PTR`
* `LONG32` : `typedef signed int LONG32`
* `LONG64` : `typedef signed __int64 LONG64, *PLONG64`
* `LPCSTR` : `typedef const char* LPCSTR`
* `LPCVOID` : `typedef const void* LPCVOID`
* `LPCWSTR` : `typedef const wchar_t* LPCWSTR`
* `LPSTR` : `typedef char* PSTR, *LPSTR`
* `LPWSTR` : `typedef wchar_t* LPWSTR, *PWSTR`
* `NET_API_STATUS` : `typedef DWORD NET_API_STATUS`
* `NTSTATUS` : `typedef long NTSTATUS`
* `PCONTEXT_HANDLE` : `typedef [context_handle] void* PCONTEXT_HANDLE`
* `QWORD` : `typedef unsigned __int64 QWORD`
* `RPC_BINDING_HANDLE` : `typedef void* RPC_BINDING_HANDLE`
* `SHORT` : ` typedef short SHORT`
* `SIZE_T` : `typedef ULONG_PTR SIZE_T`
* `STRING` : `typedef UCHAR* STRING`
* `UCHAR` : `typedef unsigned char UCHAR, *PUCHAR`
* `UINT` : `typedef unsigned int UINT`
* `UINT8` : `typedef unsigned char UINT8`
* `UINT16` : `typedef unsigned short UINT16`
* `UINT32` : `typedef unsigned int UINT32`
* `UINT64` : `typedef unsigned __int64 UINT64`
* `ULONG` : `typedef unsigned long ULONG, *PULONG`
* `ULONG_PTR` : `typedef unsigned __int3264 ULONG_PTR`
* `ULONG32` : `typedef unsigned int ULONG32`
* `ULONG64` : `typedef unsigned __int64 ULONG64`
* `ULONGLONG` : `typedef unsigned __int64 ULONGLONG`
* `UNICODE` : `typedef wchar_t UNICODE`
* `UNC` : `typedef STRING UNC`
* `USHORT` : `typedef unsigned short USHORT`
* `VOID` : `typedef void VOID, *PVOID, *LPVOID`
* `WCHAR` : `typedef wchar_t WCHAR, *PWCHAR`
* `WORD` : `typedef unsigned short WORD, *PWORD, *LPWORD`

### 数据结构

* `EVENT_DESCRIPTOR`
  
  ```cpp
  typedef struct _EVENT_DESCRIPTOR {
    USHORT Id;
    UCHAR Version;
    UCHAR Channel;
    UCHAR Level;
    UCHAR Opcode;
    USHORT Task;
    ULONGLONG Keyword;
  } EVENT_DESCRIPTOR, *PEVENT_DESCRIPTOR *PCEVENT_DESCRIPTOR;
  ```

* `EVENT_HEADER`

  ```cpp
  typedef struct _EVENT_HEADER {
    USHORT Size;
    USHORT HeaderType;
    USHORT Flags;
    USHORT EventProperty;
    ULONG ThreadId;
    ULONG ProcessId;
    LARGE_INTEGER TimeStamp;
    GUID ProviderId;
    EVENT_DESCRIPTOR EventDescriptor;
    union {
      struct {
        ULONG KernelTime;
        ULONG UserTime;
      };
      ULONG64 ProcessorTime;
    };
    GUID ActivityId;
  } EVENT_HEADER,
  *PEVENT_HEADER;
  ```

* `FILETIME`

  ```cpp
  typedef struct _FILETIME {
    DWORD dwLowDateTime;
    DWORD dwHighDateTime;
  } FILETIME, *PFILETIME, *LPFILETIME;
  ```

* `GUID--RPC IDL representation`

  ```cpp
  typedef struct _GUID {
    unsigned long Data1;
    unsigned short Data2;
    unsigned short Data3;
    byte Data4[8];
  } GUID, UUID, *PGUID;
  ```

* `LARGE_INTEGER`

  ```cpp
  typedef struct _LARGE_INTEGER {
    signed __int64 QuadPart;
  } LARGE_INTEGER, *PLARGE_INTEGER;
  ```

* `LCID`

  ```cpp
  typedef DWORD LCID;
  ```

* `LUID` : 64 bit

  ```cpp
  typedef struct _LUID {
    DWORD LowPart;
    LONG HighPart;
  } LUID, *PLUID;
  ```

* `MULTI_SZ`

  ```cpp
  typedef struct _MULTI_SZ {
    wchar_t* Value;
    DWORD nChar;
  } MULTI_SZ;
  ```

* `OBJECT_TYPE_LIST`

  ```cpp
  typedef struct _OBJECT_TYPE_LIST {
    WORD Level;
    ACCESS_MASK Remaining;
    GUID* ObjectType;
  } OBJECT_TYPE_LIST, *POBJECT_TYPE_LIST;
  ```

* `RPC_UNICODE_STRING`

  ```cpp
  typedef struct _RPC_UNICODE_STRING {
    unsigned short Length;
    unsigned short MaximumLength;
    [size_is(MaximumLength/2), length_is(Length/2)] 
      WCHAR* Buffer;
  } RPC_UNICODE_STRING, *PRPC_UNICODE_STRING;
  ```

* `SERVER_INFO_100`

  ```cpp
  typedef struct _SERVER_INFO_100 {
    DWORD sv100_platform_id;
    [string] wchar_t* sv100_name;
  } SERVER_INFO_100, *PSERVER_INFO_100, *LPSERVER_INFO_100;
  ```

* `SERVER_INFO_101`

  ```cpp
  typedef struct _SERVER_INFO_101 {
    DWORD sv101_platform_id;
    [string] wchar_t* sv101_name;
    DWORD sv101_version_major;
    DWORD sv101_version_minor;
    DWORD sv101_version_type;
    [string] wchar_t* sv101_comment;
  } SERVER_INFO_101, *PSERVER_INFO_101, *LPSERVER_INFO_101;
  ```

* `SYSTEMTIME`

  ```cpp
  typedef struct _SYSTEMTIME {
    WORD wYear;
    WORD wMonth;
    WORD wDayOfWeek;
    WORD wDay;
    WORD wHour;
    WORD wMinute;
    WORD wSecond;
    WORD wMilliseconds;
  } SYSTEMTIME, *PSYSTEMTIME;
  ```

* `UINT128`

  ```cpp
  typedef struct _UINT128 {
    UINT64 lower;
    UINT64 upper;
  } UINT128, *PUINT128;
  ```

* `ULARGE_INTEGER`

  ```cpp
  typedef struct _ULARGE_INTEGER {
    unsigned __int64 QuadPart;
  } ULARGE_INTEGER, *PULARGE_INTEGER;
  ```
