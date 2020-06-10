    Copyright(c) 2019-
    Author: Chaitanya Tejaswi (github.com/CRTejaswi)    License: GPL v3.0+


# PowerShell
> Personal notes.

# Resources

- TechSnips [[1]](https://www.youtube.com/playlist?list=PLviQuRV5ySnzSNQ4rui7_dJ26kgM22BKY) [[2]](https://www.youtube.com/playlist?list=PLviQuRV5ySnzt8YpB14SfHeJm8Rh0oO1A)
- [Jeff Hicks](https://jdhitsolutions.com/blog/)
- Warren Frame [[PS recipies]](https://github.com/RamblingCookieMonster/PowerShell) [[PSSQLite]](https://github.com/RamblingCookieMonster/PSSQLite)
- [Prateek Singh](https://github.com/PrateekKumarSingh/PowerShell-Interview)
- [powershell.org](https://powershell.org/articles/)
- [MS DevBlog](https://devblogs.microsoft.com/scripting/)
- [GitHub](https://github.com/PowerShell/PowerShell)
- [Awesome PS](https://github.com/janikvonrotz/awesome-powershell)

# Index

- [General](#general)
- [System Help](#system-help)
- [Exception Handling](#exception-handling)
- [Variables](#variables)
- [Operators](operators)
- [Control Flow](#control-flow)
- [Data Structures](#data-structures)
- [Objects](#objects)
- [Pipelining](#pipelining)
- [Formatting](#formatting)
- [Filtering & Comparisons](#filtering-comparisons)
- [Background Processes (`Job`)](#background-processes-aka-job)
- [Regular Expressions](#regular-expressions)
- [Remote Access](#remote-access)
- [Providers](#providers)
- [Registry](#registry)
- [Structured Data (CSV, JSON, XML)](#formatted-data-csv-json-xml)
- [Databases (SQLite, SQL Server)](#databases)
- [Modules](#modules)
- [Scripting](#scripting)
- [Recipies](#recipies)
- [Tools](#tools)
- [ ] [PowerShell/Python](#powershell-python)

## General

- Update (from PS)
    ```
    iex "& { $(irm https://aka.ms/install-powershell.ps1) } -UseMSI"
    ```

- Clear Command History <br> Press `Ctrl + F7`

- Clear RecycleBin <br> `Clear-RecycleBin`

- Disable Quick Access, Recent Items & File-Explorer History. <br>
    Quick Access (Right-Click) >> Options >> Open File Explorer to: "This PC" >> Uncheck Privacy Options.

- Uninstall built-in apps
    ```
    Get-AppxPackage *3dbuilder* | Remove-AppxPackage
    Get-AppxPackage *getstarted* | Remove-AppxPackage
    Get-AppxPackage *windowsalarms* | Remove-AppxPackage
    Get-AppxPackage *windowscalculator* | Remove-AppxPackage
    Get-AppxPackage *windowscommunicationapps* | Remove-AppxPackage
    Get-AppxPackage *windowsmaps* | Remove-AppxPackage
    Get-AppxPackage *windowsphone* | Remove-AppxPackage
    Get-AppxPackage *windowsstore* | Remove-AppxPackage
    Get-AppxPackage *zunemusic* | Remove-AppxPackage
    Get-AppxPackage *zunevideo* | Remove-AppxPackage
    Get-AppxPackage *bingfinance* | Remove-AppxPackage
    Get-AppxPackage *bingnews* | Remove-AppxPackage
    Get-AppxPackage *bingweather* | Remove-AppxPackage
    Get-AppxPackage *bingsports* | Remove-AppxPackage
    Get-AppxPackage *photos* | Remove-AppxPackage
    Get-AppxPackage *soundrecorder* | Remove-AppxPackage
    Get-AppxPackage *xboxapp* | Remove-AppxPackage
    ```

- Create New File/Folder
    ```
    ni -path 'C:\Users\CRTejaswi\Desktop' -ItemType 'directory' -name 'myTEST'
    ni -path 'C:\Users\CRTejaswi\Desktop\myTEST' -ItemType 'file' -name 'myTEST.md'
    ```

- Get storage directory of binary file.
    ```
    gps firefox | ls
    ```

- Common Aliases
    ```
    man -> Get-Help    (to lookup command syntax)
    gcm -> Get-Command (to lookup command syntax)
    gm  -> Get-Member  (to learn more about an object)
    gal -> Get-Alias

    gcb -> Get-Clipboard (Copy from clipboard)
    scb -> Set-Clipboard (Copy to clipboard)
    gps -> Get-Process

    ft  -> Format-Table
    fl  -> Format-List

    gsv -> Get-Service
    gin -> Get-ComputerInfo
    gtz -> Get-TimeZone

    gwmi -> Get-WmiObject
    gjb  -> Get-Job

    cls -> Clear-Host ('clear shell')
    clc -> Clear-Content
    clv -> Clear-Variable
    cli -> Clear-Item
    clp -> Clear-ItemProperty

    pushd, popd -> Push-Location, Pop-Location
    copy, move  -> Copy-Item, Move-Item
    history     -> Get-History
    ```

- Create new aliases <br>
    Create `aliases.csv`
    ```
    Name,Value
    gotodir,Get-ChildItem
    sel,Select-Object
    clip,Get-Clipboard
    ```
    Import these aliases using:
    ```
    import-csv aliases.csv | new-alias
    ```

<center><b>TODO</b></center>

- [ ] Come up with useful aliases

## System Help

- Update help.
    ```
    update-help -Force
    ```
- Get help.
    ```
    man *service*
    man get-service
    man get-service -detailed
    man get-service -showwindow
    man get-service -examples
    man get-service -full
    man get-service -online
    man *eventlog*
    man about_*
    ```
- Nouns & Verbs <br>
    PS names cmdlets/functions in a `Verb-Noun` manner, to make their names predictable. (eg. `Get-Service`) <br>
    To lookup all PERMITTED verbs (98), use:
    ```
    get-verb
    get-verb | measure
    ```

## Variables

- Single & Double Quotes (`'`,`"`) <br>
    Anything within a single-quote is a literal string. <br>
    Anything within a double-quote is a parsed string. <br>
    ```
    PS> $var='LOCALHOST'
    PS> $var1='Try $var'
    PS> $var2="Try $var"
    PS> $var; $var1; $var2;
    LOCALHOST
    Try $var
    Try LOCALHOST
    ```
    Using double-quotes, you can also assign cmdlet & the object with it's property/method that it generates.
    ```
    PS> $services=get-service
    PS> $var="First Service Is $($services[0].name)"
    First Service Is ABLookupSvc
    ```
- Escape Sequences <br>
    Just as C-syntax uses `back-slash` (__\\__) to escape sequences, PS-syntax used `back-tick`(__\`__) <br>

- Accessing Object Property/Method <br>
    PS3+ allows using Object.Property/Object.Method syntax.
    ```
    PS> $services=Get-Service
    PS> $services.name; $services.gettype()
    ```
    Here, `Name` & `GetType()` are property/method defined on objects returned by `Get-Service`.
    Check using `gsv | gm`.

- Declaring Variable Types
    Use `[type]$var` notation.
    ```
    PS> [int]$var = Read-Host "Enter A Number"
    Enter A Number: 100
    PS> $var | gm
        TypeName: System.Int32
    ...
    ```

<center>

| Type | Description |
| :-- | :--: |
| `[int]`,`[single]`,`[double]` | integer, single/double precision floating values |
| `[char]`,`[string]` | character, string of characters |
| `[xml]` | XML document |
| `[adsi]` | ActiveDirectory Service Interface (ADSI) query |

</center>

## Control Flow

__if-else__ <br>
Conditional execution. <br>
Execution is based on certain well-defined conditions. <br>

- Syntax
    ```
    if (<condition>){
        ...
    } elseif (<condition>){
        ...
    } else {
        ...
    }
    ```

- [x] Do something if today is Monday, post 12PM.
    ```
    $now = get-date
    if ($now.dayofweek -eq 'Monday' -and $now.hour -gt 12){
        ...
    }
    ```

__switch__ <br>
Conditional-Value execution. <br>
Execution is based on certain properties/values of `<object>`. <br>

- Syntax
    ```
    switch (<object>){
        <case>  {...}
        <case>  {...}
        default {...}
    }
    ```

- [x] Check if a string contains '1' or 'a'
    ```
    $x = '123abc'
    switch -wildcard ($x){
        "*1*" {"Contains 1"}
        "*a*" {"Contains a"}
        default {"No matches found"}
    }
    ```

__for__ <br>
Iterative Conditional execution. <br>

- Syntax
    ```
    for (<start>; <condition>; <action>){
        ...
    }
    ```

- [x] Print the number of characters in a string iteratively (\*actually prints indices)
    ```
    $x = '123abc'
    for ($i=0; $i -lt $x.Length; $i++){
        Write-Host $i -foreground black -background yellow
    }
    ```

__forEach__ <br>
Iterative execution, for each item in items. <br>

- Syntax
    ```
    forEach ($item in $items){
        ...
    }
    ```

- [x] Print the squares of 1-10
    ```
    $numbers = 1..10
    forEach ($number in $numbers){
        Write-Host ($number*$number)
    }
    ```

__do-while__ <br>
Execute until condition fails. <br>

- Syntax <br>
    [1]: Execute only when condition starts to hold $True. (May not be executed at all.)
    ```
    while (<condition>){
        ...
    }
    ```
    [2]: Execute atleast once.
    ```
    do {
        ...
    } while (<condition>)
    ```

- [x] ...
    ```
    ...
    ```

__break__ <br>
Abort execution abruptly. <br>

- In a `switch`,`for`,`forEach`,or `do-while` construct, `break` exits the construct only.
- In an `if-else` construct, `break` doesn't just exit the construct, but also the parent construct that contains the `if-else`.

__NOTE__ <br>
Avoid these mistakes:

- [x] Conditionally breaking from codeblock.
    ```
    [NO]
    while ($True){
        $choice = Read-Host 'Enter A Number'
        if ($choice -eq 0) { break }
    }
    ```
    ```
    [YES]
    do {
        $choice = Read-Host 'Enter A Number'
    } while ($choice -ne 0)
    ```

## Data Structures

- https://www.red-gate.com/simple-talk/sysadmin/powershell/powershell-one-liners-collections-hashtables-arrays-and-strings/

__Array__ <br>

- Create an empty array
    ```powershell
    $myArray = @(); $myArray.getType()

    IsPublic IsSerial Name                                     BaseType
    -------- -------- ----                                     --------
    True     True     Object[]                                 System.Array
    ```

- Initialize an array <br>
    An array can contain values of different datatypes:
    ```powershell
    # Array (flexible type)
    $myArray = 1,'Two',"$PSHome"
    ```
    We can define fixed-type arrays as well:
    ```powershell
    # Array (fixed type)
    [int[]] $myArray = 1,2,3
    ```

    - [x] __Example:__ Get process data
    ```powershell
    $myArray = gps firefox,powershell,sublime_text
    $myArray.length # 11
    ```

- Array of Arrays <br>
    We can initialize an array of arrays (__Jagged Array__):
    ```powershell
    # Array (array-of-arrays)
    $myArray = @(
        (1,2,3),
        (4,5),
        (6,7,8,9))
    $myArray[1][1] # 5
    ```
    We can initialize a matrix (__Non-Jagged Array__):
    ```powershell
    # Array (fixed-size matrix)
    $myArray = New-Object "int32[,]" 3,3
    $myArray[1,1] = 5
    $myArray[1,1] # 5
    ```
    The two differ in that the former doesn't have fixed dimensions (depends on input arrays), but the latter does (3x3).

- Access elements of an Array <br>
    To access an element:
    ```powershell
    $myArray = 1,'Two',"$PSHome"
    $myArray[1] # Two
    ```
    To access a range of elements (__Array Slicing__):
    ```powershell
    $myArray = 1,'Two',"$PSHome"
    $myArray[1..2 +0 +1]

    Two C:\Windows\System32\WindowsPowerShell\v1.0 1 Two
    ```
    This accesses 2nd & 3rd values, then 0th & 1th value. <br>
    To iterate through an array, use `forEach`:
    ```powershell
    $myArray = 1..10; $sum = 0
    $myArray | forEach {$sum += $_}
    $sum # 56
    ```

- Sort an array <br>
    Use `Sort-Object`(aka `sort`).

- Add/Remove elements to/from an array <br>
    To add, simply use `+=`.
    ```powershell
    $myArray = 1..10
    $myArray += 11,12,13
    $myArray # 1 2 3 4 5 6 7 8 9 10 11 12 13
    ```
    To remove, use conditional-operator or regex (`-ne`,`-notlike`,`-notmatch`) to describe the elements (to remove), and store the result back.
    ```powershell
    $myArray = 1..13
    $myArray = $myArray -notmatch '1.'
    $myArray # 1 2 3 4 5 6 7 8 9
    ```

- Working with dynamic arrays <br>
    Use .NET's [`System.Collections.ArrayList`](https://docs.microsoft.com/en-us/dotnet/api/system.collections.arraylist) class to work with dynamic arrays. <br>
    This is useful when you are frequently manipulating an array with large amounts of data.
    ```powershell
    $myArray = New-Object System.Collections.ArrayList
    ```

__Associative Array (Hash Table)__ <br>

- Create an empty hashtable
    ```powershell
    $myHash = @{}; $myHash.getType()

    IsPublic IsSerial Name                                     BaseType
    -------- -------- ----                                     --------
    True     True     Hashtable                                System.Object
    ```

- Initialize a hashtable <br>
    We can initialize a hashtable using `key=value` pairs:
    ```powershell
    $myHash = @{'Name'='Chaitanya'; 'Directory'="$PSHome"}
    $myHash

    Name                           Value
    ----                           -----
    Name                           Chaitanya
    Directory                      C:\Windows\System32\WindowsPowerShell\v1.0
    ```
    We can initialize a hashtable using `key` incrementally:
    ```powershell
    $myHash = @{}; $myHash['Name']='Chaitanya'; $myHash.Directory="$PSHome"
    $myHash

    Name                           Value
    ----                           -----
    Name                           Chaitanya
    Directory                      C:\Windows\System32\WindowsPowerShell\v1.0
    ```

- Sort a hashtable <br>
    Use `.GetEnumerator()` method to access each element. Sort this by `name`/`value` to sort by `key`/`value`.
    ```powershell
    $myHash = @{}; $myHash['Name']='Chaitanya'; $myHash.Veto='Alpha'
    $myHash.GetEnumerator() | sort name

    Name                           Value
    ----                           -----
    Name                           Chaitanya
    Veto                           Alpha


    $myHash.GetEnumerator() | sort value

    Name                           Value
    ----                           -----
    Veto                           Alpha
    Name                           Chaitanya
    ```

__NOTE__: Following are some strongly-typed 'Generic' .NET data structures. These can be useful when you want to constrain the variables to fixed datatypes. <br>
See [`System.Collections.Generic`](https://docs.microsoft.com/en-us/dotnet/standard/generics/collections) for documentation.<br>

__List__ <br>

- Create an empty list
    ```powershell
    $myList = New-Object System.Collections.Generic.List[String]
    $myList = [System.Collections.Generic.List[String]]::new()
    ```
    This syntax is better for use:
    ```powershell
    $myList = [System.Collections.Generic.List[String]]("Apple","Ball","Cat")
    $myList = [System.Collections.Generic.List[String]](cat $myLinks)
    ```

- Add/Remove elements to/from a list <br>
    PS offers 4 add & remove methods each. <br>
    ```
    add/addRange         -> append one/many entries at the end of list.
    insert/insertRange   -> insert one/many entries at specific index.

    remove               -> remove one entry (by value)
    removeAt/removeRange -> remove one/many entries (by index) from a specific index.
    removeAll            -> remove entries based on a criteria.
    ```
    We can use __ScriptBlock (`{}`)__ within `removeAll()` to define criteria. <br>
    This example makes use of `param($x)` to define a [lambda function](https://vexx32.github.io/2018/10/26/Anonymous-Functions/)
    ```powershell
    $myList.add("Ball")
    $myList.addRange([String[]]("Cat","Dog","Ear"))
    $myList # Ball Cat Dog Ear
    $myList.insert(0,"Apple")
    $myList.insertRange(5,[String[]]("Fish","Girl","Ice"))
    $myList # Apple Ball Cat Dog Ear Fish Girl Ice

    $myList.remove("Ice") # True <- value
    $myList # Apple Ball Cat Dog Ear Fish Girl
    $myList.removeAt(6) # <- index
    $myList # Apple Ball Cat Dog Ear Fish
    $myList.removeRange(3,2) # True <- index,count
    $myList # Apple Ball Cat Fish
    $myList.removeAll({param($x) $x -match "^f."}) # 1 <- count
    $myList # Apple Ball Cat
    ```

- Select elements from a list
    ```powershell
    $myList[1] # Bat
    $myList.indexOf('Bat') # 1
    ```

- Modify elements in a list
    ```powershell
    $myList[1] = 'Bat'
    $myList # Apple Bat Cat
    ```

- Search/Sort a list <br>
    Alike `removeAll()`, we can use __ScriptBlock (`{}`)__ within `findAll()` to define criteria. <br>
    `binarySearch()` implements __Binary Search__ to iteratively shrink dataset. This is prefered over `indexOf()` working with extremely large number of values.
    ```powershell
    $myList.findAll({param($x) $x -match "^a."}) # 1 <- count
    # $myList.sort() ??
    # $myList.binarySearch() ??
    $myList = [System.Collections.Generic.List[Int]](1..1000000)
    $myList.binarySearch(1024); $myList.indexOf(1024); # 1023 1023
    ```

__Dictionary__ <br>

- Create an empty dictionary
    ```powershell
    $myDict = New-Object System.Collections.Generic.Dictionary"[String,String]"
    $myDict = [System.Collections.Generic.Dictionary[String,String]]::new()
    ```
    This syntax is better for use: <sup>[BROKEN]</sup>
    ```powershell
    # $myDict = [System.Collections.Generic.Dictionary[String,String]](("Name","Adam")("Group","Alpha")("Grade","A"))
    ```

- Add/Remove elements to/from a dictionary
    ```powershell
    $myDict.add("Name","Chaitanya"); $myDict.Directory = "$PSHome"; $myDict.Age = 23;
    $myDict

    Key       Value
    ---       -----
    Name      Chaitanya
    Directory C:\Windows\System32\WindowsPowerShell\v1.0
    Age       23

    $myDict.remove('Age')
    $myDict

    Key       Value
    ---       -----
    Name      Chaitanya
    Directory C:\Windows\System32\WindowsPowerShell\v1.0
    ```

- Select elements from a dictionary
    ```powershell
    $myDict['Name']; $myDict.Name; # Chaitanya Chaitanya
    ```

- Modify elements in a dictionary
    ```powershell
    $myDict['Name']='Tejaswi'; $myDict.Name; # Tejaswi
    $myDict.Name='Chaitanya'; $myDict.Name;  # Chaitanya
    ```

- Check for key/value in a dictionary
    ```powershell
    $myDict.ContainsKey('Name'); $myDict.ContainsValue('Chaitanya'); # True True
    ```

    - [x] Add a key/value pair to an existing dictionary
    ```powershell
    if (-not $myDict.containsKey('Age')){
        $myDict.add('Age',23)
    }
    $myDict.Age # 23
    ```

- Enumerate a dictionary
    ```powershell
    $myDict.Keys; $myDict.values;
    # Name Directory Age
    # Chaitanya C:\Windows\System32\WindowsPowerShell\v1.0 23
    ```


__Stack__ <br>

- Create an empty stack
    ```powershell
    $myStack = New-Object System.Collections.Generic.Stack[String]
    $myStack = [System.Collections.Generic.Stack[String]]::new()
    ```
    This syntax is better for use:
    ```powershell
    $myStack = [System.Collections.Generic.Stack[String]]("Apple","Ball","Cat")
    $myStack = [System.Collections.Generic.Stack[String]](cat $myLinks)
    ```

- Add/Remove elements to/from a stack
    ```powershell
    $myStack.push("Apple"); $myStack.push("Ball"); $myStack.push("Cat");
    $myStack.pop() # Cat
    ```

- Enumerate a stack
    We can peek at top-of-stack using `.peek()`. To index a stack, convert it to array using `.toArray()`
    ```powershell
    $myStack = [System.Collections.Generic.Stack[String]]('Apple','Ball','Cat')
    $myStack.peek() # Cat
    $myArray = $myStack.toArray() # Cat Ball Apple
    ```

__Queue__ <br>

- Create an empty queue
    ```powershell
    $myQueue = New-Object System.Collections.Generic.Queue[String]
    $myQueue = [System.Collections.Generic.Queue[String]]::new()
    ```
    This syntax is better for use:
    ```powershell
    $myQueue = [System.Collections.Generic.Queue[String]]("Apple","Ball","Cat")
    $myQueue = [System.Collections.Generic.Queue[String]](cat $myLinks)
    ```

- Add/Remove elements to/from a queue
    ```powershell
    $myQueue.enqueue("Apple"); $myQueue.enqueue("Ball"); $myQueue.enqueue("Cat");
    $myQueue.dequeue() # Apple
    ```

- Enumerate a queue
    We can peek at top-of-queue using `.peek()`. To index a queue, convert it to array using `.toArray()`
    ```powershell
    $myQueue = [System.Collections.Generic.Queue[String]]('Apple','Ball','Cat')
    $myQueue.peek() # Apple
    $myArray = $myQueue.toArray() # Apple Ball Cat
    ```

## Objects

Unlike UNIX, PS outputs objects (instead of text). This makes it easy to sort them. <br>
This instruction gets all the members (properties/methods) of `get-process` cmdlet, sorted by name. <br>
    ```powershell
    get-process | get-member | sort Name

       TypeName: System.Diagnostics.Process

    Name                       MemberType     Definition
    ----                       ----------     ----------
    __NounName                 NoteProperty   string __NounName=Process
    BasePriority               Property       int BasePriority {get;}
    BeginErrorReadLine         Method         void BeginErrorReadLine()
    BeginOutputReadLine        Method         void BeginOutputReadLine()
    CancelErrorRead            Method         void CancelErrorRead()
    CancelOutputRead           Method         void CancelOutputRead()
    Close                      Method         void Close()
    CloseMainWindow            Method         bool CloseMainWindow()
    Company                    ScriptProperty System.Object Company {get=$this.Mainmodule.FileVersionInfo.CompanyName;}
    Container                  Property       System.ComponentModel.IContainer Container {get;}
    CPU                        ScriptProperty System.Object CPU {get=$this.TotalProcessorTime.TotalSeconds;}
    CreateObjRef               Method         System.Runtime.Remoting.ObjRef CreateObjRef(type requestedType)
    Description                ScriptProperty System.Object Description {get=$this.Mainmodule.FileVersionInfo.FileDescription;}
    Dispose                    Method         void Dispose(), void IDisposable.Dispose()
    Disposed                   Event          System.EventHandler Disposed(System.Object, System.EventArgs)
    EnableRaisingEvents        Property       bool EnableRaisingEvents {get;set;}
    Equals                     Method         bool Equals(System.Object obj)
    ErrorDataReceived          Event          System.Diagnostics.DataReceivedEventHandler ErrorDataReceived(System.Object, System.Diagnostics.DataReceivedEventArgs)
    ExitCode                   Property       int ExitCode {get;}
    Exited                     Event          System.EventHandler Exited(System.Object, System.EventArgs)
    ExitTime                   Property       datetime ExitTime {get;}
    FileVersion                ScriptProperty System.Object FileVersion {get=$this.Mainmodule.FileVersionInfo.FileVersion;}
    GetHashCode                Method         int GetHashCode()
    GetLifetimeService         Method         System.Object GetLifetimeService()
    GetType                    Method         type GetType()
    Handle                     Property       System.IntPtr Handle {get;}
    HandleCount                Property       int HandleCount {get;}
    Handles                    AliasProperty  Handles = Handlecount
    HasExited                  Property       bool HasExited {get;}
    Id                         Property       int Id {get;}
    InitializeLifetimeService  Method         System.Object InitializeLifetimeService()
    Kill                       Method         void Kill()
    MachineName                Property       string MachineName {get;}
    MainModule                 Property       System.Diagnostics.ProcessModule MainModule {get;}
    MainWindowHandle           Property       System.IntPtr MainWindowHandle {get;}
    MainWindowTitle            Property       string MainWindowTitle {get;}
    MaxWorkingSet              Property       System.IntPtr MaxWorkingSet {get;set;}
    MinWorkingSet              Property       System.IntPtr MinWorkingSet {get;set;}
    Modules                    Property       System.Diagnostics.ProcessModuleCollection Modules {get;}
    Name                       AliasProperty  Name = ProcessName
    NonpagedSystemMemorySize   Property       int NonpagedSystemMemorySize {get;}
    NonpagedSystemMemorySize64 Property       long NonpagedSystemMemorySize64 {get;}
    NPM                        AliasProperty  NPM = NonpagedSystemMemorySize64
    OutputDataReceived         Event          System.Diagnostics.DataReceivedEventHandler OutputDataReceived(System.Object, System.Diagnostics.DataReceivedEventArgs)
    PagedMemorySize            Property       int PagedMemorySize {get;}
    PagedMemorySize64          Property       long PagedMemorySize64 {get;}
    PagedSystemMemorySize      Property       int PagedSystemMemorySize {get;}
    PagedSystemMemorySize64    Property       long PagedSystemMemorySize64 {get;}
    Path                       ScriptProperty System.Object Path {get=$this.Mainmodule.FileName;}
    PeakPagedMemorySize        Property       int PeakPagedMemorySize {get;}
    PeakPagedMemorySize64      Property       long PeakPagedMemorySize64 {get;}
    PeakVirtualMemorySize      Property       int PeakVirtualMemorySize {get;}
    PeakVirtualMemorySize64    Property       long PeakVirtualMemorySize64 {get;}
    PeakWorkingSet             Property       int PeakWorkingSet {get;}
    PeakWorkingSet64           Property       long PeakWorkingSet64 {get;}
    PM                         AliasProperty  PM = PagedMemorySize64
    PriorityBoostEnabled       Property       bool PriorityBoostEnabled {get;set;}
    PriorityClass              Property       System.Diagnostics.ProcessPriorityClass PriorityClass {get;set;}
    PrivateMemorySize          Property       int PrivateMemorySize {get;}
    PrivateMemorySize64        Property       long PrivateMemorySize64 {get;}
    PrivilegedProcessorTime    Property       timespan PrivilegedProcessorTime {get;}
    ProcessName                Property       string ProcessName {get;}
    ProcessorAffinity          Property       System.IntPtr ProcessorAffinity {get;set;}
    Product                    ScriptProperty System.Object Product {get=$this.Mainmodule.FileVersionInfo.ProductName;}
    ProductVersion             ScriptProperty System.Object ProductVersion {get=$this.Mainmodule.FileVersionInfo.ProductVersion;}
    PSConfiguration            PropertySet    PSConfiguration {Name, Id, PriorityClass, FileVersion}
    PSResources                PropertySet    PSResources {Name, Id, Handlecount, WorkingSet, NonPagedMemorySize, PagedMemorySize, PrivateMemorySize, VirtualMemorySize, Threads.Count, TotalProcessorTime}
    Refresh                    Method         void Refresh()
    Responding                 Property       bool Responding {get;}
    SafeHandle                 Property       Microsoft.Win32.SafeHandles.SafeProcessHandle SafeHandle {get;}
    SessionId                  Property       int SessionId {get;}
    SI                         AliasProperty  SI = SessionId
    Site                       Property       System.ComponentModel.ISite Site {get;set;}
    StandardError              Property       System.IO.StreamReader StandardError {get;}
    StandardInput              Property       System.IO.StreamWriter StandardInput {get;}
    StandardOutput             Property       System.IO.StreamReader StandardOutput {get;}
    Start                      Method         bool Start()
    StartInfo                  Property       System.Diagnostics.ProcessStartInfo StartInfo {get;set;}
    StartTime                  Property       datetime StartTime {get;}
    SynchronizingObject        Property       System.ComponentModel.ISynchronizeInvoke SynchronizingObject {get;set;}
    Threads                    Property       System.Diagnostics.ProcessThreadCollection Threads {get;}
    ToString                   Method         string ToString()
    TotalProcessorTime         Property       timespan TotalProcessorTime {get;}
    UserProcessorTime          Property       timespan UserProcessorTime {get;}
    VirtualMemorySize          Property       int VirtualMemorySize {get;}
    VirtualMemorySize64        Property       long VirtualMemorySize64 {get;}
    VM                         AliasProperty  VM = VirtualMemorySize64
    WaitForExit                Method         bool WaitForExit(int milliseconds), void WaitForExit()
    WaitForInputIdle           Method         bool WaitForInputIdle(int milliseconds), bool WaitForInputIdle()
    WorkingSet                 Property       int WorkingSet {get;}
    WorkingSet64               Property       long WorkingSet64 {get;}
    WS                         AliasProperty  WS = WorkingSet64
    ```

- Objects: Sorting & Selecting <br>
    Use `Sort-Object` (`sort`) & `Select-Object`(`select`).
    ```
    # Sort processes based on ID & VM-usage (descending). Display only ID, Name, VM, PM.
    gps | select ID,Name,VM,PM | sort VM,ID -desc
    # Save above table as HTML.
    gps | select ID,Name,VM,PM | sort VM,ID -desc | convertto-html | out-file TEST.html
    ```

__Group-Object__ <br>
We can group objects based on like values/properties. <br>

```powershell
$Object | Measure-Object <switch>
```

- [x] Group email addresses by provider <br>
    ```
    $emails = @('abc@gmail.com', 'xyz@gmail.com', 'abc@yahoo.com', 'abc@yahoo.com')
    $emails | group { ($_ -split '@')[1] }
    ```
    Here, `-split` splits result into two parts, accessible using [0] & [1]. <br>

__Measure-Object__ <br>
We can perfom certain arithmetic operations on objects. <br>

```powershell
$Object | Measure-Object <switch>
```

<center>

| Type | Switch | Meaning |
| :--: | :--: | :-- |
| Numeric | `-Max`, `-Min`, `-Average` | max/min & average of the values |
| String | `-Char`, `-Word`, `-Line` | #chars, #words, #lines in string |

</center>

__Compare-Object__ <br>
We can compare entries within two objects. <br>

```powershell
Compare-Object -reference  $A -difference $B -includeEqual -excludeDifferent
Compare-Object -reference 1,2,3,4 -difference 1,2 -includeEqual

InputObject SideIndicator
----------- -------------
          1 ==
          2 ==
          3 <=
          4 <=
```

- [x] Compare two folders; return files with same name & size.
    ```powershell
    $ref = ls ('C:\Windows\System32' -file); $diff = (ls 'C:\Windows\SysWOW64' -file);
    compare = -ref $ref -diff $diff -property Name,Length -includeEqual -excludeDifferent

    Name                              Length SideIndicator
    ----                              ------ -------------
    @AppHelpToast.png                    232 ==
    @AudioToastIcon.png                  308 ==
    @EnrollmentToastIcon.png             330 ==
    @VpnToastIcon.png                    404 ==
    ...
    ```


- Objects: Selecting (`Sort-Object` v `Where-Object`) <br>
    `Sort-Object` lets you select/filter objects based on properties. <br>
    `Where-Object` lets you select/filter objects based on a criteria. <br>
    ```
    # List all processes with ID >= 1000, sorted in ascending order.
    gps | where {$_.Handles -ge 1000} | sort -Property Handles
    ```
    ```
    Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
    -------  ------    -----      -----     ------     --  -- -----------
       1011      42    39560      23564              4664   0 LenovoVantageService
       1060      22    11932      24380              1044   0 svchost
       1073       6     8740      12100             14168   0 Windows.WARP.JITService
       1136      52    50488      41276              4136   0 Lenovo.Modern.ImController
       1201      17     7516      11760              1208   0 svchost
       1209      91   102380     165036      18.14   8500   1 SearchUI
       1376      22     7248      13288               976   0 lsass
       1453      93   179728     231288      42.13   3016   1 firefox
       2075      81   384764     151604              4332   0 MsMpEng
       3523       9     1868       3972              8632   1 rundll32
       3538     137   119552     132228     153.25   7980   1 explorer
       5508       0      188        136                 4   0 System
    ```

<center><b>TODO</b></center>

- [ ] Study `ls`,`sort`,`select`,`where` cmdlets.

- [x] Get current date & time. Then, show only time.
```
get-date; get-date | select Hour,Minute,Second

Saturday, May 9, 2020 11:49:03 AM

Hour   : 11
Minute : 49
Second : 3

```

- [x] Display a list of installed hotfixes. Display installation date, installed by, and ID, sorted by installation date.
    ```
    get-hotfix | select installedon,installedby,hotfixid | sort installedon

    InstalledOn           installedby         hotfixid
    -----------           -----------         --------
    3/13/2020 12:00:00 AM NT AUTHORITY\SYSTEM KB4538674
    3/13/2020 12:00:00 AM NT AUTHORITY\SYSTEM KB4541338
    3/13/2020 12:00:00 AM NT AUTHORITY\SYSTEM KB4537759
    3/13/2020 12:00:00 AM NT AUTHORITY\SYSTEM KB4537572
    3/13/2020 12:00:00 AM NT AUTHORITY\SYSTEM KB4517245
    4/22/2020 12:00:00 AM NT AUTHORITY\SYSTEM KB4549951
    4/22/2020 12:00:00 AM NT AUTHORITY\SYSTEM KB4552152
    ```

- [x] Display a list of 10 latest Security Event-logs. Display index, time, source for each file, with the oldest entries appearing first (and same-time entries sorted by index).
    ```
    get-eventlog -logname system -newest 10 | select index,timegenerated,source | sort timegenerated,index | out-gridview

    Index TimeGenerated        Source
    ----- -------------        ------
    19167 5/9/2020 10:09:26 AM Microsoft-Windows-TPM-WMI
    19168 5/9/2020 10:09:26 AM Microsoft-Windows-Winlogon
    19169 5/9/2020 10:09:27 AM Microsoft-Windows-TPM-WMI
    19170 5/9/2020 10:11:22 AM DCOM
    19171 5/9/2020 10:11:22 AM DCOM
    19172 5/9/2020 10:11:22 AM DCOM
    19173 5/9/2020 10:11:25 AM Microsoft-Windows-FilterManager
    19174 5/9/2020 10:15:16 AM Service Control Manager
    19175 5/9/2020 10:17:21 AM Service Control Manager
    19176 5/9/2020 10:19:17 AM Microsoft-Windows-Kernel-General
    ```

## Pipelining

- Import/Export
    ```
    man export*
    man import*
    ```
    ```
    Export-Clixml                     Cmdlet
    Export-Csv                        Cmdlet
    Export-FormatData                 Cmdlet
    Export-PSSession                  Cmdlet
    Export-BinaryMiLog                Cmdlet
    Export-WindowsDriver              Cmdlet
    Export-WindowsCapabilitySource    Cmdlet
    Export-WindowsImage               Cmdlet
    Export-Counter                    Cmdlet
    Export-ODataEndpointProxy         Function
    Export-Certificate                Cmdlet
    Export-PfxCertificate             Cmdlet
    Export-ProvisioningPackage        Cmdlet
    Export-Trace                      Cmdlet
    Export-ScheduledTask              Function
    Export-StartLayoutEdgeAssets      Cmdlet
    Export-StartLayout                Cmdlet
    Export-TlsSessionTicketKey        Cmdlet

    ImportSystemModules               Function
    Import-PowerShellDataFile         Function
    Import-Module                     Cmdlet
    Import-Alias                      Cmdlet
    Import-Clixml                     Cmdlet
    Import-Csv                        Cmdlet
    Import-LocalizedData              Cmdlet
    Import-PSSession                  Cmdlet
    Import-PackageProvider            Cmdlet
    Import-BinaryMiLog                Cmdlet
    Import-IseSnippet                 Function
    Import-Counter                    Cmdlet
    Import-PfxCertificate             Cmdlet
    Import-Certificate                Cmdlet
    Import-StartLayout                Cmdlet
    Import-TpmOwnerAuth               Cmdlet
    ```

- Import/Export CSV, XML
    ```
    gps | export-csv process.csv
    gps | export-clixml process.xml
    ```

- Convert to HTML
    ```
    gps | convertto-html | out-file TEST.html
    ls  | convertto-html | out-file -append TEST.html
    ```

<center><b>TODO</b></center>

- [ ] Select/Sort & save necessary columns to `TEST.html`
- [ ] Using CSS, beautiful `TEST.html`

- Compare two structured files (XML) <br>
    ```
    # List different processes running in two PCs (reference, difference).
    diff -reference (import-clixml reference.xml) -difference (gps) -property Name

    name         SideIndicator
    ----         -------------
    calc            =>
    mspaint         =>
    notepad         <=
    ```

- Output <br>
    ```
    out-file     -> O/P to file.
    out-gridview -> O/P to table in new GUI window. (*)
    out-printer  -> O/P to printer (SaveAs PDF).
    ```
    Instead of piping `>` output to a file, use `out-file`. <br>
    `out-file` lets you specify: <br>
    ```
    -append   -> append, not replace contents
    -encoding -> file encoding (ascii, unicode, utf8, oem, string)
    -width    -> set column width (default = 80 characters/line)
    ```
    ```
    # Append outputs instead of replacing.
    ls | out-file -append -width 100 TEST.txt
    ```

__Pipeline Parameter-Binding__ <br>

__Given:__ `cmdA | cmdB`; What exactly goes through the `|`? <br>
__Ans:__ _Objects_ <br>

- Display processes/services from a list of computers connected to your PC.
    ```
    # computers.csv
    hostname,operatingsystem
    Feynman,windows
    ...
    ```
    ```
    # Processes
    gps -ComputerName (import-csv .\computers.csv | select -ExpandProperty hostname) | out-gridview

    # Services
    gsv -ComputerName (import-csv .\computers.csv | select -ExpandProperty hostname) | select Name,Status | sort Name | out-gridview
    ```

## Formatting

- Format-Table, Format-List, Format-Wide, Format-Custom

- Customized Formatting

- [x] Display process names, IDs, responding (to Windows or not) in a table
    ```
    gps | format-table Name,ID,Responding -autosize -wrap
    ```
- [x] Display process names, IDs, virtual/physical memory usage (in MB) in a table
    ```
    gps |
        format-table Name,ID,
        @{name='Virtual(MB)';expression={$_.vm/1MB};formatstring='F2'},
        @{name='Physical(MB)';expression={$_.workingset/1MB};formatstring='F2'} -autosize
    ```
- [x] Display available event-logs - their names and retention periods in a table
    ```
    get-eventlog -list |
        format-table @{name='Name';expression={$_.LogDisplayName}},
                     @{name='Retention(days)';expression={$_.MininumRetentionDays}}
    ```
- [x] Display service grouped-by their status (start/stop) <br>
[[OUTPUT]](services.pdf)
    ```
    gsv | sort status -desc | format-table -groupby status
    ```
- [x] Display a list of all binaries (.exe) in `C:\Windows` with their name, versionInfo & fileSize <br>
[[OUTPUT]](binaries.pdf)
    ```
    ls C:\Windows\*.exe | format-list Name,VersionInfo,@{Name='Size';Expression={$_.length}}
    ```

__NOTE__ <br>

- Prefer outputting Objects over formatted-tables.
    ```
    [NO]
    gwmi win32_logicaldisk |
        format-table deviceid,
        @{name='Total Memory (GB)';e={$_.size/1GB};formatstring='F2'},
        @{name='Free Memory (GB)';e={$_.freespace/1GB};formatstring='F2'}
    ```
    ```
    [YES]
    gwmi win32_logicaldisk |
        select deviceid,
        @{name='Total Memory (GB)';e={$_.size/1GB -as [int]}},
        @{name='Free Memory (GB)';e={$_.freespace/1GB -as [int]}}
    ```
    Here we've used `-as [int]` to round off, as  `Select-Object` doesn't offer `formatstring` option. <br>
    Outputting objects allows us to do this (something which formatted tables don't):
    ```
    .\test.ps1 | export-csv test.csv
    ```

## Filtering & Comparisons

__The Approach__ <br>

- A. Specify what you need. <br>
- B. Filter out what you need from what PS gives you. <br>

    eg. To get a specific service, you can specify it with `get-service`. <br>
    ```
    gsv -name e*,*seo*
    ```
    But if you want a list of ONLY running services, you have to filter. <br>
    ```
    gsv -name e*,*seo* | where {$_.status -eq 'running'}
    ```
    Don't rely on headers O/P by cmdlets like `get-service` to infer it's properties. <br>
    Always use `get-member` to lookup cmdlet property names. <br>
    ```
    gsv | gm
    ```

__The Operators__ <br>
Refer `about_comparison_operators`. <br>

<center>

| Name | Description | Note |
| :-- | :--: | :--: |
| `-eq`,`-ceq` | equal to | `c` implies case-sensitive |
| `-ne`,`-cne` | not equal to | . |
| `-ge`,`-cge`,`-le`,`-cle` | greater/lesser than OR equal to | . |
| `-gt`,`-cgt`,`-lt`,`-clt` | greater/lesser than | . |
| `-and`,`-or`,`-not` | and, or, not | . |
| `-like`,`-clike` | checks matching strings | `c` implies case-sensitive |
| `-match`,`-cmatch` | matches RegEx | . |

</center>

- [x] Example (`-not`): Both these commands check if the process isn't responding.
    ```
    $_.responding -eq $False
    -not $_.responding
    ```

- [x] Display all binaries (EXE) in `C:\Windows\System32` larger than 5MB.
    ```
    ls C:\Windows\System32\*.exe | where {$_.length -gt 5MB}
    ```

- [x] Get all hotfixes that are security updates.
    ```
    get-hotfix -Description 'Security Update'
    ```

- [x] Display a list of all running processes with the name `Conhost` or `Svchost`.
    ```
    gps -name svchost,conhost | format-table -groupby name
    ```

## Background Processes (aka `job`)
PS calls _'background process'_, a `job`. <br>

- Create a local background process (`job`) <br>
    Use [1] for on-spot instructions. Use [2] if instructions are retrieved from a file.
    ```
    start-job -scriptblock {...}
    start-job -filepath ...
    ```
    Although this can also work for remote execution, prefer WMI cmdlets.
    ```
    start-job -scriptblock {get-eventlog security -computerName server1,server2}
    ```

- Create a remote background process (`job`) <br>
    By default, WMI cmdlets, even if they're running on multiple PCs, do so synchronously. <br>
    This means, if PC1 has a long list of commands to execute, it will take forever to execute commands on PC2. <br>
    To do this asynchronously using background process (`job`), attach the `-asJob` parameter to the cmdlet.
    ```
    gwmi Win32_OperatingSystem -computerName (cat allservers.txt) -asJob
    ```

- Managing background processes (`job`) <br>

    ```
    Get-Job     -> Retrieve status of all jobs.
    Receive-Job -> Retrieve results of all jobs.
    Remove-Job  -> Deletes job alongwith all cached memory.
    Stop-Job    -> Terminates a blocked job.
    Wait-Job    -> Forces Shell to wait until a job is finished executing.
    ```
    Retrieving results of a job removes it from memory. To avoid this deletion, use `-keep` parameter.
    ```
    receive-job -id 1 -keep
    ```

- Scheduling background processes (`job`) <br>
    Start by creating a trigger using `New-JobTrigger`. <br>
    Set additional options using `New-ScheduledTaskOption`. <br>
    Register the job with the Task Scheduler using `Register-ScheduledJob`. <br>
    The last step defines the job in 'Task Scheduler' by writing to an XML file. <br>
    For me, this is located at `C:\Users\Chaitanya Tejaswi\AppData\Local\Microsoft\Windows\PowerShell\ScheduledJobs`. <br>

- [x] Play an audio daily at 7:30PM
    ```
    register-scheduledjob -name myDailyJobs -scriptblock {vlc "B:\CRTejaswi\Music\Anna McLuckie - Little Man On The Moon.mp3"} -trigger (new-jobtrigger -daily -at '19:30')
    ```
    While this isn't neccesary for this particular example, use this template to run with elevated privileges.
    ```
    register-scheduledjob
      -name myDailyJobs
      -scriptblock {vlc "B:\CRTejaswi\Music\Anna McLuckie - Little Man On The Moon.mp3"} -trigger (new-jobtrigger -daily -at '19:30')
      -scheduledjoboption (new-scheduledjoboption -waketorun -runelevated)
    ```
    Scheduled tasks are non-volatile, meaning they don't get erased when you close the Shell. This is because it is stored in XML files on disk (`C:\Users\Chaitanya Tejaswi\AppData\Local\Microsoft\Windows\PowerShell\ScheduledJobs`).

## Batch Cmdlets/Processing

## Regular Expressions

Pattern matching is done using `-match`&`-cmatch` operators, and `select-string` cmdlet. <br>


<center>

| Expression | Meaning |
|   :---:   |  :---:  |
| `.` | Any character (except newline) |
| `\d`, `\D` | Digit (0-9), !Digit |
| `\w`, `\W`| Word (a-z, A-Z, 0-9, _), !Word|
| `\s`, `\S` | Whitespace (space, tab, newline), !Whitespace |
| `\b`, `\B` | WordBoundary, !WordBoundary |
| `^`, `$` | Beginning/End of String |
| `[]`,`[^]`,`()`,`{}` | CharacterSet, !CharacterSet, WordGroup, #Values |

__Quantifiers__

| Expression | Meaning |
|   :---:   |  :---:  |
| `0` | >=0 |
| `+` | >=1 |
| `?` | 0 or 1 |
| `{n}` | Exact value (=n) |
| `{start, stop}` | Range of values (min, max) |

</center>

- [x] Get all files that have a 2-digit value in their name. <br>
    ```
    ls | where {$_.name -match "\d{2}"}
    ```
- [x] Display process id, name & company of all processes that are from MicroSoft. <br>
    ```
    gps | where {$_.company -match "^Microsoft"} | select name,id,company
    ```
- [x] Retrieve all DNS entries where Data property is an IPv4 address. <br>
    ```
    Get-DnsClientCache | where {$_.data -match "^\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}"}
    ```
- [x] Retrieve IIS logfile records that contain 40x errors. <br>
    ```
    ls *.log -recurse |
        select-string -pattern "\s40[0-9]\s" |
        format-table Filename,LineNumber,Line -wrap
    ```

## Remote Access
- Windows Remote Management (WinRM)
- Windows Management Instrumentation (WMI) [OLD] & the Common Information Model (CIM) [NEW] <br>
    `Get-WmiObject`,`Invoke-WmiMethod`,`GetCimInstance`,`Invoke-CimMethod` <br>
    WMI commands work over RPCs. CIM commands work over WS-MAN (WinRM). <br>
    CIM instructions need WinRM to be enabled on every PC. So, prefer the old WMI instructions. <br>

- [x] Display all HDD partitions with total/free memory.
    ```
    gwmi win32_logicaldisk |
        format-table deviceid,
        @{name='Total Memory (GB)';e={$_.size/1GB};formatstring='F2'},
        @{name='Free Memory (GB)';e={$_.freespace/1GB};formatstring='F2'}
    ```
- [x] Display list of services sorted by mode (auto, manual)
    ```
    gwmi -class win32_service -Filter "state = 'running'" | sort startmode,name
    ```

## Providers

Refer: [Providers](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_providers) <br>
Also See: [Registry](#registry)

A provider represents a __data storage__ as a __filesystem__. <br>
Windows FileSystem has this heirarchy:
```
File -> Folder -> Drive
```
File/Folder are collectively called __Item__, and so we have items associated with a drive. Since a Provider maps physical storage to drives (even if they're not a filesystem object, eg. Registry): we have cmdlets that deal with __items__. <br>
These verbs apply to all __items__:
```
Get/Set, Copy/Move, New/Remove/Rename/Clear
```
Each provider has certain 'capabilities' for its associated cmdlets. <br>
```
ShouldProcess -> We can use `-WhatIf` & `-Confirm` to test before executing it.
Filter        -> We can use `-Filter` to filter out content.
Credentials   -> We can specify credentials to be provided each time a cmdlet is run.
Transactions  -> We can commit or rollback changes made by the cmdlet.
```

__Built-In Providers__ <br>

<center>

| Provider | Drive(s) | OutputType | Capabilities |
| :--: | :--: | :-- | :-- |
| Alias | `Alias:` | `System.Management.Automation.AliasInfo` | ShouldProcess |
| Certificate | `Cert:` | `Microsoft.PowerShell.Commands.X509StoreLocation` | ? |
| | | `System.Security.Cryptography.X509Certificates.X509Certificate2` | . |
| Environment | `Env:` | `System.Collections.DictionaryEntry` | ShouldProcess |
| FileSystem | `C:` | `System.IO.FileInfo` | ShouldProcess, Filter, Credentials |
| | | `System.IO.DirectoryInfo` | . |
| Function | `Function:` | `System.Management.Automation.FunctionInfo` | ShouldProcess |
| Registry | `HKLM: HKCU:` | `Microsoft.Win32.RegistryKey` | ShouldProcess, Transactions |
| Variable | `Variable:` | `System.Management.Automation.PSVariable` | ShouldProcess |
| WSMan | `WSMan:` | `Microsoft.WSMan.Management.WSManConfigContainerElement` | Credentials |

</center>

## Registry

Refer: [Registry](https://en.m.wikipedia.org/wiki/Windows_Registry)

The Registry is a database that store low-level settings for any Windows OS. <br>
Registry files are stored in `%SystemRoot%\System32\Config` <br>

<center>
    <img src="resources/registry_files.png" title="Registry Files">
</center>

<center>
    <img src="resources/registry_structure.png" title="Registry Structure">
</center>

## Structured Data (CSV, JSON, XML)

https://www.youtube.com/watch?v=Ukuj_DxueIc&app=desktop
https://github.com/jdhitsolutions/psdatafiles/tree/master/demos

__NOTE__ <br>
    All file conversion cmdlets use verbs like `Import`,`Export`,`ConvertTo`,`ConvertFrom`. <br>
    These have specific meaning.
    ```
    Import/Export         -> convert & save to a file.
    ConvertTo/ConvertFrom -> only convert (to be displayed or piped further).
    ```

__CSV__ <br>


## Databases

### SQLite
http://ramblingcookiemonster.github.io/SQLite-and-PowerShell/
https://www.darkartistry.com/2019/08/create-insert-and-query-sqlite-with-powershell/
https://www.tutorialspoint.com/sqlite/index.htm

## Scripting

Also See: [myScripts](scripts.md) <br>
Review: [Control Flow](#control-flow), [Filtering & Comparisons](#filtering-comparisons). <br>

### Basics

__Specifying Parameters__ <br>
    By specifying `CmdletBinding` (at the top of script), we can add several features to our parameters.

- Enabling the features <br>
    All of these features listed below need `CmdletBinding` to be enabled.
    ```
    <#
    # Comments
    #>
    [CmdletBinding()]
    ```

- Mandatory parameters <br>
    Here, we make `$myVar` a mandatory parameter. (The `HelpMessage` is optional).
    ```
    param (
        [Parameter (Mandatory=$True, HelpMessage='A variable-name is mandatory')]
        [string]$myVar,

        [int]$Var1,
        [int]$Var2,
    )
    ```

- Parameter aliases <br>
    Here, we give an alias (`name`) to `$myVar`.
    ```
    param (
        [Parameter (Mandatory=$True)]
        [Alias ('name')]
        [string]$myVar,

        [int]$Var1,
        [int]$Var2,
    )
    ```

- Validating parameter input <br>
    Here, we specify a set of valid inputs for `$Var1` & `$Var2`.
    ```
    param (
        [string]$myVar,

        [ValidateSet(0,1)]
        [int]$Var1,
        [ValidateSet(0,1)]
        [int]$Var2,
    )
    ```
    Read more about validation tricks, [here](https://jdhitsolutions.com/blog/tag/validation/).

- Using `Write-Verbose` to print progress information <br>
    Using `Write-Verbose` (instead of `Write-Host`) in our scripts allows us to optionally determine the progress of our code. <br>
    Verbose output is visible only when the script is run with `-verbose` switch. But, you cannot set its foreground/background colors (as you can with `Write-Host`), since PS has predefined color-codes for Verbose/Debug/Warning. <br>
    ```
    Write-Verbose "Using $myVar Values"
    ```

__Default Parameters__ <br>
Refer `$PSDefaultParameterValues`. <br>
You can set default values for parameters such as `-Path`, `-Credential`, ... by adding these values into a built-in variable, `$PSDefaultParameterValues`. <br>
Note that the scope of `$PSDefaultParameterValues` is limited to the shell when specified in PS, and to the script when specified in a `.ps1` file. This trick can be handy to specify defaults for a script in the script itself and not affect the shell's defaults. <br>

- [x] Specify default-credentials for every cmdlet that has a `-Credential` parameter.
    ```
    $credential = Get-Credential -UserName Administrator -Message "Enter Admin's Password"
    $PSDefaultParameterValues.add('*:Credential', $credential)
    ```

- [x] Ask for credentials everytime an `invoke-command` cmdlet is executed.
    ```
    $PSDefaultParameterValues.add('Invoke-Command:Credential', {Get-Credential -UserName Administrator -Message "Enter Admin's Password"})
    ```
    Since invoke-command runs on local/remote PCs, this works as a basic security measure.


__Operators__ <br>
Refer `about_operators`. <br>

<center>

| Name | Description | Example|
| :-- | :-- | :-- |
| `-as` | type casts an object | `1000/3 -as [int]` => `333` |
| `-is` | type checks an object | `12.45 -is [int]` => `False` |
| `-replace` | replace substring within a string | `'Hello World' -replace 'e',3` => `H3llo World` |
| `-join` | convert array to delimited-string | `1,2,3,4,5 -join '|'` => `1|2|3|4|5` |
| `-split` | convert delimited-string to array | `"1 2 3 4 5" -split " "` => `1, 2, 3, 4, 5` |
| `-like` | wildcarded substring matching | `'this' -like '*his*'` => `True` |
| `-contains` | checks if an object exists in a collection | `'abc','bcd','cde' -contains 'bcd'` => `True`|
| `-in` | checks if an object exists in a collection | `'bcd' -in 'abc','bcd','cde'` => `True`|

</center>

__Manipulating Strings & Dates__ <br>

- Pipe any string to `get-member` to list all associated properties/methods.
    ```
    'Test' | gm
    ```
- Pipe `get-date` to list all associated properties/methods.
    ```
    get-date | gm
    ```
    When working with `WMI` objects, the dates may not be straight-forward.
    ```
    gwmi win32_operatingsystem | select lastbootuptime

    lastbootuptime
    --------------
    20200602222520.500000+330
    ```
    For this, use `ConvertFromDateTime()` & `ConvertToDateTime()` methods. (Check using `gwmi win32_operatingsystem | gm`)
    ```
    $os = gwmi win32_operatingsystem
    $os.ConvertToDateTime($os.LastBootUpTime)

    Tuesday, June 2, 2020 10:25:20 PM
    ```

__Script Blocks (`{}`)__ <br>
Refer `about_script_blocks`. <br>
Anything within `{}` is a __script block__.
<center>
    <img src="resources/01.png" title="Script Blocks: Use Cases">
</center>

https://www.youtube.com/watch?v=u6tdj1IFbpw
https://www.sconstantinou.com/powershell-script-blocks/
https://www.youtube.com/watch?v=WP_Olf8GH_g
https://www.youtube.com/watch?v=uoH6mnzwSZc




# Recipies

Also See: [myScripts](scripts.md)

## Clipboard Operations (`gcb`/`scb`)

- [x] Print/Create PDF from copied text
    ```
    gcb | out-printer
    ```

- [x] Copy a filepath(s) to clipboard (So it becomes easy to paste in GUI)
    ```
    ls *.pdf | % {$_.fullname} | scb
    (ls file.pdf).fullname | scb
    ```

- [x] Convert text (from docx) to epub/html/pdf
    ```
    start input.docx
    gcb | pandoc $_ --metadata title='Title' +RTS -Ksize -RTS -o output.epub
    gcb | pandoc $_ --metadata title='Title' -o output.html
    gcb | pandoc $_ --metadata title='Title' --pdf-engine=xelatex -o output.pdf
    ```

- [x] Copy code(s)/text & save as code-file/text-file <br>
    `ascii` is ideal. `utf8` gives `UTF-8 with BOM`, so, you'll have to re-encode in editor.
    ```
    gcb | out-file -Encoding ascii output.py
    gcb | out-file -Encoding utf8 output.py
    ```
    Instead of creating a new file each time, simply append to the original:
    ```
    gcb | out-file -Append -Encoding ascii output.md
    ```
    This is ideal for simultaneous Copy & Paste (if you are too lazy to use `Ctrl+C`,`Ctrl+V` repeatedly).

- [x] Open links in Firefox
    ```
    # Copied
    firefox (gcb)
    # From text-file
    firefox (cat .\input.txt)
    ```
    A better way is to create a PS User-Profile
    ```
    notepad $profile
    -> Add variables/methods... and save.
    -> Run them from Shell
    ```
    To `$profile`, append path of file containing links `$mylinks=B:\CRTejaswi\Documents\Links.txt`. <br>
    Open links using:
    ```
    firefox (cat $mylinks)
    ```
    Use `subl $mylinks` to update links you want to visit now. <br>
    Read more about PS User-Profiles using: `man about_profiles -ShowWindow`


- [x] [Eject USB drive](https://serverfault.com/a/580298)
    ```
    $driveEject = New-Object -comObject Shell.Application
    $driveEject.Namespace(17).ParseName("D:").InvokeVerb("Eject")
    ```

- [x] Write templated strings to a file <br>
    This writes `file i.mp4; (i = 1-20)` to `MERGE.txt`, and then, attempts to merge these video files.
    ```
    $page = @(); $n=1; while ($n -ne 21) {$page += "file $n.mp4"; $n++}
    $page | out-file -encoding ascii MERGE.txt
    ffmpeg -f concat -safe 0 -i MERGE.txt -c copy OUTPUT.mp4
    ```
    We create an empty array `$page`, append entries to it, and pipe it to `MERGE.txt`.

- [ ] Print strings following a matching sub-string <br>
    From CONFIG, I want to query the cmd to merge files using FFmpeg. <br>
    Logic:
      -> print 10 lines after a 'merge' (inclusive) is encountered.

    These cmd gets `ffmpeg` lines out of CONFIG.
    ```
    (cat $myconfig) -match 'ffmpeg'
    ```
    https://devtipscurator.wordpress.com/2016/11/25/how-to-filter-contents-in-a-text-file-using-powershell/

## Temporary Files

__Create/Delete A Temporary File__ <br>
```
$tmp = New-TemporaryFile
...
del $tmp.FullName -Force
```

- [x] Append source-code & outputs of code-files <br>
    Assuming you have a temporary file `$tmp`, and source file names `test`;
    ```
    py .\test.py | out-file $tmp.fullname; cat .\test.py,$tmp.fullname | out-file -encoding ascii Py3.md -append

    .\test.exe | out-file $tmp.fullname; cat .\test.c,$tmp.fullname | out-file -encoding ascii C.md -append
    ```

## HTTP Requests

PS gives `Invoke-WebRequest` (aka `curl`) to work with webpages. <br>

- [WebScraping (Prateek Singh)](https://github.com/PrateekKumarSingh/PowershellScrapy)
- https://www.youtube.com/watch?v=QrC3ErlxpII
- https://www.youtube.com/watch?v=yCaS8UTmd88
- https://www.youtube.com/watch?v=va0WI9EyR2g
- https://www.youtube.com/watch?v=7liyba6YEG0
- https://www.youtube.com/watch?v=7A_RtRKhMcs
- https://www.youtube.com/watch?v=0DWM3xZbI2Y
- https://www.youtube.com/watch?v=9Patluspez4

__NOTE:__ <br>
Before doing anything useful, make sure to install & configure Internet Explorer. (Although you can make do without this by using `-UseBasicParsing` switch; this is an absolutely fuckall workaround. This is because using it doesn't give you access to the webpage's DOM, which means, you cannot parse it, or get anything useful out of it. It's like standing with a gun - a water gun. So, just install the damn thing!) <br>

- Install from [here](https://support.microsoft.com/en-in/help/17621/internet-explorer-downloads).
- To configure: `Win+I` >> Apps & Features >> Optional Features >> Add a feature >> Internet Explorer 11. Then, `Win+R` >> Control >> Turn Windows Features ON/OFF >> [x] Internet Explorer. Restart PC. <br>
- Open up Internet Explorer, set to `Recommended Settings`. Done! <br>

- [x] Get contents of a URL <br>
    This generates a local copy of my [CV](https://crtejaswi.github.io/CV).
    ```
    $response = Invoke-WebRequest -Uri crtejaswi.github.io/CV
    $response.Content | out-file -encoding utf8 test.html
    ```
- [x] Get all links from a webpage <br>
    This gets all links from my [CV](https://crtejaswi.github.io/CV).
    ```
    $links = (curl crtejaswi.github.io/CV).links.href
    ```

- [x] Get weather (temperatures) <br>
    Retrieving data for Delhi (28.7041° N, 77.1025° E).
    ```
    $myurl = 'https://weather.com/en-IN/weather/today/l/28.7041,77.1025?temp=c'
    $response = curl -Uri $myurl
    ($response.allelements | where { $_.class -Match "\w+--location--\w+" -or $_.class -Match "\w+--tempHiLoValue--\w+" }).innerText

    Rohini Sector 2, Delhi Weather
    35°/26°
    ```

- [x] Get all of my Youtube playlists.

    ```powershell
    $response = curl -Uri 'https://www.youtube.com/channel/UC5NM5NZrw1hV6hgxnaPQPNw/playlists'
    $links = $response.links
    $links.href -match '^/playlist'

    /playlist?list=PL3pGy4HtqwD10u_vC6AksMg_RrIPMzvIA
    /playlist?list=PL3pGy4HtqwD3gyn8LDKSjCVDCbeyhbMTt
    ...
    ```
    Assuming absolute links; this opens all of them in a browser:
    ```powershell
    $links.href -match '^/playlist' | out-file -encoding ascii $tmp.fullname
    firefox (cat $tmp.fullname)
    ```
    This won't work, since all links are relative. This works:
    ```powershell
    $page = @()
    $page += ($links.href -match '^/playlist')
    $page = $page.replace('/playlist','www.youtube.com/playlist')
    $page | out-file -encoding ascii $tmp.fullname
    firefox (cat $tmp.fullname)
    ```

- [ ] Append to my Youtube playlist. <br>
    Given a [playlist](https://www.youtube.com/playlist?list=PL_c7L8RcICKpLCrTT_ZlyBlooup2aHBjT); append it to my own [playlist](https://www.youtube.com/playlist?list=PL3pGy4HtqwD10u_vC6AksMg_RrIPMzvIA), so I can easily watch it on my phone/pc/tv. <br>
    Since, we'll send a POST request, we need to use YouTube's API. <br>
    To manually insert videos from a playlist, we add `&disable_polymer=true` to the original link, then `Add All`. Is this any useful for querying the API? <br>


__REST APIs__ <br>

- https://www.youtube.com/watch?v=Uk0IHzR57hQ
- https://www.youtube.com/watch?v=7mEmQgGowMY
- https://www.youtube.com/watch?v=9piyM38it_8
- https://www.youtube.com/watch?v=RgwrHpE0IK4
- https://www.youtube.com/watch?v=t66ZgGeykug
- https://www.youtube.com/watch?v=mYcGV5BwMt4
- https://www.youtube.com/watch?v=oUl-Mmwvgec
- https://www.youtube.com/watch?v=_WjT2mrKXuE
- https://www.youtube.com/watch?v=yfwTAnfMhzw
- https://www.youtube.com/watch?v=AAPxI4QXPw8

## To-Do

<center>
    <img src="resources/02.png" title="To-Do">
</center>

Also, finish review exercises & put code in relevant sections.


## Tools

- [x] Write a utility that gets technical specifications of remote PCs. <br>
Each entry is logged into a database. If this fails, log error info in a file.

```powershell
<#
.SYNOPSIS
Gets system-specifications of remote computers on a CIM server.
.EXAMPLE
PS> Get-MachineInfo -ComputerName localhost
ComputerName Version    ServicePackMajorVersion
------------ -------    -----------------------
localhost    10.0.18363                       0
#>

function Get-MachineInfo {
    param (
        [string[]]$ComputerName,
        [string]$LogFailuresToPath,
        [string]$Protocol = 'wsman',
        [switch]$ProtocolFallback
    )

    forEach ($name in $computername){
        if ($protocol -eq 'DCom'){
            $option = New-CimSessionOption -Protocol DCom
        } else {
            $option = New-CimSessionOption -Protocol WSMan
        }

        $session = New-CimSession -ComputerName $name -SessionOption $option
        $os = Get-CimInstance -ClassName Win32_OperatingSystem -CimSession $session
        $session | Remove-CimSession
        $os | Select-Object -Property @{n='ComputerName'; e={$name}},Version,ServicePackMajorVersion
    }
}
```
```
PS> import-module -name 'C:\Users\Chaitanya Tejaswi\Documents\WindowsPowerShell\Modules\myTest' -force -verbose

VERBOSE: Loading module from path 'C:\Users\Chaitanya Tejaswi\Documents\WindowsPowerShell\Modules\myTest\myTest.psm1'.
VERBOSE: Exporting function 'Get-MachineInfo'.
VERBOSE: Importing function 'Get-MachineInfo'.```

PS> Get-MachineInfo -ComputerName localhost

ComputerName Version    ServicePackMajorVersion
------------ -------    -----------------------
localhost    10.0.18363                       0
```

- [x] Write a utility that changes logon passwords of services of remote PCs. <br>
If this fails, log error info in a file. Passwords may be supplied as plain string.

```powershell
<#
.SYNOPSIS
Sets logon passwords of services of remote computers on a CIM server.
.EXAMPLE
PS> Set-ServiceLogon -ComputerName localhost
                     -ServiceName BITS
                     -NewPassword 'P@ssw0rd'

PS> Set-ServiceLogon -ComputerName S1,S2
                     -ServiceName BITS
                     -NewPassword 'P@ssw0rd'
                     -NewUser 'COMPANY\User'
#>

function Set-ServiceLogon {
    param (
        [string[]]$ComputerName,
        [string]$ServiceName,
        [string]$NewUser,
        [string]$NewPassword,
        [string]$ErrorLogFilePath
    )

    forEach ($name in $computername){
        $option = New-CimSessionOption -Protocol WSMan
        $session = New-CimSession -ComputerName $name -SessionOption $option

        if ($PSBoundParameters.ContainsKey('NewUser')){
            $args = @{'StartName'=$NewUser; 'StartPassword'=$NewPassword}
        } else {
            $args = @{'StartPassword'=$NewPassword}
        }

        Invoke-CimMethod -ComputerName $name -MethodName Change `
                         -Query "SELECT * FROM  Win32_Service WHERE name='$ServiceName'" `
                         -Arguments $args |
                            Select-Object -Property @{n='ComputerName'; e={$name}} `
                                                    @{n='Result'; e={$_.ReturnValue}}

        $session | Remove-CimSession
    }
}
```

# PowerShell & Python