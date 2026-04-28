# wifialli-packages

Public OpenWrt package feed for [wifialli-cpp](https://github.com/YOUR_ORG/wifialli-cpp).

> **This repo is auto-populated by CI.** The built `.ipk` files and package
> index are pushed here automatically whenever `wifialli-cpp/main` is updated.
> Do not edit the `gh-pages` branch manually.

## Using this feed on your OpenWrt device (ramips/mt7621)

```sh
echo "src/gz wifialli https://YOUR_ORG.github.io/wifialli-packages/mipsel_24kc" \
  >> /etc/opkg/customfeeds.conf

opkg update
opkg install wifialli

# First-time configuration
cp /etc/wifialli/config.yaml.example /etc/wifialli/config.yaml
vi /etc/wifialli/config.yaml

# Enable and start the service
/etc/init.d/wifialli enable
/etc/init.d/wifialli start

# View logs
logread -f | grep wifialli
```

## Repo setup (one-time)

### 1. Create this repo on GitHub as **public**

```sh
gh repo create YOUR_ORG/wifialli-packages --public --description "WifiAlli OpenWrt package feed"
cd wifialli-packages
git init
git add README.md
git commit -m "Initial commit"
git remote add origin git@github.com:YOUR_ORG/wifialli-packages.git
git push -u origin main
```

### 2. Enable GitHub Pages

Go to **Settings → Pages** and set:
- Source: **Deploy from a branch**
- Branch: **gh-pages** / **/ (root)**

### 3. Create a deploy key

```sh
ssh-keygen -t ed25519 -f packages_deploy_key -N "" -C "wifialli-packages-deploy"
```

- Add the **public key** (`packages_deploy_key.pub`) to this repo:
  **Settings → Deploy keys → Add deploy key** (tick "Allow write access")

- Add the **private key** (`packages_deploy_key`) to the **wifialli-cpp** repo:
  **Settings → Secrets and variables → Actions → New repository secret**
  Name: `PACKAGES_DEPLOY_KEY`

```sh
# Clean up local key files afterwards
rm packages_deploy_key packages_deploy_key.pub
```

The next push to `wifialli-cpp/main` will build the packages and populate the `gh-pages` branch here.
