Syntax & Usage:

a. Return the value property:

```
textObject.value

e.g.
<!DOCTYPE html>
<html>
<body>

First Name: <input type = "text" id="myText" value="Mickey">

<button onclick="fn()">Click</button>
<p id="demo"><p>

<script>
function fn(){
    var x = document.getElementById("myText").value;
    document.getElementById("demo").innerHTML = x;
}
</script>
</body>
</html>
```

​       b.set the value property:

```
textObject.value = text

<!DOCTYPE html>
<html>
<body>
Name: <input type="text id="myText" value="Mickey">
<button onclick = "fn()">CLICK</button>

<script>
function fn(){
   document.getElementById('myText').value = "Alan Adams";  
}   //reset the value of the input field by replacing Mickey with Alan Adams
</script>

</body>
</html>
```

