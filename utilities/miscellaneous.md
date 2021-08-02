# Miscellaneous

There are many useful functions that can be used in many places to avoid duplicating code, you can see a list here :

## Errors

* [argError](https://advanced-command-handler.github.io/docs/modules.html#argError) : Send an argument error.
* [codeError](https://advanced-command-handler.github.io/docs/modules.html#codeError) : Send an internal error.
* [permissionsError](https://advanced-command-handler.github.io/docs/modules.html#permissionsError) : Send a permission error.

## Arguments

* [getThing](https://advanced-command-handler.github.io/docs/modules.html#getThing) : Get easily arguments of a given type.

## JSON Utilities

* [saveJSON](https://advanced-command-handler.github.io/docs/modules.html#saveJSON) : Saves a JSON safely, returning a boolean meaning if it worked.
* [readJSON](https://advanced-command-handler.github.io/docs/modules.html#readJSON) : Reads a JSON, prefer using `import` or `require`.

## Permissions

* [isOwner](https://advanced-command-handler.github.io/docs/modules.html#isOwner) : Returns `true` if the given ID is included in the owners set in [`CommandHandler.create`](https://advanced-command-handler.github.io/docs/modules/commandhandler.html#create-1).
* [isPermission](https://advanced-command-handler.github.io/docs/modules.html#isPermission) : Returns `true` if the given string is a valid permission. It's also a type guard for `PermissionString` type for TypeScript users.

## Other

* [cutIfTooLong](https://advanced-command-handler.github.io/docs/modules.html#cutIfTooLong) : Cut a text at a given length \(if too long\).
* [getKeyByValue](https://advanced-command-handler.github.io/docs/modules.html#getKeyByValue) : Get a key from a value in an object.
* [random](https://advanced-command-handler.github.io/docs/modules.html#random) : Returns a random value from an array.

