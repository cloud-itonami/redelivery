# Operator quickstart

**Nothing in this repository says what redelivery does, and there is nothing here
to deploy.** 13 tracked files: a Vite/Svelte scaffold whose page is a placeholder,
an actor manifest pointing at a component that does not exist, and no
`wrangler.jsonc` at all.

That is not a complaint about this repository in particular — measured below, it is
the shape of a large part of the cohort. This document is repository orientation
and says which of the two questions ("what is it" / "how do I run it") the tree can
answer. Neither, as it stands.

Steps marked ✅ were run against this tree on 2026-08-15.

---

## 1. The whole repository ✅

```bash
git ls-files | wc -l                       # 13
git ls-files | grep -c wrangler            # 0   -- no deploy configuration
git ls-files | grep -c '\.wasm$'           # 0
wc -c appview/*/svelte/src/App.svelte      # 450
```

`App.svelte` in full, minus its `<style>` block:

```svelte
<main>
  <h1>etzhayyim-wasm-redelivery-rd3l1vry</h1>
  <p>Vite entry scaffold after SvelteKit cleanup.</p>
</main>
```

The page renders its own directory name and says it is a scaffold. There is no
router, no data, no fetch, and — with no `wrangler.jsonc` — nothing that says where
a build of it would go.

## 2. What the actor manifest claims ✅

```bash
python3 -c "
import json,io,glob
d=json.load(io.open(glob.glob('appview/*/kotodama.jsonld')[0]))
for k in ('component','profile','duties','kpi','governance'):
    print(k,'=',json.dumps(d.get(k),ensure_ascii=False)[:150])
"
ls appview/*/component.wasm     # zsh: no matches found (bash: No such file or directory)
```

| field | value | |
|---|---|---|
| `component.path` | `component.wasm` | **file absent** |
| `profile.description` | `Redelivery — AI Agent` | the name plus a category |
| `duties` | `["勤労の義務"]` | scaffold default |
| `kpi` | `["credits の獲得"]` | scaffold default |
| `governance.complianceFrameworks` | `["redelivery-policy"]` | no policy file exists here |

**The description is circular.** "Redelivery — AI Agent" restates the display name
and adds a category, so the manifest — the one file whose job is to say what this
actor is — does not. Neither does `README.edn`, which carries schema, repository,
kind and source path and no prose. So the answer to "what does redelivery do" is
not in this repository, and this quickstart cannot supply it either: inventing a
purpose for an actor would be worse than recording that none is written down.

## 3. ⚠ Both of those are fleet-wide, measured

Because a local observation invites a local fix, and these are not local. Measured
across `cloud-itonami` and `etzhayyim`:

| | count |
|---|---|
| `kotodama.jsonld` actor manifests read | **1,422** |
| `profile.description` is exactly `<displayName> — AI Agent` | **480** (34%) |
| declare a `component` path | 676 |
| **… whose component file is absent** | **598** (88% of those that declare one) |
| repositories with a `svelte/src/App.svelte` | 67 |
| … carrying the placeholder sentence above | **39** (58%) |
| … placeholder **and** no `wrangler.jsonc` | **28** |

So: a third of the actor manifests do not say what their actor does; nearly nine in
ten declared components are not in their repository; and 28 repositories, this one
included, hold a placeholder page with no deployment target.

Reproduce the last three rows without leaving this tree:

```bash
git ls-files | grep -c wrangler                                    # 0
grep -c 'Vite entry scaffold after SvelteKit cleanup' \
  appview/*/svelte/src/App.svelte                                  # 1
ls appview/*/component.wasm 2>&1 | tail -1                         # no matches found
```

## 4. Build ⚠ NOT WALKED, and it would not help

`svelte/package.json` declares Vite with Tailwind and PostCSS. There is no lockfile
and no `node_modules`, so an install needs the network; it was not run and is not
claimed to work. Through the resource governor if you do:

```bash
node <root>/scripts/resource-guard.mjs run build -- \
  npm --prefix appview/etzhayyim-wasm-redelivery-rd3l1vry/svelte run build
```

A successful build produces the placeholder page in §1, and there is no
`wrangler.jsonc` to deploy it with.

---

## 5. Provenance ✅

`migration.edn` uses `etzhayyim.migration/extracted-v1` — like `ransomwatch` and
unlike the `/v1` seeds — with `:status :extracted` and **no
`:identity/:allowed-additions`**, so there is no declared allow-list for this
document to be added to and none was invented.

Source: `etzhayyim/root` at `60-apps/etzhayyim-project-redelivery`, revision
`1fb8430a`, tree `dd3bf2b3`, 11 tracked files, 5,409 bytes. `README.edn` records
`:kind :standalone-app-artifact`, which is at least consistent with what is here: an
artifact, standing alone, not yet wired to anything.

## 6. So what would make this repository answerable

Two separate things, and neither is a documentation task:

1. **What it is.** Someone who knows must write it — in `profile.description`, or a
   README naming itself. 480 manifests share this gap, so a fix here is one of 480.
2. **What runs.** Either the component named in the manifest, or a
   `wrangler.jsonc` plus something worth serving. Today the page is a placeholder
   and there is no deploy target, so "deploy redelivery" has no meaning yet.
