Conduit
==============
Documentation for the PHP routing engine.

The Main purpose of Falcon is to direct URI Requests to processing files.
Example of a route:

Here is an example url request:

`https://api.example.com/1.0/user/2234/details`

This is broken down as:

`https://` `api.example.com`/`1.0`/`user/2234/details`

or

`protocol` `domain` `version` `request route`

What Falcon gets to work with is `version` `request route`.

The `request route` is broken down into parts, or a `path`. The path for this example is `user` `2234` `details`.

This path is now matched against the resource rules of the requested version.

The router loads up all the defined rules and sequentually 

### Directory Structure

All request are pushed to Falcon's `index.php` file, where it takes over the routing and loading of resources. This is a common practice used in "clean" urls and is referred to as `url rewrite`.

Versioning is a big part of Falcon as it allows you to work on multiple versions within the same platform and not cause older integrations to fail. If you are not wanting to use versioning, its save to simply call your version `api` e.g. `https://example.com/api/my-route`

The directory structure of falcon:
```
/
|-- version
|    |-- methods
|    |    +-- a_method.php
|    |-- resources
|    |    |-- routes.json
|    |    +-- app-globals.json
|    +-- static
|         |-- css
|         |-- images
|         +-- js
+-- index.php
```

There is no limit to the amount of resource .json files. This allows you to group your routes as you like. Grouping them makes for easier management on large projects.

The `methods` folder can be anything and is not governed by Falcon. It's advised to create multiple folders to help organize routing. Use meaningfull folder names. `methods`, `templates`, `libs` are good names, while `files`, `php`, `stuff` are not.

The `resources` folder is governed by Falcon. It's where the routing rules are saved.

The `static` folder is governed by Falcon and the `.htaccess` and `falcon.conf` examples for apache and lighttpd respectivly, have reqrite rules for the `static` folder to be handled as a static file handler. You would use this to store fiels like styles, scripts and images if needed.

### Defining Route Rules

A route rule is defined through a `.json` file found in the `version/resources` folder. A rules file is an array or rule objects. It can contain as many rules as is needed and rules can be spread accross multiple files.

An example rule with all available properties set: 

```
[
	{
		"name"      :   "user/:id/:action",
		"desc"      :   "Route Description - a note to me so I know what I made it.",
		"debug"     :   true,
		"my_custom" :   {
		    "key"   :   "value",
		    "settings"  :   {
		        "username"  :   "bob",
		        "user_id"   :   "234"
		    }
		},
		"headers"   :	{
			"Content-Type"  :   "text/javascript",
			"Age"           :   "7"
		},
		"libraries" :   [
			"libs/validate_request.php",
			"libs/mysql.php
		],
		"methods"   :   {
			"GET"   :   {
				"headers"   :   {
					"Access-Control-Allow-Origin"   :   "*"
				},
				"libraries" :   [
					"libs/lib_for_get_request.php"
				],
				"file"      :   "methods/get_handler.php"
			},
			"POST"  :   "methods/post_handler.php"
		}
	}
]
```
An example with the minimum required properties:
```
[
	{
		"name"      :   "stuff",
		"methods"   :   {
			"GET"   :   "get_stuff.php"
		}
	}
]

```

An example containing 3 rules in a single file:
```
[
	{
		"name"          :   "user/:id",
		"desc"          :   "Example showing multiple verbs for a single route.",
		"methods"       :   {
			"GET"       :   "users/get_user.php",
			"POST"      :   "users/add_user.php",
			"DELETE"    :   "users/delete_user.php"
		}
	},
	{
		"name"          :   "user/:id/:action",
		"desc"          :   "Example showing single verb for a route.",
		"methods"       :   {
			"GET"       :   "users/get_user.php"
		}
	},
	{
		"name"          :   "users/list",
		"desc"          :   "Example showing single verb for a static route.",
		"methods"       :   {
			"GET"       :   "users/list.php"
		}
	}
]

```







