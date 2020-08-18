# Nodewrkspace

## Node support

https://nx.dev/angular/plugins/node/overview

## Configuring package.json copying

**1. Create a `package.json` in the node app's folder**

This `package.json` will only serve for publishing to npm. Add whatever you need. The main entry point produced by the Node app will be `main.js`.

**2. Configure copying of the package.json**

By default only the `assets` folder gets copied. To copy also the `package.json`, just add another entry in the `workspace.json` where your node app's configuration lives.

```json
{
  "version": 1,
  "projects": {
    "nodeapp": {
      ...
      "architect": {
        "build": {
          "builder": "@nrwl/node:build",
          "options": {
            ...
            "assets": [
              "apps/nodeapp/src/assets",
              {
                "input": ".",
                "glob": "package.json",
                "output": "."
              }
            ]
          }
        ...
        },
      ...
      },
    ...
    }
  }
}
```

**3. Build the app**

Run

```
$ yarn nx build nodeapp --configuration=production
```

Where `nodeapp` is the name you've chosen for your node application

You should now see the result be produced in the `dist/apps/nodeapp` folder.

## Automating publishing

To further automate, [run-commands](https://nx.dev/angular/plugins/workspace/builders/run-commands#run-commands) are a great way to execute just any arbitrary command. Even custom scripts which usually live in the `tools` folder of the workspace (but you can really place them anywhere you want).

Add the following run-command to the generated `nodeapp`'s configuration in `workspace.json`

```
{
  "version": 1,
  "projects": {
    "nodeapp": {
      "root": "apps/nodeapp",
      "sourceRoot": "apps/nodeapp/src",
      "projectType": "application",
      "prefix": "nodeapp",
      "schematics": {},
      "architect": {
        "build": {...},
        "serve": {...},
        "lint": {...},
        "test": {...},
        "pack": {
          "builder": "@nrwl/workspace:run-commands",
          "options": {
            "parallel": false,
            "commands": [
              {
                "command": "nx run nodeapp:build --configuration=production"
              },
              {
                "command": "echo '>>> publishing to npm here <<<'"
              }
            ]
          }
        }
      }
    }
  },
  ...
}
```

This allows to just run

```
$ yarn nx run nodeapp:pack
```

Which will invoke the build command as well as the actual publishing. Obviously substitute the `echo ...` with whatever you need for publishing.
