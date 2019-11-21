---
title: Hello World
---
## ThingsEVA



测试



```bash
#!/bin/sh

set -e

cd $GITHUB_WORKSPACE

echo "--- Installing node_modules"
npm install

HEXO=${GITHUB_WORKSPACE}/node_modules/hexo/bin/hexo
REPO_URL=https://git:${PERSONAL_TOKEN}@github.com/${DEPLOY_REPO}

if [ ! -e "${DEPLOY_DIR}" ]; then
    mkdir "${DEPLOY_DIR}"
fi

cd ${DEPLOY_DIR}

echo "--- Init git"
git clone ${REPO_URL} .
git config user.name "${GITHUB_ACTOR}"
git config user.email "${GITHUB_ACTOR}@users.noreply.github.com"

# TODO: DEPLOY_BRANCH must exists
echo "--- Checkout"
git checkout ${DEPLOY_BRANCH}

echo "--- Generating"
$HEXO generate

echo "--- Committing git"
git add --all
git commit --allow-empty -m "Publish: `date '+%Y-%m-%d %H:%M:%S'`"
git push origin ${DEPLOY_BRANCH} --force

echo "--- Completed"

```

