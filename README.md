THIS IS A VEEEEEEEEEEERY OLD PROJECT IN EARLY 2009. DO NOT USE. 

PLEASE JUST TRY [ejs](https://www.npmjs.com/package/ejs) INSTEAD.

# tpl

A very simple JavaScript template engine using embeded javascript code


## Syntax

### \<?js `codeSlice` ?>

You could embed javascript slices into the template(or just strings), just like PHP does.

If we have a template: 

```php
<ul><?js
    var i = 5;
    while(i -- > 0){
    	?>  <li></li><?js
    }
?></ul>
```

Then it will create:

```html
<ul>
  <li></li>
  <li></li>
  <li></li>
  <li></li>
  <li></li>
</ul>
```

### @{`value`}

Will be replaced by the `value` variable or statement within the current scope. 

For example, if the template is:

```ftl
<span class="name">@{it.name}</span>
```

object:

```js
{
  name: "Peter"
}
```

result:

```html
<span class="name">Peter</span>
```

**NOTICE** that in top level of the template, there is always a variable `it` which is the object passed into the template function.


#### Duplex usage

```php
<ul><?js
    it.forEach(function(user){
    	var age = parseInt(user.age);
    	?><li>@{user.name}, age @{age}</li><?js
    });
?></ul>
```

The template above might accept some data like:

```js
[
	{name: 'Peter', age: '40'},
	{name: 'Tom', age: 10}
]
```

And the result is:

```html
<ul>
<li>Peter, age 40</li>
<li>Tom, age 10</li>
</ul>
```


## APIs

### tpl.compile(template)

Compiles the template string into a template function which only accepts one parameter, `it`.

- template `String` the template string

Returns `function(it)`

```
var templateFn = tpl.compile(template);
var result = templateFn(object);
```

### tpl.render(template, object)

Renders a template with the given object.

Do **NOT** use this method frequently, because compiling a template costs much of performance and `tpl.render` will not memoize any result.

So, for most cases, you should cache the template function which returns from `tpl.compile` by yourself.
