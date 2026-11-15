---
"@better-auth/oauth-provider": minor
---

The new `beforeTokenIssuance` option lets you refuse tokens for any grant after the grant is validated and before any token is stored.

It runs once per token request for the authorization code, refresh token, and client credentials grants, and for every extension grant that calls `issueTokens`, including device code, CIBA, and token exchange. It receives the client, user, scopes, resources, `acr`, `amr`, `act`, authorization details, the DPoP `confirmation` the tokens will be bound to, the `grantId`, and the grant's stored request: `verificationValue` for an authorization code, or the `grantRequest` an extension grant passes to `issueTokens`. Throw an `APIError` with an OAuth error, usually `invalid_grant`, to refuse; the token endpoint returns it unchanged. A refused refresh does not rotate the refresh token, and a refused device code or CIBA request stays approved for another poll. An authorization code is already used up when the hook runs. A provider extension can set its own `beforeTokenIssuance`, which runs before the option's.

A grant with a one-time credential passes `consumeCredential` to `issueTokens`, and the provider consumes the credential only after every hook accepts.
