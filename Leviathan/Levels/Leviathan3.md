# Leviathan Level 3

You can watch the walkthrough for this level here:  
[![YouTube](https://img.shields.io/badge/YouTube-Walkthrough-red?logo=youtube)](https://www.youtube.com/watch?v=YcL3ShoNBKs)

> This video shows my full process solving (in Spanish) Level 3 from scratch, including the obstacles and mistakes I faced along the way. Some walkthroughs might be longer or shorter depending on the complexity of the level or how quickly I find the solution.

---

## 🔍 Exploration

We begin by inspecting the current directory and the discovered binary:

```bash
leviathan3@gibson:~$ ls -l
total 20
-r-sr-x--- 1 leviathan4 leviathan3 18100 Apr 10 14:23 level3
leviathan3@gibson:~$ file level3 
level3: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=5f6860c1ad35cc89c2397be07b3684c8b6222d84, for GNU/Linux 3.2.0, with debug_info, not stripped
```

The binary has SUID permissions and prompts for a password:

```bash
leviathan3@gibson:~$ ./level3 
Enter the password> test
bzzzzzzzzap. WRONG
```

We check for readable strings:

```bash
leviathan3@gibson:~$ strings level3 
tdL 
/lib/ld-linux.so.2
_IO_stdin_used
fgets
stdin
puts
__stack_chk_fail
system
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
snlp
rint
bomb
...s
3cr3f
h0no
kaka
*32.
2*[xf
[You've got shell]!
/bin/sh
bzzzzzzzzap. WRONG
Enter the password> 
;*2$" 
secret
GCC: (Ubuntu 13.3.0-6ubuntu2~24.04) 13.3.0
u`	l
printf
__off_t
_IO_read_ptr
_chain
strcmp
do_stuff
size_t
_shortbuf
_IO_buf_base
long long unsigned int
__int64_t
long long int
other
_fileno
_IO_read_end
_flags
_IO_buf_end
_cur_column
_IO_codecvt
_old_offset
geteuid
_IO_marker
stdin
GNU C17 13.3.0 -m32 -mtune=generic -march=i686 -g -fno-pic -fasynchronous-unwind-tables -fstack-protector-strong -fstack-clash-protection
_IO_write_ptr
short unsigned int
_IO_save_base
kaka
_lock
_flags2
_mode
fgets
_IO_write_end
kaka2
_IO_lock_t
_IO_FILE
_markers
unsigned char
short int
_IO_wide_data
asdf
_vtable_offset
setreuid
__off64_t
_IO_read_base
_IO_save_end
__pad5
_unused2
morenothing
_freeres_buf
_IO_backup_base
system
_freeres_list
main
_IO_write_base
/root/otw-games/game-leviathan/levels/leviathan3
level3.c
/usr/lib/gcc/x86_64-linux-gnu/13/include
/usr/include/bits
/usr/include/bits/types
/usr/include
stddef.h
types.h
struct_FILE.h
stdio.h
stdlib.h
string.h
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
level3.c
__FRAME_END__
_DYNAMIC
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
strcmp@GLIBC_2.0
__libc_start_main@GLIBC_2.34
__x86.get_pc_thunk.bx
printf@GLIBC_2.0
fgets@GLIBC_2.0
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
stdin@GLIBC_2.0
_end
_dl_relocate_static_pie
_fp_hw
nothing
__bss_start
__TMC_END__
do_stuff
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
.debug_aranges
.debug_info
.debug_abbrev
.debug_line
.debug_str
.debug_line_str
```
Notable strings:

- ✅ /bin/sh and [You've got shell]! — suggests that a shell is spawned upon correct password.

```bash
[You've got shell]!
/bin/sh
bzzzzzzzzap. WRONG
Enter the password> 
;*2$" 
secret
```
We trace the program (with `ltrace`) to see how it behaves internally:

```bash
leviathan3@gibson:~$ ltrace ./level3 
__libc_start_main(0x80490ed, 1, 0xffffd394, 0 <unfinished ...>
strcmp("h0no33", "kakaka")                                                                                       = -1
printf("Enter the password> ")                                                                                   = 20
fgets(Enter the password> test
"test\n", 256, 0xf7fae5c0)                                                                                 = 0xffffd16c
strcmp("test\n", "snlprintf\n")                                                                                  = 1
puts("bzzzzzzzzap. WRONG"bzzzzzzzzap. WRONG
)                                                                                       = 19
+++ exited (status 0) +++
leviathan3@gibson:~$ 
```

📌 Here we can clearly see that the password is being compared against the hardcoded string "snlprintf\n".

## 💣 Exploitation

Now that we know the password, we can enter it to spawn a shell as leviathan4:

```bash
leviathan3@gibson:~$ ./level3
Enter the password> snlprintf
[You've got shell]!
$ whoami
leviathan4
$ cd /etc/leviathan_pass/leviathan4
/bin/sh: 2: cd: can't cd to /etc/leviathan_pass/leviathan4
$ cat /etc/leviathan_pass/leviathan4
WG1egElCvO
```

## 🔐 Password for Leviathan 4
WG1egElCvO
