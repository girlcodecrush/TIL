1. The input text object refers to an HTML <input type="text">
2. Syntax: element.setAttribute(attributename, attributevalue)
   - attributename, attributevalue need to be in string type
3. Create an input text object

```
funtion createInputBox(){
    var ipt = document.createElement("INPUT");
    ipt.setAttribute('type', 'text');
    ipt.setAttribute('value', 'Hooray!');
    document.body.appendChild(ipt);
}
```

4. document.write()

​     -used to write some text to an output stream - p, div, etc.

```
document.write("<h1>Hello World!</h1><p>Have a nice day!</p>);
```

