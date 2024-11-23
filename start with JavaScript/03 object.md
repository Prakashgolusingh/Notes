# Objects
+ Objects are used to store keyed collections of various data and more complex entities.
+ In JavaScript, An object can be created with figure brackets {…} with an optional list of properties.
  ```
    let user = {     // an object created with property
    name: "John",  // by key "name" store value "John"
    age: 30        // by key "age" store value 30
    "likes birds": true,  // multiword property name must be quoted
    };
  ```
+ A property is a “key: value” pair, where key is a string (also called a “property name”), and value can be anything.
+ An empty object (“empty cabinet”) can be created using one of two syntaxes:
  ```
    let user = new Object(); // "object constructor" syntax
    let user = {};  // "object literal" syntax
  ```

The value can be of any type. Let’s add a boolean one:

user.isAdmin = true;
> [!NOTE]
> To remove a property, we can use the delete operator: `delete user.age;`

> [!TIP]
> The last property in the list may end with a comma:
> That is called a “trailing” or “hanging” comma. Makes it easier to add/remove/move around properties, because all lines become alike.

## Square brackets
+ For multiword properties, the dot access doesn’t work:
> [!CAUTION]
> `user.likes birds = true;` // this would give a syntax error
+ The dot requires the key to be a valid variable identifier. That implies: contains no spaces, doesn’t start with a digit and doesn’t include special characters ($ and _ are allowed).
+ There’s an alternative “square bracket notation” that works with any string:

let user = {};
1. set
  user["likes birds"] = true;
2. get
  alert(user["likes birds"]); // true
3. delete
  delete user["likes birds"];
+ Now everything is fine. Please note that the string inside the brackets is properly quoted (any type of quotes will do).
+ Square brackets also provide a way to obtain the property name as the result of any expression :
  ```
    let key = "likes birds";
    user[key] = true;
    // same as user["likes birds"] = true;
  ```
+ Here, the variable key may be calculated at run-time or depend on the user input. And then we use it to access the property. That gives us a great deal of flexibility.
  ```
    let key = prompt("What do you want to know about the user?", "name");
    // access by variable
    alert( user[key] ); // John (if enter "name")
    // The dot notation cannot be used in a similar way:
    let key = "name";
    alert( user.key ) // undefined
  ```
## Computed properties
+ We can use square brackets in an object literal, when creating an object. That’s called computed properties.
  ```
    let fruit = prompt("Which fruit to buy?", "apple");
    let bag = {
      [fruit]: 5, // the name of the property is taken from the variable fruit
    };
    alert( bag.apple ); // 5 if fruit="apple"
    let bag = {
    [fruit + 'Computers']: 5 // bag.appleComputers = 5
    };
  ```
+ The meaning of a computed property is simple: [fruit] means that the property name should be taken from fruit.
+ Square brackets are much more powerful than dot notation. They allow any property names and variables. But they are also more cumbersome to write.

## Property value shorthand
In real code, we often use existing variables as values for property names.
  ```
    function makeUser(name, age) {
      return {
        name: name,
        age: age,
        // ...other properties
      };
    }
    let user = makeUser("John", 30);
    alert(user.name); // John
  ```
+ In the example above, properties have the same names as variables. The use-case of making a property from a variable is so common, that there’s a special property value shorthand to make it shorter.
+ Instead of name:name we can just write name, like this:
    ```
    function makeUser(name, age) {
      return {
        name, // same as name: name
        age,  // same as age: age( We can use both normal properties and shorthands in the same object: like age: 30)
        // ...
      };
    }
  ```
## Property names limitations
+ As we already know, a variable cannot have a name equal to one of the language-reserved words like “for”, “let”, “return” etc.
+ But for an object property, there’s no such restriction:
  ```
    // these properties are all right
    let obj = {
      for: 1,
      let: 2,
      return: 3
    };
  ```
+ Other types are automatically converted to strings.
+ For instance, a number 0 becomes a string "0" when used as a property key:
  ```
    let obj = {
    0: "test" // same as "0": "test"
    };
    alert( obj["0"] ); // test
    alert( obj[0] ); // test (same property)
  ```
+ There’s a minor gotcha with a special property named __proto__. We can’t set it to a non-object value:
  ```
    let obj = {};
    obj.__proto__ = 5; // assign a number
    alert(obj.__proto__); // [object Object] - the value is an object, didn't work as intended
  ```
+ Reading a non-existing property just returns undefined.`user.noSuchProperty === undefined;` // true means "no such property"
+ When an object property exitsts, but stores undefined, so we use "in" operator for that. `"key" in object`. eg. `"age" in user; // return true/false`.
+ key usually a quoted string or omit quotes means a variable that contains key name.eg -
  ```
    let key = "age";
    alert( key in user ); // true, property "age" exists
  ```
+ We mostly use null for “unknown” or “empty” values. So the in operator is an exotic guest in the code.
+ we use for...loop to get all key.
  ```
    for (let key in user) {
      alert( key );  // name, age, isAdmin
      // values for the keys
      alert( user[key] ); // John, 30, true
    }
  ```
+ If we loop over an object, integer properties are sorted, others appear in creation order.let’s consider an object with the phone codes:
  ```
    let codes = {
      "49": "Germany",
      "41": "Switzerland",
      "44": "Great Britain",
      "1": "USA"
    };

    for (let code in codes) {
      alert(code); // 1, 41, 44, 49
    }  
  ```
+ "49" is transformed to an integer number.But "+49" are not so its order of reading is // 49, 41, 44, 1.

+ Math.trunc is a built-in function that removes the decimal part
 ```
    alert( String(Math.trunc(Number("49"))) ); // "49", same, integer property
    alert( String(Math.trunc(Number("+49"))) ); // "49", not same "+49" ⇒ not integer property
    alert( String(Math.trunc(Number("1.2"))) ); // "1", not same "1.2" ⇒ not integer property
  ```