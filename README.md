# Mintlify Starter Kit

Click on `Use this template` to copy the Mintlify starter kit. The starter kit contains examples including

- Guide pages
- Navigation
- Customizations
- API Reference pages
- Use of popular components

### Development

Use Node.js 20.17 or newer (LTS recommended). This repository pins the expected
version in `.nvmrc`:

```bash
nvm use
```

Install the current [Mintlify CLI](https://www.mintlify.com/docs/cli/install). The
supported package and command are both named `mint`:

```bash
npm install -g mint@latest
mint version
```

Run the local preview from the repository root, where `docs.json` is located:

```bash
mint dev
```

To update an existing installation and validate the documentation:

```bash
mint update
mint validate
```

### Publishing Changes

Install our Github App to auto propagate changes from your repo to your deployment. Changes will be deployed to production automatically after pushing to the default branch. Find the link to install on your dashboard. 

#### Troubleshooting

- The local preview is out of sync or does not start: run `mint update` and try again.
- The page loads as a 404: make sure you run `mint dev` from the folder containing `docs.json`.
- The `mint` command is missing: run `npm install -g mint@latest` under the Node.js version selected by `nvm use`.
- If both `mint` and the legacy `mintlify` commands are installed, remove the old package with `npm uninstall -g mintlify`.
