
# SVFS
*(Quoting from the original project [README](https://pypi.org/project/SVFS/)):*

SVFS allows to create virtual filesystem inside file on real filesystem. It can be used to store multiple files inside single file (with directory structure). Unlike archives, SVFS allows to modify files in-place. SVFS files use file-like interface, so they can be used (pretty much) like regular Python file objects. Finally, it’s implemented in pure python and doesn’t use any 3rd party modules, so it should be very portable. Tests show write speed to be around 10-12 MB/s and read speed to be around 26-28 MB/s.

## Example

Following code creates SVFS, opens file in it, writes string and reads it back:

from SVFS import SVFS #Import SVFS class

s = SVFS() # Create instance of SVFS class
s.CreateSVFS('test.svfs','testvolume',100,100,100) # Create SVFS with 100 inodes and 100 blocks of 100 bytes.
s.OpenSVFS('test.svfs') # Open created SVFS
with s.open('testfile','w') as file: # Create and open new file for writing in SVFS.
    file.write('write test') # Write string into file
t = s.open('testfile','r') # Open same file for reading
print(t.read()) # Read entire file and print data
t.close() # Close file
s.CloseSVFS() # Close SVFS

That code outputs:

    write test

Which means it works correctly.

## Updates to v3:

SVFS now distinguishes between strings/bytes and will transparently encode/decode:

    # Open up a file in text mode. 
    with s.open('text_file','w', encoding='utf8') as text_file: 
        # If no encoding is given, defaults to 'utf-8'
        text_file.write("""This is a\nmultiline\ntext file""") # Write string into file, encoded as utf-8

    t_file = s.open('text_file','r') # Open same file for reading
    t_file.readlines()

This code outputs:

    ['This is a\n', 'multiline\n', 'text file']

Binary files work similarly:

    # Open up a file in binary mode. 
    with s.open('binary_file','wb') as binary_file: 
        # Passing 'b' as part of the mode parameter sets binary mode
        binary_file.write(b'Some bytes')
    
    b_file = s.open('binary_file', 'rb')
    b_file.read()

Outputs:

    b'Some bytes'
