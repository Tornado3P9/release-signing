# release-signing

```bash
curl -fsSLO https://raw.githubusercontent.com/Tornado3P9/release-signing/refs/heads/main/release-public-signing-key.asc
gpg --import release-public-signing-key.asc
gpg --fingerprint 5721682F09E2C6BAAA8050541E6EF85F35741223
```

```bash
gpg --verify SHA256SUMS.asc SHA256SUMS
sha256sum --check SHA256SUMS --ignore-missing
```
