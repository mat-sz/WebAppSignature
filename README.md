# WebAppSignature

A proposal for creating a system for client-side cryptographic validation of web application code and assets.

## Motivation

With the advent of client side rendering and browser-based cryptography (WebCrypto), much more data processing is done on the client side nowadays than ever before. To provide users with the highest level of security possible, cryptographic work is usually performed in the browser and encryption secrets ideally should never leave that environment. One of the challenges such systems face is the possibility of the frontend code being replaced on the end server (or a reverse proxy if one is being used) by malicious actors. WebAppSignature aims to protect against such attacks by allowing for web applications to be signed with a key that should not leave the build server and thus might be significantly more difficult to obtain for attackers.

## Challenges

The main challenges this system faces are the user experience, the trust in keys used to sign the application and the potential issues that arise when the application is replaced by the attacker before the user has the chance to use the application for the first time.

With certain trustless cryptographical systems, the user has to verify public key hashes manually. For example, in a certain secure instant messaging software, a prompt will be shown asking for the user to contact the other party via another channel to check whether the hash shown is correct. To mitigate this issue, a trust system (such as the one currently used for SSL certificates) may be utilized. This would also resolve the key trust issue, although this would introduce a centralization element into the system.

For the "first visit" issue, a DNS TXT record may be used to indicate URLs that must include a signature.

## Possible implementation details

Since a HTML document is usually the entry point to any given web application, I would suggest adding an attribute to the root element (`<html>`) that would indicate if the app should be verified. Other attributes would indicate if third-party resources should be signed, other (non-script) assets require signatures or requested behavior if signature is missing in the future.

### Root element attributes

- `signature-require` - if present, the browser will verify the signature of the current `.html` file and all `.js` files loaded from that page.
- `signature-require-asset` - if present the browser will also verify the signature of any other loaded resources.
- `signature-require-third-party` - if present the browser will verify signatures for resources loaded from other origins.

### Public key file location and format

To be decided.

### Signature file location and format

#### Location

| URL | Signature file URL | Explanation |
| `https://example.com/` | `https://example.com/.sig` | The signature for a directory path is expected to be in a file with no name, just the `.sig` extension. |
| `https://example.com/directory/` | `https://example.com/directory/.sig` | Same as above. |
| `https://example.com/index.html` | `https://example.com/index.html.sig` | For any other files, `.sig` is appended to the file name. |

#### Format

To be decided.

## Example implementation

A Chrome extension is currently under development.
