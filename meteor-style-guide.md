This is a set of guidelines for writing clear, organized, high-quality code that is consistent with the code in Meteor core.

All code in pull requests should follow these rules. Anyone writing a Meteor app or package is encouraged to follow this guide as well!

Commit messages

The first line of a commit should be 50 characters or less. This is a soft limit, so that git's command line output is more readable. Use this line as a clear summary of the change. Try using imperative language in this line. A good commit message is "Add reactive variable for CPU clock speed". Worse examples include "cpu clock speed" or "Added Meteor.CPU_speed so that we can avoid a setTimeout in the main example."

Subsequent lines (if any) can be fairly verbose and detailed. Use your judgement.

Whitespace

2 space indents (setq js-indent-level 2)
spaces, not literal tabs (setq-default indent-tabs-mode nil)
no trailing whitespace (setq-default show-trailing-whitespace t)
maximum line length 80 (setq-default fill-column 80)
Emacs users should check out js2-mode for a nice way to avoid silly javascript errors, and help enforce standards.

Line length

Source code. Try to stick to 80 characters per line. Comments should be line-wrapped to under 80 characters. It's OK to occasionally go over 80 characters if it substantially improves later indentation, or if there is no good way to break the line.

Terminal output. Lines should be kept short enough that they won't wrap on an 80-column-wide window under normal circumstances (it's fine if, if the user gives something a really super unusually long name, that wrapping happens). Or, even better, detect the actual width of the terminal and format according to the actual window width.

Spaces between tokens

Separate tokens with a single space as in these examples:

a = b + 1, not a=b+1

function (a, b, c) { return 1; }, not function(a,b,c) {return 1;}

for (i = 0; i < 3; i++), not for(i=0;i<3;i++)

a(1, 2, 3), not a(1,2,3)

{ a: 1, b: 2 }, not {a:1,b:2}

if (a), not if(a)

Unary negation also takes a space:

if (! a), not if (!a)

But increment and decrement operators don't take a space:

a++, not a ++

--b, not -- b

When functions or objects fit entirely on a single line, put a space inside the enclosing braces:

stack.push({ parent: node, red: true }), not stack.push({parent:node, red: true})

a(function () { return true; }), not a(function () {return true;})

Single-line arrays don't get that space though:

samples.concat([1, 2, 3]), not samples.concat([ 1, 2, 3 ])

Comments

Code should be well commented. Make sure to document the most important pieces of information for both users of the code and future maintainers of the code.

Most functions should begin with a comment explaining what they do, what arguments they take, and what they return. A good function comment allows developers to use a function without having to read it's source.

Most files should start with a comment explaining what functionality is in the file. A good file comment allows developers to understand how the different pieces in the system fit together without having to read a lot of code.

Also comment liberally inside functions. For example, objects saved as class members should note what the object keys and values are. Any tricky bits of code should have a comment explaining what is going on. Focus on documenting why the code is the way it is, not what it does.

Generally prefer // comments to /*. Use /* comments when putting a comment in the middle of a statement. Javadoc /** style comments are OK for function definitions.

Bad:

var Whirlygig = function (name) {
  var self = this;
  self.name = name;
  self.values = {};
};

_.extend(Whirlygig.prototype, {
  /* add a value */
  addValue: function (x) {
    self.values[x.key] = x.value - 1;  // subtract 1 from value
  },

  /* get serialized list */
  serialize: function () {
    return _.map(self.values, function (v, k) {
      var newVal = mungeValue(v, false);
      return {key: k, value: v + 1};  // add 1 to values
    });
  }
});
Good:

// A Whirlygig represents a remote client's view of the global Weasel.
// It can receive arbitrary values from the network and save them
// for later use.
var Whirlygig = function (name) {
  var self = this;
  self.name = name;  // name of the remote weasel
  self.values = {};  // remote key name -> 0-indexed value

};

_.extend(Whirlygig.prototype, {
  // Take a key/value pair from the remote Weasel and save it locally.
  addValue: function (x) {
    // Weasels use 1-indexed arrays. Subtract 1 to convert to 0-indexed.
    self.values[x.key] = x.value - 1;
  },

  // Return a list of stored values in a format suitable for sending to
  // a Weasel.
  serialize: function () {
    return _.map(self.values, function (v, k) {
      var newVal = mungeValue(v, false /* foldValue */);
      // Weasels use 1-indexed arrays. Add 1 to convert back to 1-indexed.
      newVal = newVal + 1;
      return {key: k, value: newVal};
    });
  }
});
Comments should be grammatically correct English text

For example, avoid spelling mistakes and run-on sentences. Many people judge the quality of software in part by the quality of the comments, so it's important that your writing reflect well on the project. At least when it comes to grammar, take the same care in writing comments as you would in writing a post on the Meteor blog.

Use the Oxford comma unless there is a good reason not to. That is, in a list of three or more items, include a comma after the next-to-last item. Right: "I dedicate this book to my parents, Ayn Rand, and God." Wrong: "I dedicate this book to my parents, Ayn Rand and God." (Theresa Nielson Hayden) But sometimes there is in fact a good reason to omit the Oxford comma. ("To my mother, Ayn Rand, and God.")

The period goes outside the parenthetical when the parenthetical is a sentence fragment (as in this example). (But the period goes inside the parenthetical when the parenthetical is a complete sentence that stands by itself, as in this case.) As Stack Overflow puts it, "the sentence should still be valid if you remove the parentheses and everything inside them."

Use camelCase for identifiers

Functions should be named like doAThing, not do_a_thing.

Other variable names get the same treatment: isConnected, not is_connected.

Originally Meteor used a different convention, in which underscores were sometimes encouraged. But now we've moved everything over to camelCase to match common JavaScript convention. If you see old inchworm_style identifiers you're encouraged to convert them. There are a few places in public APIs where inchworm_style aliases are provided for backward compatibility. These will likely be removed for 1.0.

Private identifiers start with underscores

If a method or property name starts with _, it's a private implementation detail and shouldn't be used from outside the module where it is defined. It could disappear or change at any time.

Brace style

Use this brace style:

if (a < 0) {
  console.log("a is negative");
  handleNegativeA();
} else if (a > 0) {
  console.log("a is positive");
  handlePositiveA();
} else {
  console.log("a is zero or NaN");
  handleOtherA();
}
Braces are optional around single-statement blocks

You may leave out the curly braces when the block's condition and its body are each short enough to fit on one line:

if (! alreadyDone)
  doSomething();

while (node.parentNode)
  node = node.parentNode;

// BUT NOT:
if (somethingVeryVeryVeryVeryLong &&
    somethingElseLong)
  shortBody();
// AND NOT:
if (something)
  someLongFunctionCall(
      withThisArg,
      andThatArg);
If you feel that the code would be much easier to read with braces, though, you can add them at your discretion.

It's OK to have braces in one branch of an if..else construct, and not another:

if (widgetMarketOpen) {
  buyWidgets();
  sellWidgets();
} else
  throw new Error("No widget trading for you!");
Always use semicolons; beware semicolon insertion when breaking lines

JavaScript permits semicolons to be omitted sometimes at the end of statements. Don't do this.

When breaking lines you must be careful to avoid the JavaScript semicolon insertion feature. For example, this code:

return
  doSomething() * 2 + 27;
will be interpreted in JavaScript as:

return;
doSomething() * 2 + 27;
Signature comment for arguments

If a function takes more arguments than there are formal parameters (via arguments), comment like so:

_.extend(Foo.prototype, {
  myVariadicFunction: function (/* arguments */) { ... },
  anotherVariadicFunction: function (x, y, z, /* arguments */) { ... },
});
(Not /* varargs */ -- that's a C concept.)

self rather than this

For methods that are more than a line or two long, always begin with var self = this, and always use self, never this. Ideally, do this everywhere. (Rationale: if the method contains callbacks or other local functions, it is far too easy to write this inside the function, expecting to get the outer this. You need to use self in those cases. At that point, it's too hard to remember when to use self and when to use this, so you need to use self everywhere.)

IE limitations

Do not index into strings (no "abc"[1]). In a = b.c; a(); don't assume anything about this when a is called. If you need this to be something specific, always use bind, call, or apply. (Old IE will sometimes try to auto-bind a so that this === b!)

Don't use Javascript keywords in ways that will confuse syntax highlighters or old browsers

For example, don't name a method 'return', even if that's allowed by the Javascript grammar. Some emacs syntax highlighting modes will choke on that. Even if that doesn't happen, it's still grating to see it highlighted as a keyword. What's more, old browsers may fail to parse the code!

This goes both for actual JavaScript keywords, as well as random words don't currently have special meaning in JS, but which at some point were reserved, or rumored to be reserved, or whatever, and thus have ended up in people's syntax highlighting tables. For example, 'class'. Lists of such words can be found here.

Sometimes you will have to work with a third-party library that does not follow this rule. A prime offender is Future from the node-fibers library, which has a method named return. In this case you should use a string constant to protect the keyword, like so:

var f = new Future;
f['return'](123); // not f.return(123) since return is a keyword
It's important that you do this, because if the code ended up in a file that is used on both the client and the server, then it could fail to parse on old browsers. The parse failure will happen even if the code can not actually ever run on the client.

Class pattern

Use the native Javascript system: new and prototypes. Begin your constructor function with (1) var self = this, and then (2) define every single attribute that will ever be defined on the object, in straight-line code, commenting each one. If any of them are public, indicate this in the comments. Then, define methods on the class by extending its prototype. Private methods should start with _. For public (API) classes, avoid private methods if possible/convenient, but don't go to extreme lengths.

Singletons

Don't use global variables (or a closure) instead of an object just because the object would be a singleton. That is: if you have something that can logically be seen as an object, with attributes and methods that act on the attributes, make it a proper object even if there will only ever be one instance of that object.

Don't make the attributes into file-scope variables and make the methods into exported functions, and don't use the closure-based module pattern from "Javascript: The Good Parts".

Instead, use the normal class pattern for your object, and then instantiate the object once at the end of the file.

Good criteria for deciding whether you should use an object or file-scope variables:

In the future, is it even remotely conceivable that someone would ever want to have two instances of this thing?
Even if not, does this functionality add up to something that is logically an object? (Is it a set of methods that control access to some private state? If not, would it be clearer if it were expressed that way?)
Avoid setting variables to undefined -- instead, use null

Reserve undefined to mean "argument not provided" or "no such key in object", or for cases where null would signify some other valid value. Use null in other cases, especially the case where a variable's type is "Foo or nothing", eg, "Array<String> or null", "String or null" (analogous to NULL in C, null in SQL, foo option in ML...)

Note that undefined is not a valid EJSON or JSON value, while null is.

An uncaught exception is a bug

If a user of our software sees a stack trace, that's a bug in our software. (Unless of course the exception came from code they wrote, rather than code we wrote!)

That means that it's perfectly OK to throw exceptions to handle conditions that indicate a bug in our code. // count must be positive or zero var printSpam = function (count) { if (count < 0) throw new Error("negative count?"); for (var i = 0; i < count; i++) console.log("spam!"); };

However, in all other cases where you call a function that could throw an exception, you must have a clear and specific plan for handling that exception, and that plan should be immediately evident to the reader of your code. For example, if we are trying to open a file and that file doesn't exist, that's not a bug (unless we have some reason to know for certain that the file exists) and must not result in an uncaught exception. It's an error condition and should (ultimately) result in a helpful error message being printed for the user.

Never close over loop variables

This code is broken:

for (var a in foo) {
  stuff.push(function () { console.log(a); }); // Broken
}
_.each(stuff, function (f) { f(); });
If foo is { x:1, y:2, z:3 }, it will print 'z' three times, because all three functions created inside the loop point at the same instance of the variable a, which will have the value it has at the end of the loop. This fix won't help:

for (var a in foo) {
  var x = a;
  stuff.push(function () { console.log(x); }); // Still broken
}
_.each(stuff, function (f) { f(); });
because Javascript has only one scope per function (it is as if it floats the definition of 'x' up to the beginning of the nearest enclosing function). You will need to do something more aggressive:

// Use bind
var func = function (x) { console.log(x); };
for (var a in foo) {
  stuff.push(_.bind(func, null, a)); // Works
}
_.each(stuff, function (f) {f();});

// Or a higher-order function
var makeFunc = function (x) { return function () { console.log(x); }; };
for (var a in foo) {
  stuff.push(makeFunc(a)); // Works
}
_.each(stuff, function (f) {f();});

// Or put the relevant part of the loop body in a separate function
var addFunc = function (x) {stuff.push(function () {console.log(x);})};
for (var a in foo) {
  addFunc(a); // Works
}
_.each(stuff, function (f) { f(); });

// Or don't use a native loop
_.each(_.keys(a), function (x) {
  stuff.push(function () { console.log(x); }); // Works
});
_.each(stuff, function (f) { f(); });

// Or refactor to avoid
for (var a in foo) {
  console.log(a);
}
Often, none of these alternatives will look very good. Use your discretion and creativity.

Equality Testing

Always use ===, not ==.

If you want to compare a number to a string version of said number, use x === parseInt(y), x.toString() === y, or x === +y. It is much more clear what is going on. (Note that those alternatives differ in how they handle non-numeric characters or leading zeros in the string. Only the third alternative gets all the edge cases right.)

Coercion to boolean

If you have a value that could be truthy or falsey, and you want to convert it to true or false, the preferred idiom is !!:

var getStatus = function () {
  // Return status (a non-empty string) if available, else null
};

var isStatusAvailable = function () {
  return !! getStatus();
};
|| for default values

|| is handy for introducing default values:

this.name = this.name || "Anonymous";

_.each(this.items || [], function () { ... });
But be careful. This doesn't work if false is a valid value.

&& for conditional function calls

&& can be handy when conditionally calling a function for its value:

// Preferred
return event && handleEvent(event);

// Not preferred
return event ? handleEvent(event) : null;
We also like to use it to call a function only if some other value is present:

// Preferred
for (var node = first; node; node = node.parent) {
  node.label && node.callback && node.callback(node.label);
}

// Not preferred (unless it's clearer in a particular case)
for (var node = first; node; node = node.parent) {
  if (node.label && node.callback)
    node.callback(node.label);
}
It's true that this confuses people who haven't seen && used in this way before. But once you're used to it it's easy to read and convenient. We think it's worth the tradeoff.

a && b || c

This is an idiom borrowed from Python. It does the same thing as a ? b : c, except that if a and b are both false, it returns c. You should use a && b || c only where a ? b : c won't do the job.

// If we have a name, return the entry for it in Template, if any. Otherwise return {}.
return name && Template[name] || {};

// This is subtly different. It will return undefined if name is truthy but isn't present in Template.
return name ? Template[name] : {};
Take a moment to convince yourself that a && b || c works as advertised, then memorize its behavior and don't give it another thought.

Keyword arguments with options

If a function takes a lot of arguments, especially if many of those arguments are optional, or several of the arguments are functions, don't try to put all of the arguments in the function's signature. Instead, add a final argument, options.

var makeCube = function (owner, options) {
  // Provide default values for the options
  options = _.extend({
    color: 'grey',
    weight: 1.0,
    material: 'aluminum',
    onRender: function () {},
    onDestroy: function () {}
  }, options);

  console.log(owner + ", here is your " + options.color + "cube.");
  options.onRender();
  ...
});
Keep the most important arguments in the signature (eg, owner above). Ideally, the arguments in the signature are the "cake" (the main substance and structure of the operation), and the options are the "icing" (frills and modifiers).

However, this is just a guideline. You should do whatever will make life easiest for the person calling the function. Consider the situation from their point of view. What are they trying to accomplish by calling your function? What will make the callsite look clearest to someone reading the code a year later who doesn't understand it?

Error objects

All errors that are thrown or passed to callbacks should all be instances of Error. They should not be instances of Meteor.Error unless they are errors meant to go over the wire via DDP. That is, only use a Meteor.Error to specifically tell the client something, not to indicate a programming error or an unexpected condition in common code.

Functions should be consistent about whether they return a value

Either: all returns in a function should either explicitly return a value (perhaps undefined), and function should end with an explicit return or throw; or there should be no returns in the function which return a value. (js2-mode.el will complain if you don't, and often its complaints indicate an actual bug where you meant to return false or something.)

var BAD1 = function () {
  if (x)
    return 5;
};
var BAD2 = function () {
  if (x)
    return;
  return 5;
};
var GOOD1 = function () {
  if (x)
    return 5;
  throw Error("Nope");
};
var GOOD2 = function () {
  if (x)
    return 5;
  return undefined;
};
Deprecated code and backwards compatibility

When making breaking changes to Meteor, we try to preserve backwards compatibility when feasible. For example, if we rename a function, we also keep the old name available. When doing this, mark any backwards-compatibility code with a comment like:

// XXX COMPAT WITH 0.6.4
where 0.6.4 is the last release which had the old behavior. (Ignore patch releases like 0.6.4.1, which generally should not change any interfaces.)

In addition, when feasible (eg, aliasing a function name), put the backwards compatibility code in a file called deprecated.js in the package.

Initializing object member variables

Always initialize all member variables in an object constructor. This is so you don't have to search through the file to find all the potential member variables. It's also a good place to comment on what each variable is.

var Foo = function (options) {
  var self = this;
  // Have we done the thing already?
  self.doneThingYet = null;   // <- good
};

_.extend(Foo.prototype, {
  doThing: function () {
    var self = this;
    self.doneThingYet = true;
    self.thingResults = 4;  // <- bad. using an undeclared variable.
  }
});
Exports in CommonJS modules (eg, node.js code)

Normally in Meteor code, both packages and apps, you don't use CommonJS constructs such as require and exports. Instead you let Meteor set up your imports and exports for you. However, if you work on the meteor command line tool, you're writing bare node.js code and the following applies.

This is the generally preferred style for module exports. It's a good all-around, general purpose style:

// mypackage.js

var mypackage = exports;

mypackage.someFunction = function () {
  ...
};

var helperFunction = function {
  ...
};

mypackage.otherFunction = function () {
  helperFunction();
  mypackage.someFunction();
  ...
};
Here's another acceptable style. It's good when you want to make it very clear what exactly symbols a module exports, since all of the exports are grouped at the bottom. However this comes at a cost: First, when you when you look at a given function, you can't tell whether it's exported. Second, it's harder to copy and paste between mypackage.js and other files, because you call someFunction as someFunction() inside the file, but mypackage.someFunction() from outside the file. Use this style when you have a specific reason to:

// mypackage.js

var someFunction = function () {
  ...
};

var helperFunction = function {
  ...
};

var otherFunction = function () {
  helperFunction();
  someFunction();
  ...
};

_.extend(exports, {
  someFunction: someFunction,
  otherFunction: otherFunction
});
Don't use the following style. Besides requiring an extra indent level, it forces all variables and functions that aren't exported to move to the top or the bottom of the file. That harms readability and is obnoxious, and there are no compensating benefits:

// WRONG -- DON'T DO THIS

var mypackage = exports;

var helperFunction = function {
  ...
};

_.extend(mypackage, {

  someFunction: function () {
    ...
  },

  otherFunction = function () {
    helperFunction();
    mypackage.someFunction();
    ...
  }

});

// WRONG -- DON'T DO THIS
To be clear, _.extend is great for putting methods onto prototypes. Just don't use it to put exported functions onto exports for the reasons noted above.

Generate error messages on the client, not on the server

Say you have a server method that can fail in one of three ways: "No such user", "Site doesn't exist", or "Access denied".

Right: In the event of failure, the server returns a symbolic error code, 'no-such-user', 'site-not-found', or 'access-denied'. The client then looks up that error code in a table to get an appropriate message and displays it to the user.

Wrong: The server returns an error string, such as "No such user", "Site doesn't exist", or "Access denied", which the client prints. This creates multiple problems. First, what if in the future the client wants to make a decision based on the error returned? (For example, a client might want to create the site if it doesn't exist, or try to log in as an administrator if access was denied.) Second, it complicates internationalization (the user's language code would have to be sent to the server so that it could return error messages in the appropriate language, requiring a protocol change, whereas constructing the error on the client does not block internationalization on a protocol change).

Wrong: The server returns a HTTP-style status code and message, such as "403 Access Denied" or "404 No Such User". HTTP just doesn't define enough status codes for them to be a viable alternative to string codes like "site-not-found". For example, if 404 is used to represent the 'no-such-user' condition, what's left to represent 'site-not-found'?

Only user interface code should engage with the user

Code should be strictly divided into user interface code, which can print messages, read input, make sounds, or otherwise engage with the user; and library code, which is not allowed to do any of these things.

The amount of user interface code should be kept as small as possible. It should focus on getting instructions from the user, calling library code to carry out those instructions, and explaining to the user what happened – wherever possible, the logic to present this user interface should be separated from the library code that does the actual work, for clarity, reusability, and modularity.

User interface code can call library code, but library code must not call user interface code. Library code is not aware that the user exists, because indeed it might in the future be used in a context where there is no human user or terminal.

User interface code needs to have a clear UX contract that explains how it fits into the interaction flow with the user, such as implementing a command-line command or running a login interaction.

As an example of this principle, when a random function in the codebase needs to report an error or print a status message, it is not allowed to write to stdout or stderr. If any function is allowed to write to the terminal, it becomes impossible to gain any confidence in the quality of our UX, because it is impossible to reason about what messages will be printed when without looking at every single write statement in the codebase and reasoning about the call graph. Instead, the function needs to expose some kind of interface to its caller (such as a particular return value, or a callback) that indicates the condition that has occurred. The caller needs to plumb that back to its own caller, and so forth, until we reach user interface code that can figure out the right way to present the condition to the user (if it thinks the user cares about it).

Likewise, random functions must not have the side effect of printing a prompt and collecting input. If your function calls out to a service that requires authentication, it shouldn't start out by prompting the user for a password if not logged in, unless it is explicitly user interface code and that prompt is in its contract. Instead, refactor to provide a function "ensureLoggedIn" that the user interface code calls to explicitly run a login flow, before starting a complex operation that requires that the user be authenticated.
