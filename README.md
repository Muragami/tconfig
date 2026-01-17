A basic INI reader/writer built in C, with minimal dependencies (POSIX clib implementation)

This was an attempt to write something compact and easy to use for reading
and writing INI files in my projects.

Since it is meant for configuration, it is not optimized in any way, as it is
not meant to work on large, complex files. If you need that, look elsewhere.

It's able to read most INI formats without any issues.  Comments are delimited by
the ';' or '#' character. Keys can be delimited by either '=' or ':'. There is no
support for escaping these characters in keys or values. That is beyond the scope
of this simple implementation.

A basic usage example would be something like this:
```c
int main()
{
    ini_table_s* config = ini_table_create();
    if (!ini_table_read_from_file(config, "test.ini")) {
        puts("test.ini does not exist! Adding entries!");
        ini_table_create_entry(config, "Section", "one", "two");
        ini_table_write_to_file(config, "test.ini");
    }else {
        puts("Entry one is: %s\n", ini_table_get_entry(config, "Section", "one"));
    }
    ini_table_destroy(config);
    return 0;
}
```

You don't need to check for NULL on ini_table_create(), since it creates an empty
structure.  Always make sure to call ini_table_destroy() when you're finished
using the data, however.

A recent PR allowed reading/writing of comment lines.

Added dead simple error handling via a callback and a configurable exit at error flag.

Added the ability to read values as floating point.

Added the ability to use custom io routines, call ini_table_read() and ini_table_write() with your in/out
struct.

Added the ability to use a callback for parsing, call ini_read() with the IO struct and callback struct.

For more information on these functions, check out tconfig.h, which I spent a bit of time documenting for easy reading.

