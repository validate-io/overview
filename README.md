Overview
========

> An overview of Validate.io. 

[Validate.io](https://github.com/validate-io) is a collection of JavaScript utilities for value validation.

In contrast to statically typed languages, such as C/C++, Java, and others, JavaScript is dynamically typed, which means that data types are assigned to values, not variables. Accordingly, value validation in JavaScript must occur at runtime when variables are dynamically assigned to values. For example, consider

``` c
int square(int x)
{
	int square_of_x;
	square_of_x = x * x;
	return square_of_x;
}
```

which, in JavaScript, becomes

``` javascript
function square( x ) {
	return x*x;
}
```

In the case of the C function, if a non-integer value is provided to the `square()` function, the program will throw an error. In the case of the JavaScript function, the program will do the best it can. For example,

``` javascript
var a = {};
console.log( square( a ) );
// Returns NaN
```

In this case, JavaScript concluded rightly that the result is not-a-number (`NaN`), but did not throw an error. While we may conclude that JavaScript silently failed, such silent failures can be a source of bugs. Somewhere higher up the application chain, variables may be assigned to unexpected data types, leading to a subtle cascade of unexpected behavior. Dynamic typing and silent failures do lend JavaScript a certain resilency (a program won't die even if completely broken), but this comes at the cost of possibly more difficult and time-consuming debugging when attempting to track down a problem at its source.

One way to prevent the silent failure above is to modify the function to include type-checking.

``` javascript
function square( x ) {
	if ( typeof x !== 'number' || x !== x ) {
		throw new TypeError( 'square()::invalid input argument. Value must be numeric.' );
	}
	return x*x;
}
```

The above modification places the burden of type-checking during application runtime. Critics may claim that the JavaScript code is now slower (additional boolean operations) and more verbose (equivalent in length to the C program). The critiques have merit, but need to be considered within the wider development context. Part of writing good code is creating code which is easily consumable and robust, especially code which is consuming facing (e.g., almost all libraries). 

JavaScript value validation is nothing new. Often in client applications, methods validate user input before relaying information back to the server, such as may be the case when processing forms. Accordingly, several client-side validation libraries exist: [validate.js]( http://rickharrison.github.io/validate.js/ ), [underscore](http://lodash.com/), and others.

Validation is not as common in server-side applications and libraries. Often times, server-side libraries exposing public methods do not type check input arguments. Instead, library developers leave it to the consumer to ensure her use conforms to available documentation and method internals. One reason cited for omitting input argument validation is that validation makes code less compact (longer files) and that consumers should be responsible for ensuring their code plays nice with API (i.e., read the code).

> Input validation helps both library developers and consumers.

This practice, however, has questionable value for the following reasons. First, including input validation is a form of documentation. Second, validation more clearly defines expectations. Third, validation helps prevent library authors from introducing subtle bugs in their code, especially when playing fast and loose with truthiness and falsiness. Fourth, throwing errors when validation requirements are not met allows consumers to fail fast. And accordingly, validation enables consumers to more easily debug library interfacing code.

While not commonly used, server-side validation libraries do exist, such as the relatively popular [validator](https://www.npmjs.org/package/validator) module. The problem with almost all of these libraries is that they are __all-or-nothing__. For instance, suppose you want to use the [underscore](http://underscorejs.org/#isObject) method to validate that a value is an object. To do so, you cannot easily extract and use that one method; you must download and load the entire library. Such requirements lead to code bloat and attempts to wrangle a library into providing a piece of functionality for a particular use case.

Additionally, while some libraries attempt to be monolithic repositories for validation, others are so specialized to not be generally useful. And even if one attempted to compose a larger library from several smaller ones, the differences in code style and approach (method chaining, callbacks, argument order, etc) would lead to a franken-mess.

> Do one thing and do one thing well.

[Validate.io](https://github.com/validate-io) attempts to address these problems by providing small modular components, each exposing consistent interface and style, which may be arbitrarily combined into whatever validation library is demanded of your validation. If you only need a validation library which type checks [`objects`](https://github.com/validate-io/object), [`strings`](https://github.com/validate-io/string), [`arrays`](https://github.com/validate-io/array), and [`timestamps`](https://github.com/validate-io/timestamp), then easily roll your own library requiring only those components. If you want an all-encompassing validation library, then just use the Validate.io [validator](https://github.com/validate-io/validator), which contains all validation modules and provides a consistent rule-based interface to input argument validation. If you would prefer to use method-chaining, then simply implement a method-chaining interface across the individual components.

> Complexity in composition.

While many of the validation components are simple,

``` javascript
function isString( value ) {
	return ( typeof value === 'string' );
}
```

complexity arises in their composition. By modularizing individual components into distinct autonomous units, we alleviate the burden of implementing each method from scratch and instead shift the focus to abstraction layers bringing the components together. Hence, we embrace flexilibity while ensuring a consistent and well-tested foundation.

[Validate.io](https://github.com/validate-io) is a work-in-progress. Certainly feel free to offer suggestions and join the organization if you want to contribute more fully. Any contributions should abide by the rules set forth in the [contributing guide](https://github.com/validate-io/contributing).


