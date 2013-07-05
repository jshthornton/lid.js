lid.js - Linking Ids
====================

*A utility for dealing with ids in templates.*

The primary aim of lid is to prodive a way for developers to easily, and programmatically generate unique ids, but also retrieve that unique id again to reference it again (whether for a label[for] or as a selector).

lid.js supports **AMD** (no global set) and global usages.

## Usage

### Referencing
#### Global
	<script src="lid.js"></script>

#### AMD
	require(['lid'], function(lid) { });

### Generating Unique Ids
Unique ids can be generated for one time use. This functionality is not new or uncommon to other libraries, this is merely the backbone of lid.js which is exposed if the developer requires it.

	lid.gen(); // 0
	lid.gen(); // 1


### Linking Ids
One of the major problems with templating is not generating unique ids, however it is the ability to retrieve the unique id that was generated without having to store references yourself.
This can grow to be a pain after some time, and your template files will start to look too "codey".

lid.js makes this easy for you...

	lid.link('input1'); // 0
	lid.link('input1'); // 0

	lid.link('select1'); // 1
	lid.link('select1'); // 1
	lid.link('select1'); // 1

By doing this you can access the unique id that was generated first.
Now you might be thinking "but what about if we want to reuse the same template again?"; this is covered, see [flushing](#flushing).

The powerful aspect of referencing it like this is the ability to use it potentially infinitely whilst doing it out of order.

	lid.link('input1'); // 0
	
	lid.link('select1'); // 1
	lid.link('select1'); // 1

	lid.link('input1'); // 0

	lid.link('select1'); // 1

#### Example
If we had a template string like the following:

	<label>My Label</label><input type="text" value="hello">

How would we link the label and input together (to provide full accessibility)?
With lid.js and logic templating engine it is simple.

The following example is using underscores templating engine, with "with" enabled (bad idea, but for cleaner example).

##### Template
	<label for="<% lid.link('box') %>">My Label</label><input id="<% lid.link('box') %>" type="text" value="hello">

##### JS
	_.template(tmpl, { lid: lid });
	// <label for="0">My Label</label><input id="0" type="text" value="hello">

Sometimes however, you really can not be bothered to be creating keys/signatures for each and everything id you want, especially if you have all of your code in the correct order.

This is why you can also pass in a boolean value as the first parameter.

	lid.link(true); // 0
	lid.link(false); // 0

True will start a new reference (which is anonymous), False will always return the last refernce (regardless whether it was created with a key or anonymously).

	lid.link('email'); // 0
	lid.link(false); // 0

	lid.link(true); // 1
	lid.link(false); // 1

	lid.link('email'); // 2
	lid.link(false); // 2

	lid.link(true); // 3
	lid.link(true); // 4

This is very useful if you just need a few elements blocked together.

### Seeding

So, this is all very well and good, but what about if you want to make your ids have a meaning, because it sure is hard to debug with numeric ids, or incase you are scared of collisions.
Both `lid.gen` and `lid.link` provide the ability to seed.

With seeding you can pass in an extra parameter and it will prefix it to your unique id.

	lid.gen('input'); //input0
	lid.link('email', 'email'); //email1

**Note:** Adding a seed to the any calls after the first call will result in the seed being ignored.

	lid.link('email'); 			// 0
	lid.link('email', 'test'); 	// 0

	lid.link('name', 'emp'); 	// emp1
	lid.link('name'); 			// emp1

### <a id="flushing"></a>Flushing

Now you have finished your template, and you have generate some ids, great! But one problem, you can use the same template again... Because if you call `lid.link('email')` again it will return you the old id. 

Damn, so what do we do? We flush the lid. By calling `lid.flush()` all previous refernces are disposed of, and you're free to use it again.

	lid.link('email'); // 0
	lid.flush();
	lid.link('email') // 1


## Extensions

### Parsing


## Dependancies
None