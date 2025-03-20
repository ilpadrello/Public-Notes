There is a feature in typescript that let you have faster and more reliable module import using personalized path.

In the tsconfig.json file you can have something like this : 

```
"baseUrl": "./ts", /* Specify the base directory to resolve non-relative module names. */
"paths": {
	  "@utils/*": ["./utils/*"],
      "@models/*": ["./models/*"],
      "@services/*": ["./services/*"],
      "@controllers/*": ["./controllers/*"],
      "@middlewares/*": ["./middlewares/*"],
      "@routes/*": ["./routes/*"],
      "@libs/*": ["./libs/*"]
    },
```

using this approach you can import your module just with : 
```
import * as test from '@utils/test'
```

This workaround let you remove all the `../../../../../../` in your code.

Unfortunately, for a reason that I REALLY CAN'T THINK OF IT when transpiling the code, the consequent JavaScript file will have the import as `@utils` and will not understand from where you need to import the stuff.

WHY???? NO REALLY WHY?????

## SOLUTION :

The solution that I have found is to use `tsc-alias`.
This package, replace (after the build) the aliases (I'm still in disbelief this is a thing to do).

So we need to update the package.json file to add our tsc-alias : 

```
"scripts": {
    "build": "tsc && tsc-alias",
    "rebuild": "rm -rf dist && npm run build",
    "rebuild-db": "NODE_ENV=development node dist/utils/db/rebuild-db.js",
    "start:watch": "tsc-watch --onSuccess \"npm run start:dev\"",
    "start:dev": "tsc-alias && NODE_ENV=development nodemon dist/server.js",
    "start:prod": "NODE_ENV=production node dist/server.js",
    "new:pg:migration": "bash ./ts/utils/db/wscrm/create_migration.sh",
    "stop": "pkill -SIGINT scidaaa",
    "test": "jest"
  },
```
