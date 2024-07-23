# JWT Auth Tokens

## Use ioctl to issue a JWT <a href="#use-ioctl-to-issue-jwt" id="use-ioctl-to-issue-jwt"></a>

Every account ioctl created contains a pair of 256-bit private/public key. We can use it to sign and issue JWT:

```json
➜  ioctl jwt sign --with-arguments '{"exp":"1608193125","sub":"weather","scope":"Create"}' -s my_account -y
Enter password #my_account
```

Enter your password to sign the token:

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Read More about JWT Tokens</summary>

### What is JWT <a href="#what-is-jwt" id="what-is-jwt"></a>

JWT (JSON Web Token) is a very popular technology widely used in web API and user authentication. It contains certain access control claims, such as what data/resource can be access, the access expire time, and access rights (read, write, or delete).

The token is base64-encoded and digitally signed using a secret (with the _HMAC_ algorithm) or a private key. By verifying the signature it can be guaranteed that the claims must come from the holder of the signing key.

In a nutshell, JWT consists of three parts separated by dot . , which are

* Header
* Payload
* Signature

### Example

Here is an example of a JWT encoded token:

```
eyJhbGciOiJFUzI1NiIsInR5cCI6IkpXVCJ9.
eyJleHAiOjE2MDU4NzIyNDksImlhdCI6MTYwODE2ODQ0NywiaXNzIjoiMHgwNDFkMjRiNDc0ZjM5YzVmMTBlNjlmZmNmMzhlZjA4ZmViY2U4ZTNkMGZmNWFjOWI0YzMzNjA2OWI1ZDEwYmNjMGZjN2MxNDNhNDQwMTRmMTQ5YWFkNzQ3YWMwNTJmNzhmMDZiODA3M2I0YzA0NWI0NGJlMWFiYTIzMTM3ZTcxNjFlIiwic3ViIjoid2VhdGhlciIsInNjb3BlIjoiQ3JlYXRlIn0.
i3KHZTmF1jWKIDSBOF1BWEg4G7C4H7BIdshS0uzz45687UU3K2Uzey3R5Qs7QrHrsU0J99PeR7i_Qc03wVYVjw
```

Decoding the header part:

```
eyJhbGciOiJFUzI1NiIsInR5cCI6IkpXVCJ9
```

gives the following header, indicating it is signed by ES256 algorithm (256-bit Elliptic-curve Signature):

```
{
  "alg": "ES256",
  "typ": "JWT"
}
```

Decoding the payload part:

```
eyJleHAiOjE2MDU4NzIyNDksImlhdCI6MTYwODE2ODQ0NywiaXNzIjoiMHgwNDFkMjRiNDc0ZjM5YzVmMTBlNjlmZmNmMzhlZjA4ZmViY2U4ZTNkMGZmNWFjOWI0YzMzNjA2OWI1ZDEwYmNjMGZjN2MxNDNhNDQwMTRmMTQ5YWFkNzQ3YWMwNTJmNzhmMDZiODA3M2I0YzA0NWI0NGJlMWFiYTIzMTM3ZTcxNjFlIiwic3ViIjoid2VhdGhlciIsInNjb3BlIjoiQ3JlYXRlIn0.
```

gives the following claims:

```
{
  "exp": 1608193125,
  "iat": 1608168517,
  "iss": "0x041d24b474f39c5f10e69ffcf38ef08febce8e3d0ff5ac9b4c336069b5d10bcc0fc7c143a44014f149aad747ac052f78f06b8073b4c045b44be1aba23137e7161e",
  "sub": "weather",
  "scope": "Create"
}
```

where:

* `"exp"` is the token's expiration time
* `"iat"` is the token's issue time (you can convert date/time [here](https://www.unixtimestamp.com/))
* `"iss"` is the public key of issuer
* `"sub"` is the subject, here it refers to a resource/data named **weather**
* `"scope"` is the access control rights granted for the resource, here it allows to **create**

The signature in our example is:

```
i3KHZTmF1jWKIDSBOF1BWEg4G7C4H7BIdshS0uzz45687UU3K2Uzey3R5Qs7QrHrsU0J99PeR7i_Qc03wVYVjw
```

that can be verified against the issuer public key `iss` above.

</details>

