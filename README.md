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
$ yarn nx build nodeapp
```

Where `nodeapp` is the name you've chosen for your node application

You should now see the result be produced in the `dist/apps/nodeapp` folder.
