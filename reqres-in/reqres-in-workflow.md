# Building request workflows

When you start a collection run, Postman runs all requests in the same order they appear in your collection.

Requests in folders are executed first, followed by any requests in the collection root.

If all steps were followed now we have a collection with:

 * POST - Register undefined user error
 * POST - Register valid user
 * POST - Login password error
 * POST - Login email error
 * POST - User login
 * GET - User list
 * GET - User by id
 * GET - User by id not found
 * PATCH - Patch user by id

And that's will be the execution order if we left as is, so, let's change this a little.

Set a request workflow is done using `postman.setNextRequest()` function that receive one argument that can be the name or the id of request to be set to the next execution request.

It's important to know some rules when use workflow with setNextRequest function:

 * it only works when you run an entire collection - click on send will not trigger your workflow
 * it must be set in pre-request or test scripts. I suggest put at the end of your test but it may put on first test line, for example
 * use next request with the request ID, so if request name changes workflow still works
 * setNextRequest() scope is limited to the collection. It's not able to jump to a request in another collection

To change execution order, we need put `postman.setNextRequest("request_name")` at `Tests` so, to a better understanding, let use request name:

 * keep `Register undefined user error` as your first request in collection list
 * open `Test` of `Register undefined user error`
 * put `postman.setNextRequest("Login password error")` at the end of tests
 * open `Test` of `Login password error`
 * put `postman.setNextRequest("Login email error")` at the end of tests

This changes will set execution request errors sequence.

 * open `Test` of  `Login email error`
 * put `postman.setNextRequest("Register valid user")` at the end of tests
 * open `Test` of  `Register valid user`
 * put `postman.setNextRequest("User login")` at the end of tests
 * open `Test` of  `User login`
 * put `postman.setNextRequest("User list")` at the end of tests
 * open `Test` of  `User list`
 * put `postman.setNextRequest("User by id")` at the end of tests
 * open `Test` of  `User by id`
 * put `postman.setNextRequest("Patch user by id")` at the end of tests
 * open `Test` of  `Patch user by id`
 * put `postman.setNextRequest("User by id not found")` at the end of tests
 * open `Test` of  `User by id not found`
 * put `postman.setNextRequest(null)` at the end of tests

On last one, `postman.setNextRequest(null)` will stop workflow requests execution.

Now, let's check if it works:

 * click on `...` collection menu on right of collection name
 * choose `Run collection`, check that request order
 * click `Run reqres.in APIs` button
 * check execution order

Is execution the expected?

 * Register undefined user error
 * Login password error
 * Login email error
 * Register valid user
 * User login
 * User list
 * User by id
 * Patch user by id
 * User by id not found

If some request name changes, remember to change the `Tests` that was defined as next request to de executed.
