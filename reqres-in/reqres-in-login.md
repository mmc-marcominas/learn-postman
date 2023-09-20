# Using reqres.in to explore Postman resources

Let's use [reqres.in](https://reqres.in/) to create some requests with test.

## Login password error request

Create a POST request and set url to `{{base_url}}/api/login` - name it as `Login password error`

Set request body to:

``` json
{
    "email": "marco.almeida@reqres.in"
}
```

Set tests to:

``` javascript
const expectedStatus = 400;
const result = pm.response.json();
const expectedError = "Missing password";

pm.test(`On error - it should return status code ${expectedStatus}`, () => {
  pm.expect(pm.response.code).to.eql(expectedStatus);
});

pm.test("On error - it should return error on response", () => {
    pm.expect(result.error).to.exist;
});

pm.test("On error - it should return expected error", () => {
   pm.expect(result.error).to.be.a('string');
   pm.expect(result.error).to.eql(expectedError);
   pm.expect(result.error).to.have.lengthOf(expectedError.length);
});
```

Run and check result. Expected response is:

``` json
{
    "error": "Missing password"
}
```

## Login email error request

Duplicate last request and name it as `Login password error`

Set request body to:

``` json
{
    "password": "cityslicka"
}
```

Set tests to:

``` javascript
const expectedStatus = 400;
const result = pm.response.json();
const expectedError = "Missing email or username";

pm.test(`On error - it should return status code ${expectedStatus}`, () => {
  pm.expect(pm.response.code).to.eql(expectedStatus);
});

pm.test("On error - it should return error on response", () => {
    pm.expect(result.error).to.exist;
});

pm.test("On error - it should return expected error", () => {
   pm.expect(result.error).to.be.a('string');
   pm.expect(result.error).to.eql(expectedError);
   pm.expect(result.error).to.have.lengthOf(expectedError.length);
});
```

Run and check result. Expected response is:

``` json
{
    "error": "Missing email or username"
}
```

## User login request

Duplicate previous request and change it's body to:

``` json
{
    "email": "eve.holt@reqres.in",
    "password": "marco-vei"
}
```

Set tests to:

``` javascript
const expectedToken = "QpwL5tke4Pnpja7X4";
const expectedStatus = 200;
const result = pm.response.json();

if (result.token) {
    pm.collectionVariables.set("token", result.token);
}

pm.test(`On sucess - it should return status code ${expectedStatus}`, () => {
  pm.expect(pm.response.code).to.eql(expectedStatus);
});

pm.test("On sucess - it should return token on response", () => {
    pm.expect(result.token).to.exist;
});

pm.test("On sucess - it should return expected token", () => {
   pm.expect(result.token).to.be.a('string');
   pm.expect(result.token).to.eql(expectedToken);
});
```

Run and check result. Expected response is:

``` json
{
    "token": "QpwL5tke4Pnpja7X4"
}
```

Check if collection variable contains `token` equal to `QpwL5tke4Pnpja7X4`.
