# OBS Studio Flaking Loaded

OBS Studio for NixOS ❄️ that bundles an extensive collection of 3rd party plugins

## Submitting OBS Studio Plugin to nixpkgs

- Fork [nixpkgs](https://github.com/flexiondotorg/nixpkgs)
- If this is your first OBS Studio plugin, add yourself as a maintainer.

Edit `nixpkgs/maintainers/maintainer-list.nix` and add yourself (alphabetically) as a maintainer:

```nix
    handle = {
      # Required
      name = "Your name";

      # Optional, but at least one of email, matrix or githubId must be given
      email = "address@example.org";
      matrix = "@user:example.org";
      github = "GithubUsername";
      githubId = your-github-id;

      keys = [{
        fingerprint = "AAAA BBBB CCCC DDDD EEEE  FFFF 0000 1111 2222 3333";
      }];
    };
```

- `handle` is the handle you are going to use in nixpkgs expressions,
- `name` is your, preferably real, name,
- `email` is your maintainer email address,
- `matrix` is your Matrix user ID,
- `github` is your GitHub handle (as it appears in the URL of your profile page, `https://github.com/<userhandle>`),
- `githubId` is your GitHub user ID, which can be found at `https://api.github.com/users/<userhandle>`,
- `keys` is a list of your PGP/GPG key fingerprints.

The commit message for adding yourself as a maintainer should be: `maintainers: add GithubUsername`

## Add an OBS Studio plugin

- Add `nixpkgs/pkgs/applications/video/obs-studio/plugins/name-of-plugin.nix`
  - Use existing plugins as a reference.

- Add the plugin to `nixpkgs/pkgs/applications/video/obs-studio/plugins/default.nix`
  - Something like:

```nix
name-of-plugin = callPackage ./name-of-plugin.nix { };
```

- Commit changes locally
  - Message for new OBS Studio plugins should be: `obs-studio-plugins.name-of-plugin: init at x.y.z` where x.y.z is the version number or commit reference.
- Test build:

```
cd nixpkgs/pkgs/applications/video/obs-studio/plugins
nix build .#obs-studio-plugins.name-of-plugin --tarball-ttl 0
```
- Update `sha256` in `nixpkgs/pkgs/applications/video/obs-studio/plugins/name-of-plugin.nix`
- Commit and squash.
- Build again: `nix build .#obs-studio-plugins.name-of-plugin --tarball-ttl 0`
- Does it look like an OBS Studio plugin: `tree result/`
- Run `nixpkgs-review`: `nix run nixpkgs#nixpkgs-review rev HEAD`
- You'll be dropped into a shell, copy the contents of `report.md` for pasting in your pull request to nixpkgs
  - **Do not copy/paste the snippet below**, it is just an example.

```html
Result of `nixpkgs-review` run on x86_64-linux [1](https://github.com/Mic92/nixpkgs-review)
<details>
  <summary>1 package built:</summary>
  <ul>
    <li>obs-studio-plugins.name-of-plugin</li>
  </ul>
</details>
```

- Submit a pull request to nixpkgs

#### Reference pull requests

- https://github.com/NixOS/nixpkgs/pull/219872
- https://github.com/NixOS/nixpkgs/pull/212262

