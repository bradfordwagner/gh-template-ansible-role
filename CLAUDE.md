# Vault Ansible Role

Installs HashiCorp Vault (Community or Enterprise) as a versioned binary with a symlink. See `README.md` for variables and usage.

## Upgrades

Two independent things can go stale and should be checked when revisiting this role:

1. **Vault version default** (`defaults/main.yml`'s `vault_version`): this role defaults to the latest final (non-`alpha`/`beta`/`rc`), non-HSM/FIPS Vault **Community** release. Check `https://releases.hashicorp.com/vault/index.json` for a newer final Community version (a version with no `+` suffix and no `alpha`/`beta`/`rc` in the string) and bump `vault_version` if one exists. Keep `test.yml`'s pinned `verify_version`/`vault_version` values in sync with this default.
2. **`andrewrothstein.unarchivedeps` role dependency** (`meta/requirements.yml`): check its current latest release/tag and bump the pinned `version` if newer.
3. **CI base image** (`config.yaml`'s `upstream.tag`, `ghcr.io/bradfordwagner/ansible`): check `ghcr.io/v2/bradfordwagner/ansible/tags/list` (anonymous token from `ghcr.io/token?service=ghcr.io&scope=repository:bradfordwagner/ansible:pull`) for a newer semver tag. The `builds:` OS list must be updated to match whatever OS variants that new tag actually publishes — older OS families get dropped between releases (e.g. `debian_bookworm`, `rockylinux_9`, `ubuntu_jammy`, and `alpine_3.18/19/20` all disappeared by `6.4.0`), so bumping the tag alone without updating `builds:` breaks CI.

Ansible role dependencies and the CI base image are part of "upgrades" for this repo, not just the software the role installs.

## Galaxy role name

`meta/main.yml` sets `role_name: gh_template_ansible_role` on purpose. Tag pushes import this repo into Galaxy, and Galaxy keys roles by `<namespace>.<role_name>`. With `role_name: vault`, a template tag re-pointed the real `bradfordwagner.vault` at this repo and replaced its versions with the template's tags, which broke `ansible-galaxy install bradfordwagner.vault,1.1.6` for consumers. Never set this to a real role's name. A role derived from this template must set its own `role_name`.
