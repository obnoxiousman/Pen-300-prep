
## Naming Convention
One important thing to note is that shared libraries in Linux use the [_soname_](https://en.wikipedia.org/wiki/Soname) naming convention. This is typically something like lib(libraryname).so, which may also include a version number appended to the end with a period or full-stop character. 
For example, we might see lib(libraryname).so.1. Naming our libraries following this convention will help us with the linking process.

