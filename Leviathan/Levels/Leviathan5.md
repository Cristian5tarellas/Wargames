# Leviathan Level 5

You can watch the walkthrough for this level here:  
[![YouTube](https://img.shields.io/badge/YouTube-Walkthrough-red?logo=youtube)](https://www.youtube.com/watch?v=Bbqw-52itfQ&t=1s&ab_channel=Gabahack)

> This video shows my full process solving (in Spanish) Level 5 from scratch, including the obstacles and mistakes I faced along the way. Some walkthroughs might be longer or shorter depending on the complexity of the level or how quickly I find the solution.

---


## 🔍 Exploration

We begin by inspecting the current directory. Checking the binary founded with SUID permissions.

```bash
leviathan5@gibson:~$ ls
leviathan5
leviathan5@gibson:~$ file leviathan5 
leviathan5: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=0fbe5a715bf8cd28d02bc0e989a37f9c0ab21614, for GNU/Linux 3.2.0, not stripped
```
We explore the strings of the binary

```bash
leviathan5@gibson:~$ strings leviathan5 
td4 
/lib/ld-linux.so.2
_IO_stdin_used
fgetc
puts
exit
setuid
putchar
unlink
fopen
feof
__libc_start_main
fclose
getuid
libc.so.6
GLIBC_2.0
GLIBC_2.1
GLIBC_2.34
__gmon_start__
/tmp/file.log
Cannot find /tmp/file.log
;*2$"(
GCC: (Ubuntu 13.3.0-6ubuntu2~24.04) 13.3.0
crt1.o
__abi_tag
__wrap_main
crtstuff.c
deregister_tm_clones
__do_global_dtors_aux
completed.0
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
leviathan5.c
__FRAME_END__
_DYNAMIC
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
__libc_start_main@GLIBC_2.34
__x86.get_pc_thunk.bx
_edata
fclose@GLIBC_2.1
_fini
getuid@GLIBC_2.0
unlink@GLIBC_2.0
__data_start
puts@GLIBC_2.0
__gmon_start__
exit@GLIBC_2.0
__dso_handle
_IO_stdin_used
feof@GLIBC_2.0
fopen@GLIBC_2.1
putchar@GLIBC_2.0
_end
_dl_relocate_static_pie
_fp_hw
fgetc@GLIBC_2.0
__bss_start
setuid@GLIBC_2.0
__TMC_END__
_init
.symtab
.strtab
.shstrtab
.interp
.note.gnu.build-id
.note.ABI-tag
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rel.dyn
.rel.plt
.init
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.init_array
.fini_array
.dynamic
.got
.got.plt
.data
.bss
.comment
```

We can observe that it looks for a specific file:
```bash
/tmp/file.log
Cannot find /tmp/file.log
```
And if we executed, the binary ask for it:
```bash
leviathan5@gibson:~$ ./leviathan5 
Cannot find /tmp/file.log
```
If we test the binary creating the file necessary we can see that it is reading the file and removing it:
```bash
leviathan5@gibson:~$ echo 'This is a test' > /tmp/file.log
leviathan5@gibson:~$ ./leviathan5 
This is a test
leviathan5@gibson:~$ ./leviathan5 
Cannot find /tmp/file.log
```

Lets trace the library calls to observe how the binary works without and with file:
- without file:
```bash
leviathan5@gibson:~$ ltrace ./leviathan5 
__libc_start_main(0x804910d, 1, 0xffffd384, 0 <unfinished ...>
fopen("/tmp/file.log", "r")                                                                                      = 0
puts("Cannot find /tmp/file.log"Cannot find /tmp/file.log
)                                                                                = 26
exit(-1 <no return ...>
+++ exited (status 255) +++
```
with file:
```bash
leviathan5@gibson:~$ echo 'This is a test' > /tmp/file.log
leviathan5@gibson:~$ ltrace ./leviathan5 
__libc_start_main(0x804910d, 1, 0xffffd384, 0 <unfinished ...>
fopen("/tmp/file.log", "r")                                                                                      = 0x804d1a0
fgetc(0x804d1a0)                                                                                                 = 'T'
feof(0x804d1a0)                                                                                                  = 0
putchar(84, 0x804a008, 0, 0)                                                                                     = 84
fgetc(0x804d1a0)                                                                                                 = 'h'
feof(0x804d1a0)                                                                                                  = 0
putchar(104, 0x804a008, 0, 0)                                                                                    = 104
fgetc(0x804d1a0)                                                                                                 = 'i'
feof(0x804d1a0)                                                                                                  = 0
putchar(105, 0x804a008, 0, 0)                                                                                    = 105
fgetc(0x804d1a0)                                                                                                 = 's'
feof(0x804d1a0)                                                                                                  = 0
putchar(115, 0x804a008, 0, 0)                                                                                    = 115
fgetc(0x804d1a0)                                                                                                 = ' '
feof(0x804d1a0)                                                                                                  = 0
putchar(32, 0x804a008, 0, 0)                                                                                     = 32
fgetc(0x804d1a0)                                                                                                 = 'i'
feof(0x804d1a0)                                                                                                  = 0
putchar(105, 0x804a008, 0, 0)                                                                                    = 105
fgetc(0x804d1a0)                                                                                                 = 's'
feof(0x804d1a0)                                                                                                  = 0
putchar(115, 0x804a008, 0, 0)                                                                                    = 115
fgetc(0x804d1a0)                                                                                                 = ' '
feof(0x804d1a0)                                                                                                  = 0
putchar(32, 0x804a008, 0, 0)                                                                                     = 32
fgetc(0x804d1a0)                                                                                                 = 'a'
feof(0x804d1a0)                                                                                                  = 0
putchar(97, 0x804a008, 0, 0)                                                                                     = 97
fgetc(0x804d1a0)                                                                                                 = ' '
feof(0x804d1a0)                                                                                                  = 0
putchar(32, 0x804a008, 0, 0)                                                                                     = 32
fgetc(0x804d1a0)                                                                                                 = 't'
feof(0x804d1a0)                                                                                                  = 0
putchar(116, 0x804a008, 0, 0)                                                                                    = 116
fgetc(0x804d1a0)                                                                                                 = 'e'
feof(0x804d1a0)                                                                                                  = 0
putchar(101, 0x804a008, 0, 0)                                                                                    = 101
fgetc(0x804d1a0)                                                                                                 = 's'
feof(0x804d1a0)                                                                                                  = 0
putchar(115, 0x804a008, 0, 0)                                                                                    = 115
fgetc(0x804d1a0)                                                                                                 = 't'
feof(0x804d1a0)                                                                                                  = 0
putchar(116, 0x804a008, 0, 0)                                                                                    = 116
fgetc(0x804d1a0)                                                                                                 = '\n'
feof(0x804d1a0)                                                                                                  = 0
putchar(10, 0x804a008, 0, 0This is a test
)                                                                                     = 10
fgetc(0x804d1a0)                                                                                                 = '\377'
feof(0x804d1a0)                                                                                                  = 1
fclose(0x804d1a0)                                                                                                = 0
getuid()                                                                                                         = 12005
setuid(12005)                                                                                                    = 0
unlink("/tmp/file.log")                                                                                          = 0
+++ exited (status 0) +++
leviathan5@gibson:~$
```

The binary uses a function to open and read the file and then it removes it.  

In this level the binary takes care of all possible command injections, however, the vulnerability is not in how we can inject a command.

## 💣 Exploitation

In this case we can exploit the binary generating a linked file to the file that contains the password of Leviathan6. As we have permisions SUID as Leviathan 6, we can uses the file "file.log" as a linked file to the file that contains the password of Leviathan 6, that only Leviathan 6 can read.


