This article is about using various software development languages and frameworks to programme or to use, test or extend existing open-source projects. It is focussed on using macOS devices to work, but also shares some useful tips that could apply to Windows and Linux distributions.

See also, in sister repositories:

* some coding notes for linux-based systems
	* [https://github.com/artmg/lubuild/blob/master/help/use/coding.md]
		* C, python, IDEs, building and extending Samba
* source code publishing, collaboration and version control with GitHub
	* [https://github.com/artmg/lubuild/blob/master/help/use/git-source-control.md]
* Useing Google Apps Script on top of Node.js 
	* [https://github.com/artmg/cooking-with-GAS/README.md]
* markdown-specific editors
	* https://github.com/artmg/lubuild/blob/master/help/use/markdown.md
* basic, older notes for node-js on RaspberryPi OS
    * [https://github.com/artmg/MuGammaPi/wiki/node-js.md]
* Repository dedicated to solution tips for the R statistical language and its IDEs
	* [https://gitlab.com/artmg/scienzaRt]

## Installing development tools

```
brew install        node           # JavaScript runtime
brew install --cask node           # JavaScript runtime
brew install visual-studio-code    # (mostly-) popular multi-language IDE
```




## Node.js

### Simple installation

Node.js is a widely used platform, so check first in case you have it already.

```
node -v
```

If not, we recommend the ‘long term stable’ version that avoids excessive updates and issues, use your preferred package manager to install it, e.g. `choco install nodejs-lts` or e.g. `brew install node@22` (see other versions in https://nodejs.dev/en/about/releases/ and https://formulae.brew.sh/formula/node ) 

### Managed versions

You might have different apps that have very specific requirements for node versions. This is where the node version manager comes in. These instructions were created for zsh on macOS, but if you substitute `brew` with `choco install nvm` or `runtime` or `nvm-sh` then you can easily get them working on Windows or Linux too.

```zsh
brew install nvm

# you may choose a different profile depending on your own set up
cat >> ${ZDOTDIR:-~}/.zshrc <<EndOfConfigZshRC

# Add nvm to your shell profile
source $(brew --prefix nvm)/nvm.sh

EndOfConfigZshRC
```

* restart your terminal to get use the new profile

```zsh
# Verify the NVM version
nvm --version

nvm install --lts

nvm ls

nvm use --lts
```

alternative configs

```
export NODE_VERSION=18
nvm install ${NODE_VERSION}
node -v

# this might be redundant, but it won't hurt
# assuming you don't need a different version as default for another toolset
nvm alias default ${NODE_VERSION}
```

If you want to control where nvm stores everything, `$NVM_DIR` overrides the default `~/.nvm` – to remove nvm you can just delete the folder. 



### Yarn package manager

In the popular JavaScript runtime Node.js, Yarn is an alternative package manager to `npm`. Some people prefer it as it installs packages in parallel so should be faster. It is also used to build a package from your code base. 
Note that `brew install yarn` will only install classic yarn v1.22.22, rather than modern yarn version 4+. Yet yarn is now delivered with node, so you can use

```
brew install node

# make the yarn binary available
corepack enable

# upgrade to the new 'modern' version
md yarntemp
cd yarntemp
yarn init -2

# if you later want to upgrade to the latest version
yarn set version stable
yarn install

# if your project needs you to revert to the classic version
yarn set version classic
```


# misc development tips
## GTK porting

You may have a favourite Linux application that uses the GTK widget framework for its graphical interface (GUI). If so then the [GTK Quartz integration](https://www.gtk.org/docs/installations/macos) will make it relatively straight-forward to make an app work with the [Quartz graphical layer](https://en.wikipedia.org/wiki/Quartz_(graphics_layer)) in macOS.

### Using Valgrind to diagnose memory issues

* seg fault 11
* install
    * brew install valgrind
    * macos 10.14 version not currently supported
    * brew install --HEAD https://raw.githubusercontent.com/sowson/valgrind/master/valgrind.rb



```
2020-03-10 15-51-23.411 [Notice] (playerCallback) No chunk received for 5000ms. Closing Audio Queue.
AddressSanitizer:DEADLYSIGNAL
=================================================================
==27258==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000018 (pc 0x00010084c155 bp 0x700009584b30 sp 0x700009584600 T2)
==27258==The signal is caused by a WRITE memory access.
==27258==Hint: address points to the zero page.
    #0 0x10084c154  (snapclient:x86_64+0x100118154)
    #1 0x10084a7ea  (snapclient:x86_64+0x1001167ea)
    #2 0x10080c6f5 in void* std::__1::__thread_proxy<std::__1::tuple<std::__1::unique_ptr<std::__1::__thread_struct, std::__1::default_delete<std::__1::__thread_struct> >, void (Player::*)(), Player*> >(void*) (snapclient:x86_64+0x1000d86f5)
    #3 0x7fff68dd22ea in _pthread_body (libsystem_pthread.dylib:x86_64+0x32ea)
    #4 0x7fff68dd5248 in _pthread_start (libsystem_pthread.dylib:x86_64+0x6248)
    #5 0x7fff68dd140c in thread_start (libsystem_pthread.dylib:x86_64+0x240c)

==27258==Register values:
rax = 0x00000000000044e8  rbx = 0x00007000095849e0  rcx = 0x0000100000000000  rdx = 0x0000000000000008
rdi = 0x0000000000000018  rsi = 0x0000100000000000  rbp = 0x0000700009584b30  rsp = 0x0000700009584600
 r8 = 0x000060f000007840   r9 = 0x0000700009584801  r10 = 0x000000000000007d  r11 = 0x0000000000000002
r12 = 0x0000700009584600  r13 = 0x00001e00012b08ce  r14 = 0x0000700009584828  r15 = 0x00001e00012b08c0
AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV (snapclient:x86_64+0x100118154)
Thread T2 created by T1 here:
    #0 0x1009d102d in wrap_pthread_create (libclang_rt.asan_osx_dynamic.dylib:x86_64h+0x5402d)
    #1 0x10080c54e  (snapclient:x86_64+0x1000d854e)
    #2 0x100808a86  (snapclient:x86_64+0x1000d4a86)
    #3 0x1008251d6  (snapclient:x86_64+0x1000f11d6)
    #4 0x1007cd40a  (snapclient:x86_64+0x10009940a)
    #5 0x1007ce25b  (snapclient:x86_64+0x10009a25b)
    #6 0x100801505 in void* std::__1::__thread_proxy<std::__1::tuple<std::__1::unique_ptr<std::__1::__thread_struct, std::__1::default_delete<std::__1::__thread_struct> >, void (ClientConnection::*)(), ClientConnection*> >(void*) (snapclient:x86_64+0x1000cd505)
    #7 0x7fff68dd22ea in _pthread_body (libsystem_pthread.dylib:x86_64+0x32ea)
    #8 0x7fff68dd5248 in _pthread_start (libsystem_pthread.dylib:x86_64+0x6248)
    #9 0x7fff68dd140c in thread_start (libsystem_pthread.dylib:x86_64+0x240c)

Thread T1 created by T0 here:
    #0 0x1009d102d in wrap_pthread_create (libclang_rt.asan_osx_dynamic.dylib:x86_64h+0x5402d)
    #1 0x10080135e  (snapclient:x86_64+0x1000cd35e)
    #2 0x1007c2f69  (snapclient:x86_64+0x10008ef69)
    #3 0x10082a8da  (snapclient:x86_64+0x1000f68da)
    #4 0x100747764  (snapclient:x86_64+0x100013764)
    #5 0x7fff68bde3d4 in start (libdyld.dylib:x86_64+0x163d4)

==27258==ABORTING
Abort trap: 6
```

* valgrind --leak-check=yes /usr/local/bin/snapclient
* valgrind --leak-check=yes --main-stacksize=8388608 /usr/local/bin/snapclient
* valgrind --leak-check=yes --main-stacksize=134217728 /usr/local/bin/snapclient
* 

```

==26740== Process terminating with default action of signal 11 (SIGSEGV)
==26740==  Access not within mapped region at address 0x18
==26740==    at 0x10133E808: _pthread_wqthread_setup (in /usr/lib/system/libsystem_pthread.dylib)
==26740==    by 0x10133E497: _pthread_wqthread (in /usr/lib/system/libsystem_pthread.dylib)
==26740==    by 0x10133E3FC: start_wqthread (in /usr/lib/system/libsystem_pthread.dylib)
==26740==  If you believe this happened as a result of a stack
==26740==  overflow in your program's main thread (unlikely but
==26740==  possible), you can try to increase the size of the
==26740==  main thread stack using the --main-stacksize= flag.
==26740==  The main thread stack size used in this run was 8388608.

valgrind: m_scheduler/scheduler.c:1028 (void run_thread_for_a_while(HWord *, Int *, ThreadId, HWord, Bool)): Assertion 'VG_(in_generated_code) == False' failed.

host stacktrace:
==26740==    at 0x258041E10: ???
==26740==    by 0x258042186: ???
==26740==    by 0x258042166: ???
==26740==    by 0x2580B8D47: ???
==26740==    by 0x2580B6E61: ???
==26740==    by 0x2580C81C7: ???
==26740==    by 0x2580C8484: ???

sched status:
  running_tid=5

Thread 1: status = VgTs_WaitSys syscall unix:305 (lwpid 771)
==26740==    at 0x1012EB866: __psynch_cvwait (in /usr/lib/system/libsystem_kernel.dylib)
==26740==    by 0x10134256D: _pthread_cond_wait (in /usr/lib/system/libsystem_pthread.dylib)
==26740==    by 0x100E3AB30: std::__1::condition_variable::__do_timed_wait(std::__1::unique_lock<std::__1::mutex>&, std::__1::chrono::time_point<std::__1::chrono::system_clock, std::__1::chrono::duration<long long, std::__1::ratio<1l, 1000000000l> > >) (in /usr/lib/libc++.1.dylib)
==26740==    by 0x100020504: ??? (in /usr/local/bin/snapclient)
==26740==    by 0x100020427: ??? (in /usr/local/bin/snapclient)
==26740==    by 0x10002C96A: ??? (in /usr/local/bin/snapclient)
==26740==    by 0x10002C451: ??? (in /usr/local/bin/snapclient)
==26740==    by 0x1000462B5: void* std::__1::__thread_proxy<std::__1::tuple<std::__1::unique_ptr<std::__1::__thread_struct, std::__1::default_delete<std::__1::__thread_struct> >, void (Player::*)(), Player*> >(void*) (in /usr/local/bin/snapclient)
==26740==    by 0x1000060E6: ??? (in /usr/local/bin/snapclient)
==26740==    by 0x1010543D4: start (in /usr/lib/system/libdyld.dylib)
client stack range: [0x104149000 0x104948FFF] client SP: 0x104947C48
valgrind stack range: [0x7000009B2000 0x700000AB1FFF] top usage: 9768 of 1048576

Thread 4: status = VgTs_Yielding (lwpid 9731)
==26740==    at 0x10133E808: _pthread_wqthread_setup (in /usr/lib/system/libsystem_pthread.dylib)
==26740==    by 0x10133E497: _pthread_wqthread (in /usr/lib/system/libsystem_pthread.dylib)
==26740==    by 0x10133E3FC: start_wqthread (in /usr/lib/system/libsystem_pthread.dylib)
client stack range: ??????? client SP: 0x700003C79F80
valgrind stack range: [0x700003F4A000 0x700004049FFF] top usage: 3808 of 1048576

Thread 5: status = VgTs_Runnable (lwpid 9475)
==26740==    at 0x10133E3F0: start_wqthread (in /usr/lib/system/libsystem_pthread.dylib)
client stack range: ??????? client SP: 0x700003F45B80
valgrind stack range: [0x70000404E000 0x70000414DFFF] top usage: 8720 of 1048576

```

Note: see also the FAQ in the source distribution.
It contains workarounds to several common problems.
In particular, if Valgrind aborted or crashed after
identifying problems in your program, there's a good chance
that fixing those problems will prevent Valgrind aborting or
crashing, especially if it happened in m_mallocfree.c.

If that doesn't help, please report this bug to: www.valgrind.org

In the bug report, send all the above text, the valgrind
version, and what OS and version you are using.  Thanks.


