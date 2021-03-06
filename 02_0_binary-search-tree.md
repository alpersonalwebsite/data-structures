# Binary Search Tree

Create a `Binary Search Tree` using the `class keyword` and add through its `prototype` a function to insert/`add nodes` to the tree.

## Solution:

```JavaScript
class BinarSearchTree {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}

BinarSearchTree.prototype.add = function(value) {
  if (value <= this.value) {
    if (!this.left) {
      this.left = new BinarSearchTree(value)
    } else {
      this.left.add(value);
    }
  } else {
     if (!this.right) {
      this.right = new BinarSearchTree(value)
    } else {
      this.right.add(value);
    }
  }
}

let arr = [50, 100, 40, 12, 90, 98];

let BST;
for (let i = 0; i < arr.length; i++) {
  if (i === 0) {
    BST = new BinarSearchTree(arr[0]);
  } else {
    BST.add(arr[i]);
  }
}

console.log(BST);
```

## Result:

Time may vary in relation to the processes running on your machine.

```
BinarSearchTree {
  value: 50,
  left:
   BinarSearchTree {
     value: 40,
     left: BinarSearchTree { value: 12, left: null, right: null },
     right: null },
  right:
   BinarSearchTree {
     value: 100,
     left: BinarSearchTree { value: 90, left: null, right: [Object] },
     right: null } }
Function took 4.59142804145813 milliseconds.
```

From a tree POV, it should look like...

```
             50
           /    \
          40    100
         /  \   /  \
       12 null 90  null
              /  \
            null 98
```

---

Now we are going to add the method get(value)
This function will take a value and check if we have it in our tree.

_Note:_ I´m adding and incrementing the variable levels so you can easily see all the levels that have been iterated.
Cases:

* If the value is at the root... Level is going to be 0
* If value is not present in the nodes, it will be the entire iteration which is equals to all levels. In this example: 3 (0,1,2,3)

Remember that this is just a visual reference to help you in the recursion implementation and understanding.

## Solution:

```javascript
// I start with -1 to take 0 as a level like arr
let levels = -1;

BinarSearchTree.prototype.get = function(value) {
  levels++;

  // if value is root node
  if (this.value === value) {
    return true;
  }

  // There are 2 possibilities: left>less, right>bigger

  if (value > this.value) {
    // if we dont have that value. Example: 1000
    if (!this.right) return false;
    else {
      // recursion
      return this.right.get(value);
    }
  }

  if (value < this.value) {
    if (!this.left) return false;
    else {
      // recursion
      return this.left.get(value);
    }
  }
};

console.log(BST.get(99));
console.log(levels);
```

## Result:

```
false
3
```

---

The next step is to traverse the tree in both ways:

* Depth first traversal: from top to the bottom, first left then right.
* Breadth First Traversal: level by level

For a tree like...

```
             70
           /    \
          40    100
         /  \   /  \
        25  41 90  null
       /  \
      11  null
```

The expected results are:

* Depth: 70, 40, 25, 11, 41, 100, 90
* Breadth: 70 - 40, 100 - 25, 41, 90 - 11

## Solution:

```javascript
BinarSearchTree.prototype.depth = function(traverseFn) {
  traverseFn(this.value);
  if (this.left) {
    this.left.depth(traverseFn);
  }
  if (this.right) {
    this.right.depth(traverseFn);
  }
};

function logInConsole(value) {
  console.log(value);
}

BST.depth(logInConsole);
```

## Result:

```
50
40
12
100
90
98
```

---

## Solution:

```JavaScript
BinarSearchTree.prototype.breadth = function (traverseFn) {

// we start with a queue with our tree
let q = [this];

// we iterate the q until we dont have more nodes
while (q.length) {
  // we remove the first element and log it
  let level = q.shift();
  traverseFn(level.value);

  if (level.left) q.push(level.left);
  if (level.right) q.push(level.right);
}

}

function logInConsole(value) {
  console.log(value);
}


BST.breadth(logInConsole);
```

## Result:

```
50
40
100
12
90
98
```

---

One more note... We can also use recursion to get the _minimum_ and _maximum_ value of the nodes in our tree.

```javascript
BinarSearchTree.prototype.min = function() {
  if (this.left) return this.left.min();
  else return this.value;
};

BinarSearchTree.prototype.max = function() {
  if (this.right) return this.right.max();
  else return this.value;
};

console.log(BST.min());
console.log(BST.max());
```

Result:

```
12
100
```
