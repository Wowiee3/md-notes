# Part 1 | Basic Analysis
## 1.1 | Basic Static Techniques
#### Packed and Obfuscated
- Detect packers with PEiD program

#### Linked Libraries and Functions
##### Static Linking
Common in UNIX and Linux. Static linking makes it so that all code from the library is copied to the executable, making it bigger. 

This also makes it harder to differentiate between statically linked code and the original code. <- Nothing in the PE header indicates that it contains linked code either. 

##### Runtime Linking
Common in malware, esp packed or obfuscated. Executables only connect to libraries when a function is needed, not at program start. (unlike dynamic linking).

This means that when the functions are used, you can't see what functions they are through static analysis.

*Windows has some functions that allow importing libraries not listed in a program's file header.*
- `LoadLibrary` 
- `GetProcAddress`
- `LdrGetProcAddress`
- `LdrLoadDll`

##### Dynamic Linking (most common)
Most common linking method. When the program is loaded, the host OS searches for the necessary libraries. 

When the PE calls a function from a library, the function executes within the library.
In this case, the PE header will store information about the loaded libraries.

##### Commonly used DLLs and Naming Conventions

| DLL                        | Description                                                                                                                                                                                                                                                                               |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Kernel32.dll               | Core functions (access and manipulation of memory, files, hardware)                                                                                                                                                                                                                       |
| Advapi32.dll               | Advanced core Windows components (e.g. service manager and registry)                                                                                                                                                                                                                      |
| User32.dll                 | UI components and responding to user actions                                                                                                                                                                                                                                              |
| Gdi32.dll                  | Display and manipulating graphics                                                                                                                                                                                                                                                         |
| Ntdll.dll                  | Windows kernel interface. Always imported indirectly by Kernel32.dll.<br>*If an executable imports this file, it means the author intended to use functionality not normally available to Windows programs. Some tasks for hiding functionality or manipulating processes will use this.* |
| WSock21.dll and Ws2_32.dll | Networking socket DLL.                                                                                                                                                                                                                                                                    |
| Wininet.dll                | High level networking functions (FTP, HTTP, NTP)                                                                                                                                                                                                                                          |

**Function Naming Conventions**
- Windows Functions that have been updated significantly have Ex appended to their names (e.g. CreateWindowEx)
- Functions that take strings as parameters append a A or W at the end of their names (e.g. CreateDirectoryW)
##### Exported Functions
DLLs and EXEs can export functions to be used by other programs. The PE file contains information about which functions a file exports.

In Microsoft's documentation, in order to run a program as a service you should define a `ServiceMain` function. Meaning, if a malware has an exported function called `ServiceMain` it means that it runs as part of a service.

Export information can also be viewed in **Dependency Walker**

## Links to Tools and Tips
- [[(TOOL) Dependency Walker]]
- [[(TOOL) PEview]]
- [[(TOOL) Resource Hacker]]

## Lab
- [[(LAB) Part 1]]
