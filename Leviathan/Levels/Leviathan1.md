# Leviathan Level 1 → Level 2

The first point is to explore the current directory:
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
There is a file called `check`. We can inspect this file:
```bash
leviathan1@gibson:~$ file check
check: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=115df4ab9cca6c946a5c068b6c9c103f38a6e73b, for GNU/Linux 3.2.0, not stripped
```
**Key observations:**
- It is an executable.
- It has SUID permissions (important because it allows execution as the file owner, in this case, Leviathan2)
- It is dynamically linked, meaning it loads libraries during execution.

Now that we have an initial understanding of the situation, we can explore what this executable does.
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
We can see that the executable prompts for a password. If the input is incorrect, it returns `Wrong password, Good Bye ...`

At this point, we can check for readable content whithin the file:
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

### Interesting findings:

```bash
password: 
/bin/sh
Wrong password, Good Bye ...
;*2$"0
```
This suggests that after entering the correct password, the binary executes a shell. Since it has SUID permissions, the shell will be opened as Leviathan2.

Additionally, we see:
```bash
printf
setreuid
strcmp
geteuid
```
Among these, `strcmp` is particularly interesting. In C, strcmp() is a built-in library function used to compare two strings lexicographically. It takes two strings (array of characters) as arguments, compares these two strings lexicographically, and then returns 0,1, or -1 as the result.

Since the program is dynamically linked, it loads libraries during execution. We can use `ltrace` to analyze its behavior when an incorrect password is entered:
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

We see that the program compares only the first three characters of the input with the correct password (`sex`). We know the password right now. However, you can exploit this by entering a longer word that begins with sex, which will still be accepted.
```bash
leviathan1@gibson:~$ ./check
password: sexologie

$ whoami
leviathan2
$ cat /etc/leviathan_pass/leviathan2
NsN1HwFoyN
```
By entering `sexologie`, we gain shell access as Leviathan2. From here, we can retrieve the password for the next level.

## Password for next level

NsN1HwFoyN

