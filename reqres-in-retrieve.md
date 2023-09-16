# Using reqres.in to explorer Postman resources

Let's use [reqres.in](https://reqres.in/) to create some requests with test.

## User list request

Create a GET request and set url to `{{base_url}}/api/users?page={{page}}&per_page={{per_page}}` - name it as `User list`

Set Pre-request Script request to:

``` javascript
const setIfNull = (name, value) => {
    if (!pm.collectionVariables.get(name)) {
        pm.collectionVariables.set(name, value)
    }
}

setIfNull("per_page", 4);
setIfNull("page", 3);
```

This pre-request script will create `page` and `per_page` variables with default value if not exists.

Both are used to pagging users list.

Set tests to:

``` javascript
const per_page = pm.collectionVariables.get("per_page");
const page = pm.collectionVariables.get("page");
const expectedStatus = 200;
const total = 12;
const total_pages = total / per_page;
const result = pm.response.json();

pm.test(`On sucess - it should return status code ${expectedStatus}`, () => {
  pm.expect(pm.response.code).to.eql(expectedStatus);
});

pm.test("On sucess - it should return page on response", () => {
    pm.expect(result.page).to.exist;
});

pm.test("On sucess - it should return expected page", () => {
   pm.expect(result.page).to.be.a('number');
   pm.expect(result.page).to.eql(page);
});

pm.test("On sucess - it should return per_page on response", () => {
    pm.expect(result.per_page).to.exist;
});

pm.test("On sucess - it should return expected per_page", () => {
   pm.expect(result.per_page).to.be.a('number');
   pm.expect(result.per_page).to.eql(per_page);
});

pm.test("On sucess - it should return total on response", () => {
    pm.expect(result.total).to.exist;
});

pm.test("On sucess - it should return expected total", () => {
   pm.expect(result.total).to.be.a('number');
   pm.expect(result.total).to.eql(total);
});

pm.test("On sucess - it should return total_pages on response", () => {
    pm.expect(result.total_pages).to.exist;
});

pm.test("On sucess - it should return expected total_pages", () => {
   pm.expect(result.total_pages).to.be.a('number');
   pm.expect(result.total_pages).to.eql(total_pages);
});

pm.test("On sucess - it should return data on response", () => {
    pm.expect(result.data).to.exist;
});

pm.test("On sucess - it should return expected data length", () => {
   pm.expect(result.data).to.be.a('array');
   pm.expect(result.data.length).to.eql(per_page);
});
```

Run and check result. Expected response is:

``` json
{
    "page": 3,
    "per_page": 4,
    "total": 12,
    "total_pages": 3,
    "data": [
        {
            "id": 9,
            "email": "tobias.funke@reqres.in",
            "first_name": "Tobias",
            "last_name": "Funke",
            "avatar": "https://reqres.in/img/faces/9-image.jpg"
        },
        ...
    ],
    "support": {
        "url": "https://reqres.in/#support-heading",
        "text": "To keep ReqRes free, contributions towards server costs are appreciated!"
    }
}
```

## User by id request

Duplicate last request name it as `User by id`, change url to `{{base_url}}/api/users/:id` and set path param `id` to `{{id}}` variable.

Set Authorization to Bearer Token and in Token use `{{token}}` tag to replace by token when subimit request.

Set tests to:

``` javascript
const expectedStatus = 200;
const expected = {
  id: pm.collectionVariables.get("id"),
  email: "eve.holt@reqres.in",
  first_name: "Eve",
  last_name: "Holt",
  avatar: "https://reqres.in/img/faces/4-image.jpg"
};
const result = pm.response.json();

pm.test(`On sucess - it should return status code ${expectedStatus}`, () => {
  pm.expect(pm.response.code).to.eql(expectedStatus);
});

pm.test("On sucess - it should return data on response", () => {
  pm.expect(result.data).to.exist;
  pm.expect(result.data).to.be.a('object');
  pm.expect(result.data).to.eql(expected);
});

pm.test("On sucess - it should return expected id on data", () => {
  pm.expect(result.data.id).to.exist;
  pm.expect(result.data.id).to.eql(expected.id);
});

pm.test("On sucess - it should return expected email on data", () => {
  pm.expect(result.data.email).to.exist;
  pm.expect(result.data.email).to.eql(expected.email);
});

pm.test("On sucess - it should return expected first_name on data", () => {
  pm.expect(result.data.first_name).to.exist;
  pm.expect(result.data.first_name).to.eql(expected.first_name);
});

pm.test("On sucess - it should return expected last_name on data", () => {
  pm.expect(result.data.last_name).to.exist;
  pm.expect(result.data.last_name).to.eql(expected.last_name);
});

pm.test("On sucess - it should return expected avatar on data", () => {
  pm.expect(result.data.avatar).to.exist;
  pm.expect(result.data.avatar).to.eql(expected.avatar);
});
```

Run and check result. Expected response is:

``` json
{
    "data": {
        "id": 4,
        "email": "eve.holt@reqres.in",
        "first_name": "Eve",
        "last_name": "Holt",
        "avatar": "https://reqres.in/img/faces/4-image.jpg"
    },
    "support": {
        "url": "https://reqres.in/#support-heading",
        "text": "To keep ReqRes free, contributions towards server costs are appreciated!"
    }
}
```

## User by id not found request

Duplicate previous request and change it's path param `id` to `{{id}}*1000`

Set tests to:

``` javascript
const expected = {};
const expectedStatus = 404;
const result = pm.response.json();

pm.test(`On not found - it should return status code ${expectedStatus}`, () => {
  pm.expect(pm.response.code).to.eql(expectedStatus);
});

pm.test("On not found - it should return data on response", () => {
  pm.expect(result).to.exist;
});

pm.test("On not found - it should return expected data on response", () => {
  pm.expect(result).to.be.a('object');
  pm.expect(result).to.eql(expected);
});
```

Run and check result. Expected response is:

``` json
{}
```
