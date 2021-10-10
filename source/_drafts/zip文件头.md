---
title: zip文件头
tags:
---
Local file headers
{% asset_img local-file-header.png local-file-header %}
Each local file header has the following structure:

structure of a local file header

Signature	The signature of the local file header. This is always '\x50\x4b\x03\x04'.
Version	PKZip version needed to extract
Flags	General purpose bit flag:
Bit 00: encrypted file
Bit 01: compression option
Bit 02: compression option
Bit 03: data descriptor
Bit 04: enhanced deflation
Bit 05: compressed patched data
Bit 06: strong encryption
Bit 07-10: unused
Bit 11: language encoding
Bit 12: reserved
Bit 13: mask header values
Bit 14-15: reserved
Compression method	00: no compression
01: shrunk
02: reduced with compression factor 1
03: reduced with compression factor 2
04: reduced with compression factor 3
05: reduced with compression factor 4
06: imploded
07: reserved
08: deflated
09: enhanced deflated
10: PKWare DCL imploded
11: reserved
12: compressed using BZIP2
13: reserved
14: LZMA
15-17: reserved
18: compressed using IBM TERSE
19: IBM LZ77 z
98: PPMd version I, Rev 1
File modification time	stored in standard MS-DOS format:
Bits 00-04: seconds divided by 2
Bits 05-10: minute
Bits 11-15: hour
File modification date	stored in standard MS-DOS format:
Bits 00-04: day
Bits 05-08: month
Bits 09-15: years from 1980
Crc-32 checksum	value computed over file data by CRC-32 algorithm with 'magic number' 0xdebb20e3 (little endian)
Compressed size	if archive is in ZIP64 format, this filed is 0xffffffff and the length is stored in the extra field
Uncompressed size	if archive is in ZIP64 format, this filed is 0xffffffff and the length is stored in the extra field
File name length	the length of the file name field below
Extra field length	the length of the extra field below
File name	the name of the file including an optional relative path. All slashes in the path should be forward slashes '/'.
Extra field	Used to store additional information. The field consistes of a sequence of header and data pairs, where the header has a 2 byte identifier and a 2 byte data size field.
Example
Our sample zip file starts with a local file header:

00000000  50 4b 03 04 14 00 00 00  08 00 1c 7d 4b 35 a6 e1  |PK.........}K5..|
00000010  90 7d 45 00 00 00 4a 00  00 00 05 00 15 00 66 69  |.}E...J.......fi|
00000020  6c 65 31 55 54 09 00 03  c7 48 2d 45 c7 48 2d 45  |le1UT....H-E.H-E|
00000030  55 78 04 00 f5 01 f5 01  0b c9 c8 2c 56 00 a2 92  |Ux.........,V...|
This results in the following fields and field values:

Example: first local file header

Signature	'\x50\x4b\x03\x04'.
Version	0x14 = 20 -> 2.0
Flags	no flags
Compression method	08: deflated
File modification time	0x7d1c = 0111110100011100
hour = (01111)10100011100 = 15
minute = 01111(101000)11100 = 40
second = 01111101000(11100) = 28 = 56 seconds
15:40:56
File modification date	0x354b = 0011010101001011
year = (0011010)101001011 = 26
month = 0011010(1010)01011 = 10
day = 00110101010(01011) = 11
10/11/2006
Crc-32 checksum	0x7d90e1a6
Compressed size	0x45 = 69 bytes
Uncompressed size	0x4a = 74 bytes
File name length	5 bytes
Extra field length	21 bytes
File name	"file1"
Extra field	id 0x5455: extended timestamp, size: 9 bytes
Id 0x7855: Info-ZIP UNIX, size: 4 bytes
Data descriptor