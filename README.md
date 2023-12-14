# SD JWT Test Cases

## Introduction

This repository contains the test cases for the [SD JWT](https://www.ietf.org/archive/id/draft-ietf-oauth-selective-disclosure-jwt-06.html) project. Most of the test cases are based on the [sd-jwt-python](https://github.com/openwallet-foundation-labs/sd-jwt-python/tree/main/tests/testcases) framework.

The test cases are written in JSON to ease the integration with other programming languages.

## what is included in the test cases

```json
{
  "claims": "...",
  "disclosureFrame": "...",
  "holder_disclosed_claims": "...",
  "expect_verified_user_claims": "..."
}
```

- `claims`: the claims to be signed by the issuer.
- `disclosureFrame`: which data can be selective disclosure.
- `holder_disclosed_claims`: the claims that selective disclosure by the holder.
- `expect_verified_user_claims`: the claims to be verified by the verifier.

## How to use the test cases

### Claims

Claims are the data that the issuer wants to sign. It is a JSON object.

```json
{
  "claims": {
    "sub": "john_deo_42",
    "given_name": "John",
    "family_name": "Deo",
    "email": "johndeo@example.com",
    "phone": "+1-202-555-0101",
    "address": {
      "street_address": "123 Main St",
      "locality": "Anytown",
      "region": "Anystate",
      "country": "US"
    },
    "birthdate": "1940-01-01"
  },
  ...
}
```

### disclosureFrame

The disclosureFrame is an Array of strings that represents the claims that can be disclosed by the holder.

The data is the JSON path property of the claims.

For example,

```json
{
  "a": {
    "b": {
      "c": "d"
    }
  }
}
```

The JSON path of `d` is `a.b.c`.

And we use number string to represent the array index. like this:

```json
{
  "a": {
    "b": ["c", "d"]
  }
}
```

The JSON path of `d` is `a.b.1`.

So the disclosureFrame of the Claims can be:

```json
{
  "disclosureFrame": [
    "given_name",
    "family_name",
    "email",
    "phone",
    "address",
    "birthdate"
  ],
  ...
}
```

### holder_disclosed_claims

The holder_disclosed_claims is an Array of strings that represents the claims that the holder wants to disclose to the verifier.

It's subset of the disclosureFrame.

```json
{
  "holder_disclosed_claims": [
    "given_name",
    "family_name",
  ],
  ...
}

```

### expect_verified_user_claims

The expected_verified_user_claims is an disclosd claims that the verifier wants to verify.

```json
{
  "expect_verified_user_claims": {
    "given_name": "John",
    "family_name": "Deo",
  },
  ...
}
```
