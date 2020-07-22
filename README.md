# OpenEnclave on ACC (Azure Confidential Computing)
OpenEnclave on ACC VM, see also [FAQ](https://docs.microsoft.com/en-us/azure/confidential-computing/faq)

## Create ACC Windows Gen2 VM for testing
### Create ACC Gen2 VM
   See [Support for generation 2 VMs on Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/generation-2)
    
    For example:
    Location: East US
    Operating system: Windows (Windows Server 2019 Datacenter)
    SKU : 2019-datacenter-gen2
    Publisher : microsoftwindowsserver
    VM generation : V2
    Size: Standard DC1s_v2
    vCPUs : 1
    RAM : 4 GiB
   If you are using Azure team subscription and public IP address must be protected by [JitAccess](https://docs.microsoft.com/en-us/azure/security-center/security-center-just-in-time?tabs=jit-config-avm%2Cjit-request-asc)

## Setup Windows Development box
Based on the [doc](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/Contributors/WindowsManualSGX1FLCDCAPPrereqs.md)
### Install Visual Studio 2019
1. Launch VS2019 "x64 native tools command prompt"
2. Registering Intel® Software Guard Extensions SDK for Windows
3. Downlaod and unzip [PSW](http://registrationcenter-download.intel.com/akdlm/irc_nas/16464/Intel%20SGX%20PSW%20for%20Windows%20v2.7.100.2.exe)
      For example 
      E:\sgx\Intel SGX PSW for Windows v2.7.100.2\PSW_EXE_RS2_and_before\Intel(R)_SGX_Windows_x64_PSW_2.7.100.2.exe"
4. Download and up [DCAP]()  
      For example
      E:\sgx\Intel SGX DCAP for Windows v1.7.100.2

5. Follow the [doc](https://github.com/openenclave/openenclave/blob/master/samples/README_Windows.md)
      5.1 Install the oe_prereqs
         https://www.nuget.org/packages/open-enclave/
         nuget.exe install open-enclave -Source e:\openenclave_nuget -OutputDirectory e:\oe_prereqs -ExcludeVersion
         nuget.exe install EnclaveCommonAPI -Source e:\openenclave_nuget -OutputDirectory e:\oe_prereqs -ExcludeVersion
         nuget.exe install DCAP_Components -Source e:\openenclave_nuget -OutputDirectory e:\oe_prereqs -ExcludeVersion
       5.2 set CMAKE_PREFIX_PATH=e:\openenclave\lib\openenclave\cmake
       5.3 Hello world
          E:\oe\openenclave\share\openenclave\samples\helloworld\release>cmake -DCMAKE_BUILD_TYPE=Release .. -G Ninja -DNUGET_PACKAGE_PATH=E:\oe_prereqs 
          E:\oe\openenclave\share\openenclave\samples\helloworld\release>ninja
          E:\oe\openenclave\share\openenclave\samples\helloworld\release>ninja run
             [1/1] cmd.exe /C "cd /D E:\oe\openenclave\share\ope.../samples/helloworld/release/enclave/enclave.signed"
             Hello world from the enclave
             Enclave called into host to print: Hello World!
## Test on ACC VM
    C:\Users\puliu\openenclave\bin>oesgx
    CPU supports SGX_FLC:Flexible Launch Control
    CPU supports Software Guard Extensions:SGX1
    MaxEnclaveSize_64: 2^(36)
    EPC size on the platform: 27262976

    C:\Users\puliu\oe-hello-world-release\host>helloworld_host.exe ..\enclave\enclave.signed
    Hello world from the enclave
    Enclave called into host to print: Hello World!

