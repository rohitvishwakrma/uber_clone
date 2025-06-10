# `/users/register` Endpoint Documentation

## Endpoint

`POST /users/register`

## Description
Registers a new user in the system. This endpoint validates the input, checks for existing users, hashes the password, creates a new user, and returns an authentication token along with the user data.

## Request Body
Send a JSON object with the following structure:

```
{
  "fullname": {
    "firstname": "string (min 6 chars, required)",
    "lastname": "string (min 6 chars, optional)"
  },
  "email": "string (valid email, min 10 chars, required)",
  "password": "string (min 8 chars, required)"
}
```

### Example
```
{
  "fullname": {
    "firstname": "Johnathan",
    "lastname": "Doe"
  },
  "email": "john.doe@example.com",
  "password": "password123"
  
}
```

## Responses

- **201 Created**
  - User registered successfully.
  - Returns:

    ```json
    {
      "token": "<jwt_token>",
      "user": {
        "_id": "<user_id>",
        "fullname": {
          "firstname": "Johnathan",
          "lastname": "Doe"
        },
        "email": "john.doe@example.com",
        // ...other user fields
      }
    }
    ```

- **400 Bad Request**
  - Validation failed or user already exists.
  - Returns: `{ errors: [...] }` or `{ message: 'User already exist' }`

    Example:
    ```json
    {
      "errors": [
        { "msg": "Invalid Email", "param": "email", "location": "body" }
      ]
    }
    ```
    or
    ```json
    { "message": "User already exist" }
    ```

- **Other Errors**
  - Standard error response with appropriate status code and message.

## Notes
- The password is securely hashed before storage.
- The response includes a JWT token for authentication.
- The endpoint expects the request body in JSON format.
