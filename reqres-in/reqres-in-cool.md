# Using reqres.in with cool stuffs

How about try some cool stuffs that can be used on Postman? So, let's go.

## Pre-request to validate body request

Open `Pre-request Script` of `Register undefined user error` and put this code:

``` javascript
const body = JSON.parse(pm.request.body.raw);

// check with to.exists
pm.test("Request body - it should have email property", () => {
    pm.expect(body.email).to.exist;
});

// check with has.property
pm.test("Request body - it should have password property", () => {
    pm.expect(body).to.be.an('Object').that.has.property('password');
});

// skip a javascript check with expect.fail if property not defined
pm.test.skip("Request body - it should have password property skipped", () => {
    if (!body.email) {
        pm.expect.fail('Ops, body must have email property.');
    }
});
```
What is it:

 * first we parse request body content
 * then we check if exists email property with `to.exist` approach
 * then we check if body is an object that contains `password` with `has.property` method
 * finally, skips a test that check if exists email property using javascript pattern

## Pre-request to validate body request v2

Open `Pre-request Script` of `Register valid user` and put this code:

``` javascript
// ES6 object destructuring
const { email, password } = JSON.parse(pm.request.body.raw);

// check with to.exists
pm.test("Request body - it should have email property", () => {
    pm.expect(email).to.exist;
});

// check with javascript pattern
pm.test("Request body - it should have password property skipped", () => {
    if (!password) {
        pm.expect.fail('Password is undefined');
    }
});
```

What is it:

 * first we parse request body content and put `email` and `password` in variables
 * then we check if email was sent with `to.exist` approach
 * then we check if password was sent with javascript approach

## Improve tests of User list

Open `Pre-request Script` of `User list` and put this code:

``` javascript
const schema = {
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Generated schema for Root",
  "type": "object",
  "properties": {
    "page": {
      "type": "number"
    },
    "per_page": {
      "type": "number"
    },
    "total": {
      "type": "number"
    },
    "total_pages": {
      "type": "number"
    },
    "data": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "id": {
            "type": "number"
          },
          "email": {
            "type": "string"
          },
          "first_name": {
            "type": "string"
          },
          "last_name": {
            "type": "string"
          },
          "avatar": {
            "type": "string"
          }
        },
        "required": [
          "id",
          "email",
          "first_name",
          "last_name",
          "avatar"
        ]
      }
    },
    "support": {
      "type": "object",
      "properties": {
        "url": {
          "type": "string"
        },
        "text": {
          "type": "string"
        }
      },
      "required": [
        "url",
        "text"
      ]
    }
  },
  "required": [
    "page",
    "per_page",
    "total",
    "total_pages",
    "data",
    "support"
  ]
}
```

This is a schema to validate response data.

Open `Tests` of `User list` and add this code at begining:

``` javascript
const contentType = "application/json; charset=utf-8";
const responseTimeMS = 500;
```

Now, at the end try add this tests:

``` javascript
pm.test('On sucess - it should return data to schema validation', () => {
  var schema = pm.variables.get("schema");
   pm.expect(tv4.validate(result, schema)).to.be.true;
});

pm.test('On sucess - it should return content type header', () => {
  pm.response.to.have.header("Content-Type");
  pm.expect(pm.response.headers.get("Content-Type")).to.eql(contentType);
});

pm.test(`On sucess - it should return in less than ${responseTimeMS}ms`, () => {
  pm.expect(pm.response.responseTime).to.be.below(responseTimeMS);
});

(result.support === undefined ? pm.test.skip : pm.test)
    ('On sucess - it should return support property', () => {
        pm.expect(result.support).to.be.a('Object')
});
```

What is it:

 * first we check if response body match with schema defined previously
 * then we check if headers contains `Content-Type` header
 * then we check if `Content-Type` header value
 * then we check if response time was below defined time in ms
 * finally a conditional validation if response comes with `support` property or not
   - if `support` property not found, skips test
   - if `support` property found, checks if it is an object
