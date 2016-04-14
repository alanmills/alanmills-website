View the contents of a file in a zip file from the Terminal
===========================================================
**Author:** Alan Mills  
**Date:** [9 July 2015 23:36](/blog/history/2015-07.md)  
**Tags:** [OS X Mavericks](/blog/categories/osx-10-10.md), [bash](/blog/categories/bash.md), [zip](/blog/categories/zip.md), [Pluralsight](/blog/categories/pluralsight.md)   
**Status**: Draft

Often I am working through [Pluralsight training courses](http://www.pluralsight.com) and I need to view a source file from the course exercise files.

Pluralsight provides the exercise files as a zip archive.  By default the Mac Finder does not allow you to view an individual file in a zip archive without inflating the whole file.

There is a way to view the contents of a zip and individual file from the Terminal.

List the contents of a Zip file
-------------------------------
The following command will output the listing of files in the zip archive.  
```bash
unzip -l archive.zip
```

Example listing of files in the archive.zip
```bash
Length    Date    Time    Name
------    ----    ----    ----
     0  09-07-15  23:36   demos/
 38615  09-07-15  23:36   demos/ExampleFile.js
   927  09-07-15  23:36   demos/ExamplePage.html
------                    -------
 39542                    3 files
```

View the content of a file from the zip file
--------------------------------------------
To view the contents of the *ExampleFile.js* file without unzipping the archive, run the following command:
```bash
unzip -c archive.zip demos/ExampleFile.js
```

The output of the file will be presented in the Terminal.

```bash
Archive: archive.zip
  inflating: demos/ExampleFile.js
function helloWorld() {
  console.log('hello, World!');
}
```
