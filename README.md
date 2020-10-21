# Utopian React
Utopian React is React, except for one change: we modify the production build of React to include un-minified error messages. Because our editor and projects are in the same document, and because React can only have one version per document, projects have to use the same build of React as we use in the editor. We use the production build because this means faster performance for users, but we also want users to be able to read un-minified error messages. So we have changed the build process to keep unminified error messages.

The results of the build are then pushed to `utopia-react`, and `utopia-react-dom`, and aliased to `react` and `react-dom` in the editor.

## How to update and build Utopian React
### Update React
1. Find the tag or commit you want to build from [the main React repo](https://github.com/facebook/react)
2. Make a pr against master in this repo.
3. Make sure the changes haven't messed up the lines changed in the build script at /scripts/rollup/build.js. It should roughly keep the `noMinify: true` settings the same as they were from this change: https://github.com/concrete-utopia/react/commit/c96e246bb3620ed3b86d92ef725c7f1a3cffcbb2.
4. Merge that pull request

### Build Utopian React
1. Check out concrete-utopia/react on your local computer.
2. Install with `$ yarn` and then build React using `$ yarn build react,react-dom`
3. Go make a cup of tea. Unless you are Sean, building will take quite a few minutes.

### Update Utopian React on npm
1. Open up the `react` and `react-dom` directories from `utopia/editor/node_modules`.
2. For each directory delete the contents with the exception of package.json.
3. Update the `version` field in both `react` and `react-dom` to the target version number.
4. In `react-dom`'s package.json only: update the peer dependency to point to `"^XX.X.X"`, where XX.X.X is the current target version. Note the leading circumflex.
5. Take the built `react` and `react-dom` directories from `react/build/node_modules/` and respectively overwrite the contents (save the `package.json`s) of the `utopia/editor/node_modules/react` and `utopia/editor/node_modules/react-dom` directories.
6. Run `npm publish` in each directory. This publishes them to [utopia-react](https://www.npmjs.com/package/utopia-react) and [utopia-react-dom](https://www.npmjs.com/package/utopia-react-dom) (contact @alecmolloy for write access to these packages.)

### Testing
You can confirm error messages have properly been compiled without minification simply by breaking React with e.g. a call to `useEffect()` outside a function component in the code editor, while running react in performance mode.
