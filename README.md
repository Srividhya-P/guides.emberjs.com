# READ-ONLY READ-ONLY READ-ONLY

This repo is a READ-ONLY repo. It contains the built output of the content
from https://github.com/emberjs/guides. If you would like to make corrections
or additions to a guide, please open a pull request at
https://github.com/emberjs/guides.

## Publishing

These instructions are for publishing a new version of the site at http://guides.emberjs.com. This section is intended for members of the Ember.js release team.

### Build the guides

First, in the [main guides repo](https://github.com/emberjs/guides), make sure your latest content is pushed and tagged with a version number:

```shell
git tag <revision number>
git push --tags
```

For `<revision number>` we use the following format `v<major version>.<minor version>.<patch>`, so
`v1.10.0` is correct but `1.9.1` is not.

Next, build a new snapshot and move it to the guides _site_ repo (this repo). This should be run from the main guides repo (not this one):

```shell
middleman build
mv ./build <path to guides site repo>/snapshots/<revision number>
```

### Add the version to the site

Now, change directories into the guides site repo (this repo). Add the version number to `snapshots/versions.json`, and set the default version in `divshot.json`. Then update the list of versions:

```shell
node tasks/update-versions.js
```

Finally, commit and push this repo:

```shell
git add --all
git commit -m "Add snapshot for Ember.js revision <revision number>"
git push
```

### Publish

The site is hosted on Divshot. If you don't have divshot-cli installed, do so with `npm install divshot-cli -g`.

Publish this repo to Divshot's staging environment (our site host):

```shell
divshot push
```

Verify that the content looks good at http://development.ember-guides.divshot.io/<revision number>. 

If there are no obvious defects, you're ready to publish the site content and search content:

```shell
npm publish # runs `divshot promote staging production` && `npm swiftype`
```

### Update search

Publish the searchable content of the revison you are publshing:

```shell
node tasks/publish-search --revision <revision number>
                  --environment <staging|production>
                  --api-key <API_KEY>
                  --engine <engine name>
# e.g.
node tasks/publish-search --revision v1.1.1 --environment staging --api-key SUPERSECRETBROCOMMON --engine ember
```
