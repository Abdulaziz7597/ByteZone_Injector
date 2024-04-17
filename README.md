## What is ByteZone Injector?

ByteZone Injector is a user-mode injector written in C# .NET 8.0, designed for educational purposes in computer science. It allows you to delve into injection techniques and gain a deeper understanding of how applications interact with the system, Also the injector offers **various injection techniques** to experiment with, providing a hands-on learning experience for understanding application loading and execution mechanisms.

> [!TIP]
> Driver update v1.0.1 
<details>
	<summary>Changelog</summary>

Greetings
We are excited to introduce the latest update to our driver, featuring a creative enhancement that adds a touch of magic to the injection process.

##### Patch Highlights:

 - Process Name Injection: Say farewell to anonymity! Now, in addition to injecting payloads, our driver can capture and remember the names of the brave processes that have joined the injection dance.

 - Increased Intrigue: Witness the mesmerizing display of injected process names as they twirl and whirl through the driver's memory.

 - Injection Statistics: Receive insightful statistics on the total injection attempts, successful injections, and marvel at the dazzling array of injected process names.

 - Magic Moments: Delve into the world of kernel mode injection like never before, where processes gain names and binaries come to life with each injection.

-  Improved User Experience: Feel the thrill of customization as you control the injection process and leave a lasting mark on the driver's memory.

### Technical Improvements:

1. Enhanced IOCTL handling to extract and store process names during injection.
2. Added functionality to track and display injected process names within the driver.
3. Updated driver logic to seamlessly integrate process name injection for a magical experience.

### Bug Fixes:

- Resolved minor glitches and optimized performance for a smoother injection process.
Thank you for being part of our journey towards a more immersive and enchanting injection experience. What wonders will you uncover with our enhanced driver? Let the magic unfold!
Stay tuned for more updates and keep injecting with flair!

Yours magically,
Byte Zone Team 
</details>

### Project Structure

ByteZone Injector consists of two main files:

#### `classes.cs`
This file contains the core logic for injection methods and GUI interaction. It defines two main classes:
- **DllImports:** This class encapsulates all Windows API imports and flags used throughout the injector, providing a central location for these definitions.
- **InjectionMethods:** This class houses the actual injection techniques. Currently, it supports Native Injection, but future development plans include additional methods.

#### `Main.cs`
contains the main entry point and user interface logic for the ByteZone Injector application. It provides functionality to select a process and a DLL file for injection, as well as options for choosing the injection method. Key features include:
1. Retrieval of running processes and display in a list.
2. Selection of a DLL file through a file dialog.
3. Native injection of the selected DLL into the chosen process.
4. Handling of user interactions such as button clicks and dropdown selections.
5. Display of process names and their corresponding IDs in a list.
6. Management of the user interface components such as buttons, dropdowns, and labels.


### Current Features

**Native Injection:**
This version of ByteZone Injector currently supports native injection using LoadLibraryA, providing a solid foundation for understanding injection mechanisms.


### Clean and Well-Commented Code
The codebase is well-structured and includes clear comments, making it easy to learn from and understand as you experiment with the current Native Injection feature.
- Sample
```csharp
    /*
     * PROCESS_QUERY_INFORMATION Required to retrieve certain information about a process.
     * A handle that has the
     * PROCESS_QUERY_INFORMATION access right is automatically granted
     */
    public const int PROCESS_QUERY_INFORMATION = 0x0400;
    public const int PROCESS_VM_WRITE = 0x0020; // Required to write to memory in a process using WriteProcessMemory.
    public const int PROCESS_VM_READ = 0x0010; // Required to read memory in a process using ReadProcessMemory.
    public const int PROCESS_VM_OPERATION = 0x0008; // Required to perform an operation on the address space of a process 

    /* Memory allocation */
    public const uint MEM_COMMIT = 0x00001000; // MEM_COMMIT is a Windows constant used with Windows API calls
    public const uint MEM_RESERVE = 0x00002000; // MEM_RESERVE is a Windows constant used with Windows API calls
    public const uint PAGE_READWRITE = 4; // PAGE_READWRITE is a Windows constant used with Windows API calls

    /* Generic Access Rights Win32 */
    // https://learn.microsoft.com/en-us/windows/win32/secauthz/generic-access-rights
    public const uint GENERIC_READ = 0x80000000; // Read access
    public const uint GENERIC_WRITE = 0x40000000; // Write access
    
    /* 
     * Opens a file or device, only if it exists.
     * If the specified file or device does not exist, the
     * function fails and the last-error code is set to ERROR_FILE_NOT_FOUND (2).
     * https://learn.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-createfilea
     */
    public const int OPEN_EXISTING = 3; 
```
### TwDrv 
The ByteZone Injector driver is a foundational component designed to support future kernel-level injection techniques. Developed using Visual Studio 2019 with C++ version 17 and utilizing the Windows Driver Framework (WDF) 1903 SDK, this driver lays the groundwork for upcoming features such as kernel injection using IOCTL. With its clean structure and essential functionalities, the driver provides a solid starting point for further development and experimentation with injection mechanisms.
### Installation

**ByteZone injector**
1. Download and install the .NET 8.0 SDK from [here](https://dotnet.microsoft.com/en-us/download/dotnet/8.0).
2. Download ByteZone injector from [here](https://github.com/byte-zone/ByteZone_Injector/archive/refs/heads/main.zip).
3. Build and run the project.

**TwDrv**
1. Download and install visual studio 2019 from [here](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=Community&rel=16)
2. Download and install WDK 1903 from [here](https://go.microsoft.com/fwlink/?linkid=2085767)
3. Download and install Windows SDK (1903) [here](https://go.microsoft.com/fwlink/?linkid=2083338)
4. Open TwDrv Project, Build Settings set to x64 Release.
- TwDrv Sample
```cpp
#define IOCTL_INJECT_CODE CTL_CODE(FILE_DEVICE_UNKNOWN, 0x800, METHOD_BUFFERED, FILE_SPECIAL_ACCESS)
NTSTATUS IoControlHandler(PDEVICE_OBJECT DeviceObject, PIRP Irp)
{
	UNREFERENCED_PARAMETER(DeviceObject);
	NTSTATUS status = STATUS_SUCCESS;
	PIO_STACK_LOCATION stack = IoGetCurrentIrpStackLocation(Irp);
	ULONG controlCode = stack->Parameters.DeviceIoControl.IoControlCode;

	switch (controlCode)
	{
	case IOCTL_INJECT_CODE:
			break;
		default:
			status = STATUS_INVALID_DEVICE_REQUEST;
			break;
	}

	Irp->IoStatus.Status = status;
	IoCompleteRequest(Irp, IO_NO_INCREMENT);
	return status;
}
```

**Dll Example**
- Dll Example is just a simple example for printing and testing using ```MessageBoxA``` Function
### Roadmap (Features in Development)
Development is ongoing to integrate additional injection techniques and improve user experience, including
- [ ] Better Gui
- [ ] Settings tab
- [ ] Kernel injection using IOCTL with a custom driver (driver under development, you can download the driver base)
- [ ] LdrpLoadDll (planned for future release)
- [ ] Manual mapping (planned for future release)
- [ ] Thread hijacking (planned for future release)

> [!NOTE]
> Kernel injection, manual mapping, and thread hijacking features are not yet available in this version.
