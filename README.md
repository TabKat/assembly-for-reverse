# Assembly 64 for reverse

## Accessing memory
**.data** - initialized data segment \
**.bss** - uninitialized data segment (all data in **.bss** after program reload will be erased) 

Record data in the **.data** segment

```asm
mov rcx, qword ptr ds:[0x00000000004030E0]
mov qword ptr ds:[0x00000000004030F0], rcx
```