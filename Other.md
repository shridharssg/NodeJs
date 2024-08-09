### HTTP POST request
  We use POST to create a new resource. A POST request requires a body in which you define the data of the entity to be created.

  A successful POST request would be a 200 response code

  if you execute a POST request multiple times, you'll create a new resource multiple times despite them having the same data being passed in.

### PUT
We use PUT to modify a resource. PUT updates the entire resource with data that is passed in the body payload. If there is no resource that matches the request, it will create a new resource.

if we hit multiple PUT requests will update the same existing order.

with PUT you need to pass in data to update the entire resource, even if you only want to modify one field.

### PATCH : 

With PATCH, you can update part of a resource by simply passing in the data of the field to be updated.

---
