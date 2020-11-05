# class-dump-linux
The purpose is porting mach app to linux through GNUStep.

Install AFKit on Linux(Ubuntu)

BasicInfo(pre-requisites): Ubuntu20.04, AFKit for Mac
(Ubuntu 16.04 is also tested with success)

Way 1[recommend]
___________________________________________________________________________________________________________________________________________________
step1: Install basic tools

       $ sudo apt-get install make cmake git


step2: Install GNUstep environment

       refer: http://wiki.gnustep.org/index.php/GNUstep_under_Ubuntu_Linux

       $ git clone https://github.com/plaurent/gnustep-build    
       $ cd gnustep-build/[your platform target(such as ubuntu-20.04-clang-10.0-runtime-2.0)]
       $ sudo ./[your platform target].sh


step3: Install relative lib
       
       $ sudo apt-get install libssl-dev


step4: Copy <linux_base.h> from Apple.Inc offical website

       refer: https://opensource.apple.com/source/libdispatch/libdispatch-1008.200.78/os/linux_base.h
       
       copy <linux_base.h> to [AFKit root]/dependence/os/


step5: Delete redefined file
    
       $ sudo rm -rf /usr/include/x86_64-linux-gnu(or move this out from include path before you make AFKit successfully)
       
       if this directory is not exists,please see the output by compiler when make AFkit or restart GNUstep bash script


step6: Change GNUmakefile
    
       $ cd [AFKit root]/source/
       
       in <GNUmakeFile_afKit.preamble> and <GNUmakeFile_MachObjC.preamble>:
           add '-fobj-runtime=gnustep-2.0' to "ADDITIONAL_OBJCFLAGS"

       in <GNUmakeFile_MachObjC.preamble>:
           add '-fblocks' to "ADDITIONAL_OBJCFLAGS"
       
       (optianl:if you default compiler isn't clang)
       in <GNUmakefile>
           add 'CC=clang' or use '$ make CC=clang' in next step


step7: Make AFKit

       $ cd [AFKit root]/source/
       $ make  

	step 7 ,如果缺少<opensslconf.h>,就把step5 中x86_64-linux-gnu/openssl/opensslconf.h移动到/usr/include/openssl/


step8: Set environment variables   
       
       add directory that contains [libMachObjC.so] (default as [AFKit root]/source/obj) to LD_LIBRARY_PATH, such as [export LD_LIBRARY_PATH=xx]
       dont't break the original PATH
       
       after that,LD_LIBRARY_PATH look like:
           LD_LIBRARY_PATH=...:/usr/GNUstep/Local/Library/Libraries:/usr/GNUstep/System/Library/Libraries:/xx/xx(contains libMachObjC.so):...


step9: Use AFKit
       
       $ cd [AFKit root]/source/obj
       $ ./afkit 

___________________________________________________________________________________________________________________________________________________



Way 2[not recommend,may be not work]
___________________________________________________________________________________________________________________________________________________
step1: Install GNUStep environment
       
       sudo apt-get install make gnustep gnustep-devel

       //yum install epel-release
       //yum install gnustep-* 
   


step2: Install llvm and clang
       
       sudo apt-get install llvm clang

       //yum install clang (llvm should be auto install)
       //*yum install llvm (if llvm is not install)
    


step3: Install relative lib 
       
       sudo apt-get install libssl-dev libblocksruntime-dev libdispatch-dev
       (optianl) sudo apt-get install make cmake
       (optianl) sudo apt-get install git


same as [Way 1] step6~9

if failed:
       download source code about [gnustep-base],[libobjc2] and compile them by yourself  
       and try step7~9 again

___________________________________________________________________________________________________________________________________________________
