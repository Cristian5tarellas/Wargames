# Leviathan Level 1 → Level 2


You can watch the walkthrough for this level here:  
[![YouTube](https://img.shields.io/badge/YouTube-Walkthrough-red?logo=youtube)](https://www.youtube.com/watch?v=6v0Bb1ufI-8)

> This video shows my full process solving (in Spanish) Level 1 from scratch, including the obstacles and mistakes I faced along the way. Some walkthroughs might be longer or shorter depending on the complexity of the level or how quickly I find the solution.

---

## 🔍 Solution

We begin by inspecting the current directory:

```bash
leviathan1@gibson:~$ ll
total 36
drwxr-xr-x  2 root       root        4096 Sep 19 07:07 ./
drwxr-xr-x 83 root       root        4096 Sep 19 07:09 ../
-rw-r--r--  1 root       root         220 Mar 31  2024 .bash_logout
-rw-r--r--  1 root       root        3771 Mar 31  2024 .bashrc
-r-sr-x---  1 leviathan2 leviathan1 15080 Sep 19 07:07 check*
-rw-r--r--  1 root       root         807 Mar 31  2024 .profile
```
We find a file named `check`. Let's inspect its properties:

```bash
leviathan1@gibson:~$ file check
check: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=115df4ab9cca6c946a5c068b6c9c103f38a6e73b, for GNU/Linux 3.2.0, not stripped
```
**Key observations:**
- ✅ It's an executable binary.  
- 🔐 It has **SUID permissions** (runs with `leviathan2` privileges).  
- 🔗 It's dynamically linked.

---

### ▶️ Testing the Binary

When executed, the program asks for a password:

```bash
leviathan1@gibson:~$ ./check 
password: Halo
Wrong password, Good Bye ...
leviathan1@gibson:~$ ./check 
password: 


Wrong password, Good Bye ...
leviathan1@gibson:~$ ./check 
password: d
l
Wrong password, Good Bye ...
leviathan1@gibson:~$ 
```
Let's look for readable strings inside:

```bash
leviathan1@gibson:~$ strings check
td8 
/lib/ld-linux.so.2
_IO_stdin_used
puts
__stack_chk_fail
system
getchar
__libc_start_main
printf
setreuid
strcmp
geteuid
libc.so.6
GLIBC_2.4
GLIBC_2.34
GLIBC_2.0
__gmon_start__
secr
love
password: 
/bin/sh
Wrong password, Good Bye ...
;*2$"0
GCC: (Ubuntu 13.2.0-23ubuntu4) 13.2.0
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
check.c
__FRAME_END__
_DYNAMIC
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
strcmp@GLIBC_2.0
__libc_start_main@GLIBC_2.34
__x86.get_pc_thunk.bx
printf@GLIBC_2.0
getchar@GLIBC_2.0
_edata
leviathan1@gibson:~$ strings check
td8 
/lib/ld-linux.so.2
_IO_stdin_used
puts
__stack_chk_fail
system
getchar
__libc_start_main
printf
setreuid
strcmp
geteuid
libc.so.6
GLIBC_2.4
GLIBC_2.34
GLIBC_2.0
__gmon_start__
secr
love
password: 
/bin/sh
Wrong password, Good Bye ...
;*2$"0
GCC: (Ubuntu 13.2.0-23ubuntu4) 13.2.0
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
check.c
__FRAME_END__
_DYNAMIC
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
strcmp@GLIBC_2.0
__libc_start_main@GLIBC_2.34
__x86.get_pc_thunk.bx
printf@GLIBC_2.0
getchar@GLIBC_2.0
_edata
_fini
__stack_chk_fail@GLIBC_2.4
geteuid@GLIBC_2.0
__data_start
puts@GLIBC_2.0
system@GLIBC_2.0
__gmon_start__
__dso_handle
_IO_stdin_used
setreuid@GLIBC_2.0
_end
_dl_relocate_static_pie
_fp_hw
__bss_start
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

We see:
- The prompt `password:`  
- The string `/bin/sh`, suggesting it spawns a shell  
- Use of `strcmp`, meaning it compares your input to a hardcoded string  

Since it's dynamically linked, we can trace the library calls:

```bash
leviathan1@gibson:~$ ltrace ./check 
__libc_start_main(0x80490ed, 1, 0xffffd394, 0 <unfinished ...>
printf("password: ")                                                                                             = 10
getchar(0, 0, 0x786573, 0x646f67password: Nobody
)                                                                                = 78
getchar(0, 78, 0x786573, 0x646f67)                                                                               = 111
getchar(0, 0x6f4e, 0x786573, 0x646f67)                                                                           = 98
strcmp("Nob", "sex")                                                                                             = -1
puts("Wrong password, Good Bye ..."Wrong password, Good Bye ...
)                                                                             = 29
+++ exited (status 0) +++
```

🧠 This shows the program only compares the **first 3 characters** with the string `"sex"`.

---

### 🛠️ Exploiting the Program

We enter any word starting with `"sex"`, for example:

```bash
leviathan1@gibson:~$ ./check
password: sexologie

$ whoami
leviathan2
$ cat /etc/leviathan_pass/leviathan2
NsN1HwFoyN
```
By entering `sexologie`, we gain shell access as Leviathan2. From here, we can retrieve the password for the next level.

## Password for Leviathan 2

NsN1HwFoyN

