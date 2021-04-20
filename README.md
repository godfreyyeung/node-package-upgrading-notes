# Upgrading Node Packages 

## To fix security issues
### With Yarn

1. Review Dependabot security alerts
2. Create a list of "resolutions" for each "non-conflict" upgrade recommended by Dependabot. "non-conflict" meaning Dependabot does not say a parent dependency prevents the required upgrade. Add these resolutions to `package.json`.

e.g. 
```
  "dependencies": {
    ...
  },
  "resolutions": {
    "y18n": "^3.2.2",
    "handlebars": "^4.5.3",
    "dot-prop": "^4.2.1",
  }
```

3. Try `yarn` to see if the current node version allows for these resolutions.
4. If `yarn` completes without errors, try running the app.
5. If app runs, commit the edited `package.json` and the generated `yarn.lock`.
6. If `yarn` fails, check the error. It may be due to some upgrades requiring a higher node version.
7. If a higher node version is required, identify the version and add it to the package.json "engines" property.
8. Remove the resolutions, reset `yarn.lock` and try running `yarn`, then run the app again.
9. If an error occurs, identify the error. The error may report a certain dependency requires an older version of node.
10. If that dependency is listed in the package.json (either under "dependencies" or "devDependencies"), go ahead and manually upgrade it to the latest version (revert to the older node version first). Check that it doesn't break the app, and then try upgrading node again.
11. If the error reports a dependency that isn't directly listed in package.json, we'll need to figure out the parent dependency. run `yarn why <depedency name>` and inspect the output to figure out which package is likely the parent dependency. The output may say something like "because <parent dependency> depends on it" or "hoisted from <parent-dependency>. There may be some trial and error here.
12. Manually upgrade the parent dependcy (Revert to old node version). Then try upgrading node again.
13. Once you can run a higher version of node without errors, try setting the resolutions again. 
14. Once you successfully set resolutions, can `yarn` and run the app without errors, go though the dependabot security alerts again. This time, some of the previously-conflicted dependencies may no longer have conflicts.
15. Add the recommended upgrades (that no longer have conflicts) to resolutions. 
16. Repeat 2- 15 until security alerts are resolved.
