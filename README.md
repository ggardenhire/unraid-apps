# unraid-apps

Unraid **Community Applications (CA)** template repository for
[ggardenhire](https://github.com/ggardenhire)'s self-hosted applications.

This repo holds the Docker template XML and supporting Unraid files. The
application source code lives in each app's own repository — this is only the
place Unraid pulls install templates from.

## Applications

| App | Description | Source | Docker image |
|-----|-------------|--------|--------------|
| **curtain** | Public-facing, read-only dashboard for an Unraid server (live metrics, storage breakdown, quick links). | [ggardenhire/curtain](https://github.com/ggardenhire/curtain) | [`ggarden/curtain`](https://hub.docker.com/r/ggarden/curtain) |
| **podium** | Self-updating static-site runner that clones, builds and serves GitHub repos and redeploys on push. | [ggardenhire/podium](https://github.com/ggardenhire/podium) | [`ggarden/podium`](https://hub.docker.com/r/ggarden/podium) |

## Layout

```
ca_profile.xml          # repository profile shown in Community Applications
templates/              # one Docker template per app (what CA installs)
  curtain.xml
  podium.xml
icons/                  # app icons referenced by the templates
scripts/                # XML validation used by CI
.github/workflows/      # validate templates on every push / PR
```

Each template's `<TemplateURL>` and `<Icon>` point at the raw files **in this
repo**, so Community Applications refreshes installed containers from here.

## Installing an app

Once this repository is indexed by Community Applications, search for **curtain**
or **podium** in the **Apps** tab on your Unraid server and install.

To test a template before it is indexed, paste its raw URL into
**Apps → (gear) → "Install a template by URL"**, e.g.

```
https://raw.githubusercontent.com/ggardenhire/unraid-apps/main/templates/curtain.xml
```

## Adding this repository to Community Applications

CA discovers repositories centrally rather than letting each user add a URL. To
get this repo indexed, submit it through the current Community Applications
intake form (see the [CA application policies thread](https://forums.unraid.net/topic/87144-ca-application-policies-notes/)).
Requirements include 2FA on the GitHub/Docker Hub accounts and a valid
`ca_profile.xml` in the repo root (present here). After submission, it appears
under **Apps → Statistics → Repositories**, and any template problems show under
**Statistics → Template Errors**.

## Adding or updating a template

1. Add/edit the app's XML under [`templates/`](templates/) and drop its icon in
   [`icons/`](icons/).
2. Keep `<TemplateURL>` and `<Icon>` pointing at this repo's `main` raw URLs.
3. Push — the [validation workflow](.github/workflows/validate.yml) checks every
   template for well-formedness, the fields CA expects, and that the `Icon` /
   `TemplateURL` URLs resolve.

Validate locally:

```bash
python3 scripts/validate-template.py --check-urls ca_profile.xml templates/*.xml
```
