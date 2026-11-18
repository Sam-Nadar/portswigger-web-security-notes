# Path Traversal: Validation of Start of Path

## Lab Information

- **Platform:** PortSwigger Web Security Academy
- **Category:** Path Traversal
- **Difficulty:** Practitioner
- **Status:** Solved
- **Tools:** Burp Suite Proxy

## Vulnerability Overview

Path Traversal vulnerabilities occur when applications fail to properly validate user-supplied file paths, allowing attackers to access files outside the intended application directory.

In this lab, the application validated that the supplied filename started with the expected directory path (`/var/www/images/`). However, it failed to verify the complete resolved file path. By providing the expected directory followed by directory traversal sequences (`../../../`), it was possible to bypass the validation and access the `/etc/passwd` file.

## Testing Process

1. Browse the application and intercept a request responsible for fetching a product image using Burp Suite Proxy.
2. Identify the `filename` parameter responsible for specifying the image file.
3. Modify the `filename` parameter using the following payload:

```text
/var/www/images/../../../etc/passwd
```

4. Forward the modified request to the server.
5. Observe that the response contains the contents of `/etc/passwd`, confirming that validating only the beginning of the file path can be bypassed.
6. Successfully complete the lab.

## Evidence

**### Modified Request with Path Validation Bypass**

![Path traversal payload](./evidence/path-validation-payload.png)

**### Contents of /etc/passwd in Response**

![Etc passwd response](./evidence/etc-passwd-response.png)

**### Lab Completion**

![Lab solved confirmation](./evidence/ls-path-traversal-05.png)

## Impact

- Allows unauthorized access to files outside the intended application directory.
- Can expose sensitive system files and configuration data.
- May disclose information about the server environment.
- Can lead to further system compromise depending on accessible files.

## Remediation

- Avoid directly using user-controlled input in file system operations.
- Canonicalize file paths before performing validation.
- Verify that the complete resolved path remains within the intended application directory.
- Use a strict allowlist of permitted filenames.
- Apply the principle of least privilege to application file system permissions.

## References

- [PortSwigger: Path Traversal](https://portswigger.net/web-security/file-path-traversal)
- [PortSwigger: Validation of Start of Path](https://portswigger.net/web-security/file-path-traversal/lab-validate-start-of-path)
- [OWASP: Path Traversal](https://owasp.org/www-community/attacks/Path_Traversal)