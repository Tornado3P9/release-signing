# release-signing

## Installation
```bash
# Linux
sudo apt install gnupg

# Mac
brew install gnupg

# Windows
# Install Gpg4win https://www.gnupg.org/download/ https://gpg4win.org/download.html

# Gpg4win: On Windows, Kleopatra is bundled as part of the Gpg4win suite,
# making it the primary tools for managing GnuPG (GPG) encryption on Windows.
# Kleopatra is also bundled as part of GnuPG Desktop®️ which comes with
# enterprise support and as part of GnuPG VS-Desktop®️ which is compliant
# for use with EU and NATO RESTRICTED material and the German VS-NfD.
# https://apps.kde.org/kleopatra/
# Kleopatra is a certificate manager and GUI for GnuPG.
# It is a KDE application, so it’s part of the Linux/KDE ecosystem:
# sudo apt install kleopatra scdaemon
```

## Key import and verification
```bash
curl -fsSLO https://raw.githubusercontent.com/Tornado3P9/release-signing/refs/heads/main/release-public-signing-key.asc
gpg --import release-public-signing-key.asc
gpg --fingerprint 5721682F09E2C6BAAA8050541E6EF85F35741223

gpg --verify SHA256SUMS.asc SHA256SUMS
```

## Verification of SHA256 Hashes
```bash
# Linux
sha256sum --check SHA256SUMS --ignore-missing

# Linux/Mac
shasum -a 256 --check SHA256SUMS --ignore-missing

# Windows
Get-Content .\SHA256SUMS | ForEach-Object {
    if ($_ -match '^([0-9a-fA-F]{64})\s+[* ](.+?)\s*$') {
        $expected = $Matches[1].ToLower()
        $file = $Matches[2]

        if (Test-Path -LiteralPath $file -PathType Leaf) {
            $actual = (Get-FileHash -LiteralPath $file -Algorithm SHA256).Hash.ToLower()
            if ($actual -eq $expected) {
                "$file`: OK"
            } else {
                "$file`: FAILED"
            }
        }
    }
}
# (Windows) Optionally generate the hash and manually check if it is the same as in the SHA256SUMS text file
Get-FileHash .\filename -Algorithm SHA256
certutil -hashfile filename SHA256
openssl dgst -sha256 filename
```
