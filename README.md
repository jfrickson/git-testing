# git-testing

## VERSION 0.1.0
## MODIFICATION_DATE 2017-04-26

### Last Modified: April 26, 2017

Junk repo for testing out various things

#### Fixing a release
* I (intentionally) created a release and gave it a wrong tag name `release-0.1.0`
  so I need to change the tag name on the release to `git-testing-0.1.0`

* First, get the id of the release you want to change. Do one of the following:
```
        curl -k -s https://api.github.com/repos/<owner>/<repo>/releases | egrep '^  [\{\}]|^    "id":|^    "tag_name":'
        curl -k -s https://api.github.com/repos/<owner>/<repo>/releases | grep -B1 '^    "tag_name": "<the tag name>"'
```

* Get the last commit for that release/tag
```
        git rev-list -1 "<old tag name>
```

* Get the date of that last commit
```
        curl -k -s https://api.github.com/repos/<owner>/<repo>/commits/<commit hash> | \
            grep '"date"' | head -1 | cut -d'"' -f4`
```

* Create new tag (overwriting old) and set the date
```
        git checkout <commit hash>
        GIT_COMMITTER_DATE="2016-09-06T14:59:22Z" git tag -f -a -m \
             "<new tag name>" "<new tag name>"
        git push -f --tags
```

* Now change the tag name on the release:
```
        curl -k -s -u '<user>:<password>' -X PATCH \
            --data '{"tag_name":"<new tag name>","draft":false,"prerelease":false}' \
            https://api.github.com/repos/<owner>/<repo>/releases/<id>
```

* This shouldn't leave the old tag still there, but if it is, get rid of it:
```
        git tag --delete "<old tag name>" && git push --delete origin "<old tag name>"
```
