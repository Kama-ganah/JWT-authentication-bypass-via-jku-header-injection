# Overview
During testing of the application’s JWT-based authentication, I discovered a critical flaw in the handling of the jku header. The server fails to validate the origin of the URL provided in this header, allowing an attacker to host a malicious JSON Web Key Set (JWKS) and forge tokens for any user. By creating a JWT signed with a self-generated key and pointing the jku header to it, I successfully impersonated the administrator and performed privileged actions, including deleting a user account. This demonstrates a severe authentication bypass with full account compromise potential.

# Steps Undertaken

Step 1: Captured a legitimate JWT from a standard user session.

Step 2: Generated a custom RSA key pair and hosted the public key on an attacker-controlled server.

Step 3: Modified the JWT header to include the jku pointing to the hosted key and updated the payload to impersonate the admin.

Step 4: Re-signed the JWT with the private key and sent it to access the admin panel.

Step 5: Verified impact by deleting a selected user account using the elevated privileges.

# Conclusion

This assessment confirmed a high-severity JWT authentication bypass via jku header injection. Remediation requires validating or disallowing untrusted jku URLs, using local key verification, and enforcing strict signature checks. Proper JWT handling and cryptographic controls are essential to prevent privilege escalation and full account compromise.
