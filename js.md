# General

* Never extend a host object other than `window`.
* JSHint your code (`npm install jshint`) and fix most if not all code that gives warnings.
  * Only these JSHint annotations are allowed to make exceptions for code:
      * `expr`
      * `sub`
* Document your code using JSDoc syntax.
  * Make sure only public methods and properties can appear in the documentation.
  * Use no more than 80 characters per line (code and comments in code may use more than 80 characters).
  * The return value must be documented properly: `@returns {string|null} Return value description.`.
* Never allow 'undefined' to be printed or added to a string from an undefined variable.
* If `a` is supposed to be a string but was not passed and will be added to another string: `a === undefined && (a = '');`
* Please do not use `function nameOfFunction() {}` style anywhere in your code.
* All identifiers (including variables) and  should use `camelCaseStyle` and not `underscore_style`.
* Use 4 spaces. Do not use real tabs.
* UTF-8 is the only accepted encoding.
* Always leave a new line at the end of the file.
* Use UNIX newlines; other formats will not be accepted (such as Windows CRLF).
* Brackets must be on same line as statement (see example below).
* Do not access another object's private properties from other objects; use the getter method if one is provided.
  * If a getter method is not provided, copy the original method/property from the class until a decision is made to make a public getter method in the original class.
* Test that every public method you have written works correctly when compiled with Closure Compiler with advanced optimisations: http://closure-compiler.appspot.com/home

# Example code

```javascript
/*jshint expr:true */
/**
 * A dependency function added to 00-deps.js. Please fix any function that
 *   uses 'function name() {}' style to 'var name = function () {}'.
 * @param {string|null} arg1 Argument description.
 * @param {string} [arg2] Not required argument.
 * @returns {string} Return value description.
 */
var dependencyFunction = function (arg1, arg2) {
  // Set the not required argument's value
  // Note that the () for the second part make a difference
  arg2 === undefined && (arg2 = '');

  // Brackets!
  while (condition) {
    condition = false;
  }

  for (var i = 0; i < something.length; i++) {
    something[i] = something[i] + ' ';
  }

  // Only increasing i, so end bracket may be on the same line
  for (var i = 0; i < something.length; i++) {}

  var a = function () {
    // function body
  }; // Do not use function a() {} style

  // If a condition is to be made this way, the end bracket may be on the same line
  if (condition) {}
  else {
    doSomething();
  }

  if (condition) {
    doSomething();
  }
  else if (condition2) {
    doSomethingElse();
  }
  else {
    doSomethingMore();
  }

  // These will not be accepted
  if (condition)
  {
    doSomething();
  } else {
    doSomethingElse();
  }

  if (condition)
  {
    doSomething();
  }
  else
  {
    doSomethingElse();
  }

  // String recommendations
  // - Please default to using single quotes
  // - Use 1 space between the concatenation operator (+)
  var a = ' ' + 'my' + 'string' + ' ';

  return 'something';
};

/**
 * A blank parent class. Give the original description however. This lives
 *   in 00-MyClassOriginal.js.
 * @constructor
 * @returns {MyClassOriginal} The class.
 */
var MyClassOriginal = function () {
  return this;
};
/**
 * Static method. Note that JavaScript 'extensions' never inherit static
 *   methods. See below on how to copy the static method to the inheriting
 *   class.
 * @param {string} arg1 A string parameter.
 * @returns {string} The processed string.
 */
MyClassOriginal.copyTheStaticMethod = function (arg1) {
  return arg1.replace(/\s+/, '');
};
/**
 * Instance method. See below on how to inherit.
 *   This method returns nothing so we are not documenting a return value.
 */
MyClassOriginal.prototype.instanceMethod = function () {
  fCore.debug('No return value');
};

/**
 * A class description. Should be the same as the description Flourish
 *   documentation has. This lives in 01-MyClassExtends.js.
 * @augments MyClassOriginal
 * @constructor
 * @param {string} someParam A parameter description.
 * @returns {MyClassExtends} The class.
 */
var MyClassExtends = function (someParam) {
  // Since this extends, we may want to call the parent constructor to get all the original properties
  // This is like calling parent::__construct() in PHP
  this.parent.constructor.call(this);

  /**
   * @type string
   * @private
   */
  this._someParam = someParam;

  return this;
};
/**
 * To inherit the original prototype, we must instantiate the parent class
 *   to this class' prototype.
 * @private
 * @type MyClassOriginal
 */
MyClassExtends.prototype = new MyClassOriginal();
/**
 * Gets the someParam property. A public getter.
 * @returns {string} The someParam property.
 */
MyClassExtends.prototype.getSomeParam = function () {
  return this._someParam;
};
/**
 * Sets the someParam property. A public setter.
 * @returns {MyClassExtends} The object to allow method chaining.
 */
MyClassExtends.prototype.setSomeParam = function (s) {
  this._someParam = s;
  return this;
};
/**
 * Access to parent class. Add this only when necessary. Always name it
 *   parent.
 * @type MyClassOriginal.prototype
 */
MyClassExtends.prototype.parent = MyClassOriginal.prototype;
/**
 * Documenting a private property/method is generally desired, but not
 *   required.
 * This property must be set for classes that extend or only the original
 *   constructor will be called.
 * @type function
 * @private
 */
MyClassExtends.prototype.constructor = MyClassExtends;
/**
 * A static method copied from the original class for comformance and
 *   similarity to the PHP class.
 * @param {string} arg1 A string parameter.
 * @returns {string} The processed string.
 */
MyClassExtends.copyTheStaticMethod = MyClassOriginal.copyTheStaticMethod;
```
