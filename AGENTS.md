# Harmony - Defold Game Project

A mobile grid-based strategy/puzzle game built with the [Defold](https://defold.com) game engine using Lua.

## Cursor Cloud specific instructions

### Project overview

This is a Defold engine project (Lua scripting). There are no package managers, lockfiles, Docker containers, or backend services. The only build tool is `bob.jar` (Defold's CLI builder).

### Build tool

- **bob.jar** is located at `/opt/defold/bob.jar` (Defold v1.11.2, requires JDK 21+)
- Run with: `java -jar /opt/defold/bob.jar [options] [commands]`

### Common commands

| Task | Command |
|------|---------|
| Resolve deps | `java -jar /opt/defold/bob.jar resolve` |
| Build (HTML5) | `java -jar /opt/defold/bob.jar --platform js-web --archive build` |
| Bundle (HTML5) | `java -jar /opt/defold/bob.jar --platform js-web --archive --bundle-output build/bundle/html5 bundle` |
| Serve locally | `cd build/bundle/html5/Harmony && python3 -m http.server 8080` |
| Lint Lua | `luacheck systems/ main/main.script grid/grid.script` |
| Test modules | `lua -e "local h = require('systems.harmony'); ..."` (run from /workspace) |

### Linting notes

- `luacheck` reports expected warnings on `.script` files (Defold lifecycle globals like `init`, `update`, `on_input` and engine APIs like `msg`, `hash`). These are false positives—Defold injects these at runtime.
- Lua module files under `systems/` should lint clean.

### Known issue

`systems/economy.lua` requires `"systems.harmony_rules"` but the actual module file is `systems/harmony.lua`. This causes a runtime error if `economy.lua` is loaded. This is a pre-existing bug in the repo.

### Testing Lua modules

Lua modules (`systems/*.lua`) can be tested standalone with standard Lua interpreter since they don't depend on Defold APIs:

```sh
cd /workspace && lua -e "local h = require('systems.harmony'); print(h.calculate_bonus({x=0,y=0}, {{x=0,y=0}}))"
```

`busted` test framework is also available for structured tests.
