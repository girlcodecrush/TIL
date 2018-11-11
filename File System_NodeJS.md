data transfer and save using File System, NodeJS

I. initiaate : var fs = require('fs');

II. Read files 

   fs.readFile() 

    - html file in the Nodejs folder 
- creat a Nodejs file that read the html file and return the content

```
var http = require('http');
var fs = require('fs');
http.createServer(function (req, res) {
  fs.readFile('demofile1.html', function(err, data) {
    res.writeHead(200, {'Content-Type': 'text/html'});
    res.write(data);
    res.end();
  });
}).listen(8080);
*8080 is the port address through which you can access your localhost’s dashboard.
```

III. Create files

- fs.appendFile() : append specified content to a file 

```
var fs = require('fs');

fs.appendFile('mynewfile1.txt', 'Hello content!', function (err) {
  if (err) throw err;
  console.log('Saved!');
});
```

b.fs.open()

- Flag -  the 2nd arg
- if the flag is w - a file is open for writing
- If the file doens't exist, an empty file is created

```
var fs = require('fs');

fs.open('mynewfile2.txt', 'w', function (err, file) {
  if (err) throw err;
  console.log('Saved!');
});
```

c. fs.writeFile()

- replaces the specific file and content
- a new file that contains any specific content to be creaed if the file doesn't exist

```
var fs = require('fs');

fs.writeFile('mynewfile3.txt', 'Hello content!', function (err) {
  if (err) throw err;
  console.log('Saved!');
});
```

Delete Files

fs.unlink()

```
var fs = require('fs');

fs.unlink('mynewfile2.txt', function (err) {
  if (err) throw err;
  console.log('File deleted!');
});
```

Rename Files

fs.rename()

```
var fs = require('fs');

fs.rename('mynewfile1.txt', 'myrenamedfile.txt', function (err) {
  if (err) throw err;
  console.log('File Renamed!');
});
```





