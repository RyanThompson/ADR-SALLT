# 0002 Package Fidelity

**Status:** Accepted · **Layer:** Streams · **Principles:** P8 · **Stack:** 0001

> Streams is fixed in Streams. The app never shadows a package. (Platform law: 0001)

## Rules
1. No new files under `resources/views/vendor/**`.
2. No `vendor:publish` of package views to patch markup or CSS.
3. A package bug is fixed in the package checkout, committed upstream, then the app bumps its reference.
4. Customize through builders, options, theme and app components that call the package.
5. Grandfathered overrides are deleted when touched, with the change landed upstream.

## Shape
```text
1. Edit vendor/streams/ui/resources/views/...
2. Compare against the sibling component.
3. Commit and push to laravel-streams/streams-ui.
4. Bump the app's composer reference.
```

## Not
- Copying a package Blade "just for this screen".
- Keeping an app copy after fixing upstream.


Checklist: [`checklists/0002-package-fidelity.yml`](../checklists/0002-package-fidelity.yml)
