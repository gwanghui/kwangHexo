---
title: linux-command-ls
date: 2020-06-22 08:53:45
tags:
---

### ls 명령어
      For each directory that is listed, preface the files with a line
      'total BLOCKS', where BLOCKS is the total disk allocation for all
      files in that directory.  The block size currently defaults to 1024
      bytes, but this can be overridden (*note Block size::).  The BLOCKS
      computed counts each hard link separately; this is arguably a
      deficiency.
 
      The file type is one of the following characters:
 
      '-'
           regular file
      'b'
           block special file
      'c'
           character special file
      'C'
           high performance ("contiguous data") file
      'd'
           directory
      'D'
           door (Solaris 2.5 and up)
      'l'
           symbolic link
      'M'
           off-line ("migrated") file (Cray DMF)
      'n'
           network special file (HP-UX)
      'p'
           FIFO (named pipe)
      'P'
           port (Solaris 10 and up)
      's'
           socket
      '?'
           some other file type
 
      The file mode bits listed are similar to symbolic mode
      specifications (*note Symbolic Modes::).  But 'ls' combines
      multiple bits into the third character of each set of permissions
      as follows:
 
      's'
           If the set-user-ID or set-group-ID bit and the corresponding
           executable bit are both set.
 
      'S'
           If the set-user-ID or set-group-ID bit is set but the
           corresponding executable bit is not set.
 
      't'
           If the restricted deletion flag or sticky bit, and the
           other-executable bit, are both set.  The restricted deletion
           flag is another name for the sticky bit.  *Note Mode
           Structure::.
 
      'T'
           If the restricted deletion flag or sticky bit is set but the
           other-executable bit is not set.
 
      'x'
           If the executable bit is set and none of the above apply.
 
      '-'
           Otherwise.
           
### File Type
- Regular File 
    - The most common file, which contains data of some form.
    
- Directory File
    - A file contains the names of other files and pointers to information on these files.
    - Any process that has read permission for a directory file can read contents of the directory, but only the kernel can write directly to a directory file.

- Block Special file
    - 요청이 장치 드라이버에 의해 처리되며 이러한 유형을 디바이스 파일이라고 한다. 
    - 디바이스 파일은 어떤 드라이버에 의해서 사용되는지 구분하기 위한 연관번호를 가지고 있다.
    - 블록 특수 파일은 일반 파일과 유사하고 문자 특수 파일은 파이프처럼 행동합니다.
    
- Character special file
    - A type of file providing unbuffered I/O access in variable-sized units to devices.
    - 이 장치파일은 데이터를 쓰거나 읽는게 즉시 일어나는 파이프나 시리얼 포트처럼 행동한다.
    
- FIFO
    - Process Inter communication 

- Socket
    - 네트워크의 입출력을 담당하는 API
    
- Symbolic link
    - A Type of file that points to another file 
    - 다른 파일을 가리키는 타입의 파일
    
![](/images/linux/count_of_filetype.png)
    