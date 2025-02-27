## ntldd ##

This small utility will do the one thing that most people want from ldd - it
will give you a list of libraries a library or executable depends upon.

Pros:

 * Knows about delayed loading.
 * Can list dependencies recursively (like depends.exe does, but without duplicating entries)
 * Free software
 * Output tries to mimic ldd (might be usable as a drop-in replacement for ldd)

Cons:

 * Is likely buggy and might fail spectacularly on uncommon PE files (especially created by toolsets other than MSVC or GCC)
 * Might not work on Windows CE or in relatively uncommon environments
 * Does not have any advanced features of ldd (most options do not work)
 * Does not mimic ldd completely
 
Run cmake to compile.

 * tested with Visual Studio and Ninja generators

Requires GCC and win32api MinGW packages.
