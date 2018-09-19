1. From a command prompt:
```
git clone https://github.com/Microsoft/vcpkg.git
```
2. bootstrap-vcpkg
```
powershell
PS> .\bootstrap-vcpkg.bat -disableMetrics
```
3. Integrate vcpkg in Visual Studio.
```
PS> .\vcpkg integrate install
```
4. Install some libraries (here, for two platforms/archs. )
```
PS> .\vcpkg install zlib:x64-windows zlib:x86-windows
```
5. Install [LLVM 8.0.0-r339319](https://prereleases.llvm.org/win-snapshots/LLVM-8.0.0-r339319-win64.exe)
6. Install [LLVM VS extention](https://marketplace.visualstudio.com/items?itemName=LLVMExtensions.llvm-toolchain)
7. Start Visual Studio 2017
8. File menu: New->Project->Visual C++->Windows Desktop
9. Leave name as Project1
10. Uncheck Precompiled Headers and SDL Checks
11. Click Ok     <br />
    Creates an Hello World stub Project1.cpp.

12. Save Project1.cpp as UTF8 No BOM ( Wizard generated a UTF16 file with BOM that clang, and everyone else, hates)

	File->Save Project1.cpp As...->Save (Click Little Down arrow at the Save button)
	->Save with Encoding...

	Confirm to replace.

	Encoding: Unicode (UTF-8 without signiture) - Codepage 65001
	Click Ok

In Project->Project1 Properties->Configuration Properties->

13. Set LLVM

	General->Platform Toolset->llvm
	
14. Disable DEBUG:FASTLINK for Debug (lld-link does not like /DEBUG:FASTLINK ...):

	Set Configuration->Debug

	Set Platform-> All Platforms

	Linker->Debugging->Generate Debug Info->/DEBUG:FULL

	Note! DEBUG alone now also defaults to DEBUG:FASTLINK

15. Disable /LTCG:... For Release (... nor /LTCG:) :

	Set Configuration->Release

	Set Platform-> All Platforms

	Linker->Optimization->Link Time Code Generation->Default

16. Build

	Generates an error like:

	`Error: could not open c:\src\vcpkg\installed\x64-windows\debug\lib\*.lib: invalid argument`

17. Remove VS integration:
```
	PS> .\vcpkg integrate remove
```
18. Build again.

	Should succeed now.

