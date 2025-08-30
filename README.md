# LibreChat with External JWT support

This repository patches the [official LibreChat repository](https://github.com/danny-avila/LibreChat) to add:

- External JWT authentication
- Customizable `SameSite` cookie attribute

# How to use

You need to replace the image used in the `docker-compose.yaml` with [this one](https://github.com/users/Caceresenzo/packages/container/package/librechat-external-jwt).

```diff
 services:
   api:
-    image: ghcr.io/danny-avila/librechat:v0.8.0-rc3
+    image: ghcr.io/caceresenzo/librechat-external-jwt:v0.8.0-rc3
     container_name: LibreChat-API
     ports:
```

## New configuration options

By default, the patch does not change anything; the software will work as expected.

| Env. Variable | Description |
| --- | --- |
| `OPENID_ALLOW_EXTERNAL_JWT` | Expose the `/oauth/external-jwt?id_token=` endpoint to authenticate the configured OpenID with an ID Token. |
| `COOKIES_SAME_SITE` | Configure the `SameSite` attribute for authentication cookies. If unset, default to `strict`. Allowed values are `none`, `lax` and `strict`. |

> [!NOTE]
> These features were originally made to work with Cognito:
> - Support for external JWT, as Amplify does not like when the App Client has a secret (which are necessary for LibreChat).
> - Cookie SameSite for an integration into an IFrame.

# Improve the patch

A patch for one version might not work for another one.

To continue working on the patch, you must:

1. Clone the [LibreChat repository](https://github.com/danny-avila/LibreChat):
```bash
git clone https://github.com/danny-avila/LibreChat.git
cd LibreChat
```

2. `git checkout` to a stable tag:
```bash
git checkout v0.8.0-rc3
```

3. Apply the current patch:
```bash
git apply ../external-jwt.patch
```

4. Do your edits.

5. `git add` the new edits (likely only in the `api/` directory):
```bash
git add api/
```

6. Export the new patch:
```bash
git diff --cached > ../external-jwt.patch
```

> [!IMPORTANT]
> If you are patching a newer tag, don't forget to update the `base.json` file.

# Build a new image

The GitHub action that builds the image is not triggered automatically and must be started manually.

To do that:
1. Go on [librechat-external-jwt/actions/workflows/build.yaml](https://github.com/Caceresenzo/librechat-external-jwt/actions/workflows/build.yaml).
2. Click on "Run workflow" (in gray).
3. Click on "Run workflow" (in green).

> [!NOTE]
> Building the image takes **40 minutes**. <br />
> The best way to test the changes is to [test them locally first](https://www.librechat.ai/docs/quick_start/local_setup#step-3-run-the-app).
