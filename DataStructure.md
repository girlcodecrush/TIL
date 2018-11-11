HashTable

- stores key value pairs, 'like' an object

- uses a hashing function that translates keys into numerical indices

- similar to the contiguous blocks of memory used in arrays

- a hash function is like a row of boxes

- each box has a number

- data is stored in the box with the number -> when the data value is put back in the hash function and it spits out the same number as before so you can go get your data

- the way a hash table stores data is like an array of objects

- each data box is an index inside the array and is an object that stores the data

- each piece of data that is inputed is a key and goes into a hash function

- bucket - each of the data storage boxes

- tuple - the data inside the boxes 

  

![](/Users/andreakim/Desktop/HashTable.png)

- the numbers 0 - 11 are the index numbers of the buckets
- the key-value boxes are the tuples inside each bucket
- the key is the tuple's location in the bucket
- the value is the data being stored in the bucket

```
[PART I]
//limitedArray stores all inserted values 
//it utilizes 3 methods - .get() ; .set() ; .each()

var LimitedArray = function(limit){
    var storage = [];   
    
    var limitedArray = {};
    //compile all data inseted in the storage called limitedArray as if data sets are 
    //the key-value pairs, a piece of data -> data = storage[index]
    //hence, storage[index] can be the 1st pair, 2nd pair,... 
    //storage[index] does NOT mean the value in each key-value pair
    
    limitedArray.get = function(index) {
       checkLimit(index);
       return storage[index];
    } 
};

//data positions somewhere in each of the indexed storages, called bucket
limitedArray.set = function(index, value){
  checkLimit(index);
  storage[index] = value;  
  //here, value means the entire key-value pair, that's, a piece of data
};
//callback function iterates over each item of the storage which is assumed as an array 
//iterator(item, index, the entire array)
limitedArray.each = function(callback){
  for(var i = 0 ; i < storage.length; i++){
    callback(storage[i], i, storage)
  }
}; 

var checkLimit = function(index){
    if(typeof index !== 'number'){
      throw new Error('setter requires a numeric index for its first argument');
    }
    if (limit <= index){
      throw new Error('Erro trying to access an over-the-limit index')
    }
};
  return limitedArray;
};

[PART II]
//hash table uses 3 properties - .insert() ; .retrieve() ; .remove() --> 
//HashTable.prototype.insert ; HashTable.prototype.retrieve ; HashTable.prototype.remove
//when using the properties above, relate limitedArray.get() & limitedArray.set() to each property

var HashTable = function() {
    this._limit = 8 ;
    this._storage = LimitedArray(this._limit)
}

HashTable.prototype.insert = function(k, v){
  var index = getIndexBelowMaxForKey(k, this._limit)
  var bucket = this._storage.get(index)
  
  //if bucket(a storage at index 0) doesn't exist, make an empty bucket and push the given //k-v pair of data into the empty bucket --> <points to remember>1. set the index of the //new bucket and value(the first pair of data within the bucket) ;2. when retrieving, //what we need is v(data value, not key)
  
  if(!bucket) {
    var newBucket = []
    newBucket.push([k, v])
    this._storage.set(index, newBucket)
  }else {
   if (bucket.length > 0){
     for (var i = 0; i < bucket.length; i++){
       if (bucket[i][0] === k){
           bucket[i][1] = v
       } else {
         bucket.push([k, v])
       }  
     }
   }
  }
}

//retrieve the data value of each k-v pair 
HashTable.prototype.retrieve = function(k){
    var index = getIndexBelowMaxForKey(k, this._limit)
    var bucket = this._storage.get(index)
    if(bucket.length === 1){
      if(bucket[0][0]===k){
        return bucket[0][1]
      }
    }
    if(bucket.length > 1){
      for (var i = 0 ; i < bucket.length; i++){
          if(bucket[i][0]=== k){
              return bucket[i][1]
          }
      }  
    }
}
//delete data value inserted previously, then return the value deleted as 
HashTable.prototype.remove = function(k) {
  var index = getIndexBelowMaxForKey(k, this._limit)
  var bucket = this._storage.get(index)
  var dataInserted
  if(bucket.length === 1){
    if(bucket[0][0]=== k){
      dataInserted = bucket[0][1]
      bucket.splice(0, 1)
      return dataInserted
    }
  }
  if(bucket.length > 1){
    for(var i = 0; i < bucket.lengthl i++){
      if(bucket[i][0] === k){
        dataInserted = bucket[i][1]
        bucket.splice(i, 1)
        return dataInserted
      }
    }
  }
}
```



Singly-Linked & Doubly-Linked List 

- a data structure that holds a sequence of linked nodes

- each node contains data and a pointer, which can point to another node

- each node can be a separate constructor from a singly-linked list 

- not contiguous in memory

- Node : 1. Data - stores a value  ;2. Next - points to the next node in the list

  

![](/Users/andreakim/Desktop/Screen Shot 2018-09-24 at 4.36.24 PM.png)

```
var LinkedList = function(){
    var list = {}
    //make head and tail properties; the initial value is set to null since we assume that there are no nodes 
    list.head = null
    list.tail = null
} 
//creating the first node 
list.addTail = function(value){
//a variable, newNode, is given to assign data(value) to head and tail
  let newNode = Node(value)  
  if(list.head === null){
    list.head = newNode
    list.tail = newNode
    //if there is a first node with a value in the head position
  }else {
     list.tail.next = newNode 
     list.tail = newNode
  }
}
//deleting the value in head
list.removeHead = function(){
  let shift = list.head.value 
  delete list.head     //as list is an object, using delete, can remove the key-value pair 
  list.head = list.tail //after head being removed, tail shifts to the head position
  return shift 
}

list.contains = function(target){
  if(list.head.value === target || list.tail.value = target|| list.tail.next = target) return true
  return false
}

var Node = function(value){
    var node = {}     //node has value and next properties 
    node.value = value
    node.next = null
    return node
}
```

TREE

- like DOM, hierarchical data structure 

- nodes with children, children's children, and so on ...  --> recursive data structure 

- Property  

   .children - an array containing a number of subtrees

- methods 

  .addChild() - takes any value, sets it to the target of a node - > add the node as a child of the tree

  .contains() - returns a boolean that shows if an input is found as the value of the target node



GRAPH

- consists of nodes(vertices) and edges(arcs) that connect the nodes

- any two nodes connected by an edge is symmetrical

- properties - nodes, edges

- Methods 

  .addNode() - add a new node to the graph

  .contains() - returns a boolean that shows if a certain node is in the graph

  .removeNode() - deletes a certain node from the graph; upon a removal of the node, all edges connected to the Node are removed as well

  .addEdge() - creates an edge(connection) between the two nodes

  .hasEdge() - boolean to show if two nodes are connected

  .removeEdge() - removes the connection between the two nodes

  .forEachNode() - call a passed in function once on each node, traversing

```
// Instantiate a new graph
//make nodeList and edges properties of the Graph object with this refering to Graph
var Graph = function () {
  this.nodeList = []
  this.edges = []
}

// Add a node to the graph, passing in the node's value.
Graph.prototype.addNode = function (node) {
  this.nodeList[node] = node
}

// Return a boolean value indicating if the value passed to contains is represented in the graph.
Graph.prototype.contains = function (node) {
  if (this.nodeList.includes(node)) return true
  return false
}
// Removes a node from the graph.
Graph.prototype.removeNode = function (node) {
  this.nodeList.splice(node, 1)
  //since an edge connects two nodes, taking an edge with the nodes(vertices) and values
  //that the nodes hold, as a unit at a certain index in the array edges, an edge at an          //index i has a node(vertex) at an index 0 and another at an index 1
  for (var i = 0; i < this.edges.length; i++) {
    if (this.edges[i][0] === node || this.edges[i][1] === node) {
      this.edges.splice(i, 1)
    }
  }
}
// Returns a boolean indicating whether two specified nodes are connected.Pass in the values contained in each of the two nodes.
Graph.prototype.hasEdge = function (fromNode, toNode) {
  var case1, case2
  for (var i = 0; i < this.edges.length; i++) {
    case1 = this.edges[i][0] === fromNode && this.edges[i][1] === toNode
    case2 = this.edges[i][0] === toNode && this.edges[i][1] === fromNode
    if (case1 || case2) return true
  }
  return false
}
// Connects two nodes in a graph by adding an edge between them
Graph.prototype.addEdge = function (fromNode, toNode) {
  this.edges.push([fromNode, toNode])
}
// Remove an edge between any two specified (by value) nodes.
Graph.prototype.removeEdge = function (fromNode, toNode) {
  var case1, case2
  for (var i = 0; i < this.edges.length; i++) {
    case1 = this.edges[i][0] === fromNode && this.edges[i][1] === toNode
    case2 = this.edges[i][0] === toNode && this.edges[i][1] === fromNode
    if (case1 || case2) {
      this.edges.splice(i, 1)
    }
  }
}
// Pass in a callback which will be executed on each node of the graph.
Graph.prototype.forEachNode = function (cb) {
  this.nodeList.forEach((node) => cb(node))
}

```

BINARY Search Tree

- Like Tree structure, recursive

- when two nodes position below their upper node, the less value goes to the left-hand side with the greater to the right-hand side

- results in particularly fast find operations

- one of the best uses - a dictionary 

- properties

  .left

  .right

- methods

  .insert() - accepts a value and places in the tree structrue by the rules above (right - greater/higher value ; left - lower/less value)

  .contains() - returns a boolean showing if the value is in the tree

  .depthFirstLog() -accepts a callback and executes it on every value

- use case

  Given a list of a million numbers, write a function that takes a new number and returns the closest number in the list using your BST. Profile this against the same operation using an array.

```
var BinarySearchTree = function (value) {
  var newTree = {}
  //using the instantiation pattern, "functional shared", create a f() called extend later
  //whatever value goes to the node on the left-hand side doesn't exist in the beginning, 
  //so set it to null and so does the right-hand side
  newTree.value = value
  newTree.left = null  
  newTree.right = null
 
 //create the f(), extend -> binaryMethod, the source object shares its memory address 
 //with newTree, the target object
  var extend = function (newTree, binaryMethod) {
    for (var key in binaryMethod) {
      newTree[key] = binaryMethod[key]
    }
  }
  extend(newTree, binaryMethod)
  return newTree
}

//binaryMethod holds a set of 3 methods in this case within the BinarySearchTree object
//insert(), contains(), and depthFirstLog()
//comparison between value and the value of the node already set 
var binaryMethod = {}
binaryMethod.insert = function (value) {
  if (value < this.value) {
    if (this.left !== null) {
      this.left.insert(value)
    } else {
      this.left = BinarySearchTree(value)
    }
  } else {
    if (this.right !== null) {
      this.right.insert(value)
    } else {
      this.right = BinarySearchTree(value)
    }
  }
}

binaryMethod.contains = function (value) {
  var tfJudge = false
  if (this.value === value) {
    return true
  }
  if (this.left !== null) {
    tfJudge = tfJudge || this.left.contains(value)
  }
  if (this.right !== null) {
    tfJudge = tfJudge || this.right.contains(value)
  }
  return tfJudge
}

binaryMethod.depthFirstLog = function (callback) {
  callback(this.value)
  if (this.left !== null) {
    this.left.depthFirstLog(callback)
  }
  if (this.right !== null) {
    this.right.depthFirstLog(callback)
  }
}
```

