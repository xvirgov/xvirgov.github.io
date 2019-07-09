# Elftools + ipython3

```console
$ ipython3
Python 3.5.3 (default, Sep 27 2018, 17:25:39) 
Type "copyright", "credits" or "license" for more information.

IPython 5.1.0 -- An enhanced Interactive Python.
?         -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help      -> Python's own help system.
object?   -> Details about 'object', use 'object??' for extra details.
```

```python
In [1]: from elftools.elf import elffile

In [2]: f = open("/usr/lib/debug/vmlinux-4.9.0-9-amd64","rb")

In [3]: e = elffile.ELFFile(f)
```

```python
In [4]: e.get_section_by_name(".symtab")
Out[4]: <elftools.elf.sections.SymbolTableSection at 0x7f3bf1b64550>

In [5]: s = e.get_section_by_name(".symtab")

In [6]: s.get_symbol_by_name("linux_banner")
Out[6]: [<elftools.elf.sections.Symbol at 0x7f3bf1b88048>]
```

```python
In [7]: banner = s.get_symbol_by_name("linux_banner")

In [8]: banner
Out[8]: [<elftools.elf.sections.Symbol at 0x7f3bf38ef2e8>]

In [9]: banner[0]
Out[9]: <elftools.elf.sections.Symbol at 0x7f3bf38ef2e8>

In [10]: banner = banner[0]

In [11]: banner.name
Out[11]: 'linux_banner'

In [12]: banner.entry
Out[12]: Container({'st_size': 161, 'st_value': 18446744071587234016, 'st_other': Container({'visibility': 'STV_DEFAULT'}), 'st_name': 1466726, 'st_shndx': 7, 'st_info': Container({'bind': 'STB_GLOBAL', 'type': 'STT_OBJECT'})})
```
```python
In [13]: banner.entry['st_value']
Out[13]: 18446744071587234016

In [14]: hex(banner.entry['st_value'])
Out[14]: '0xffffffff818000e0'
```
```python
In [15]: ro = e.get_section_by_name('.rodata')

In [16]: ro.header
Out[16]: Container({'sh_info': 0, 'sh_addr': 18446744071587233792, 'sh_flags': 3, 'sh_link': 0, 'sh_type': 'SHT_PROGBITS', 'sh_size': 2573074, 'sh_name': 71, 'sh_addralign': 4096, 'sh_entsize': 0, 'sh_offset': 10485760})

In [17]: hex(ro.header['sh_addr'])
Out[17]: '0xffffffff81800000'
```
```python
In [18]: f.seek(ro.header['sh_offset'] + 0xe0)
Out[18]: 10485984

In [19]: f.read(20)
Out[19]: b'Linux version 4.9.0-'
```
```python
In [20]: e.get_section_by_name('.text')
Out[20]: <elftools.elf.sections.Section at 0x7f3bf0a890f0>

In [21]: hex(e.get_section_by_name('.text').header['sh_addr'])
Out[21]: '0xffffffff81000000'
```


