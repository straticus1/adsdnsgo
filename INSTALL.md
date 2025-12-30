# Installation Guide

## Requirements

- Go 1.21 or later
- Git

## Install via Go

```bash
go install github.com/afterdarksystems/adsdnsgo@latest
```

The binary will be installed to `$GOPATH/bin/adsdnsgo`.

## Build from Source

### Clone Repository

```bash
git clone https://github.com/afterdarksystems/adsdnsgo.git
cd adsdnsgo
```

### Build

```bash
go build -o dnsgo ./cmd/adsdnsgo
```

### Install System-wide

```bash
sudo mv dnsgo /usr/local/bin/
```

## Verify Installation

```bash
dnsgo --version
dnsgo query example.com
```

## Configuration

### Create Config Directory

```bash
mkdir -p ~/.config/adsdnsgo
```

### Create Configuration File

```bash
cat > ~/.config/adsdnsgo/config.json << 'EOF'
{
  "version": "1.0",
  "defaults": {
    "output_level": "long",
    "color": true,
    "timeout": "10s",
    "retries": 3
  },
  "resolvers": {
    "default": ["8.8.8.8", "1.1.1.1"],
    "dnssec_validation": ["9.9.9.9"]
  }
}
EOF
```

## Optional: dnsscience.io Setup

To enable dnsscience.io integration:

1. Obtain API key from https://dnsscience.io
2. Save the key file:
   ```bash
   mv ~/Downloads/dnsscience-api-key.json ~/.config/adsdnsgo/
   ```
3. Configure the key:
   ```bash
   dnsgo science key set ~/.config/adsdnsgo/dnsscience-api-key.json
   ```

## Optional: DDI Appliance Setup

### Infoblox

```bash
dnsgo appliance set infoblox \
  --url https://infoblox.example.com \
  --username admin \
  --password-file ~/.config/adsdnsgo/infoblox-pass
```

### BlueCat

```bash
dnsgo appliance set bluecat \
  --url https://bluecat.example.com \
  --api-key-file ~/.config/adsdnsgo/bluecat-key
```

## Environment Variables

| Variable | Description |
|----------|-------------|
| `ADSDNSGO_CONFIG` | Path to config file |
| `ADSDNSGO_OUTPUT_LEVEL` | Default output level |
| `ADSDNSGO_RESOLVER` | Default DNS resolver |
| `ADSDNSGO_DNSSCIENCE_KEY` | dnsscience.io API key |

## Troubleshooting

### Command not found

Ensure `$GOPATH/bin` is in your PATH:

```bash
export PATH=$PATH:$(go env GOPATH)/bin
```

Add to `~/.bashrc` or `~/.zshrc` for persistence.

### Permission denied

If building from source:
```bash
chmod +x dnsgo
```

### DNS resolution issues

Test basic connectivity:
```bash
dnsgo query google.com --server 8.8.8.8
```

## Uninstall

```bash
rm /usr/local/bin/dnsgo
rm -rf ~/.config/adsdnsgo
```
