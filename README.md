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

* Now change the tag name on the release:
```
        curl -k -s -u '<user>:<password>' -X PATCH --data '{"tag_name":"<new tag name>","draft":false,"prerelease":false}' https://api.github.com/repos/<owner>/<repo>/releases/<id>
```

* This will leave the old tag still there, so get rid of it:
```
        git tag --delete "<old tag name>" && git push --delete origin "<old tag name>"
```

* The whole thing, with most of the variables filled in, how I did it:
```
        $ curl -k -s https://api.github.com/repos/jfrickson/git-testing/releases | grep -B1 '^    "tag_name": "release-0.1.0" | head -1 | cut -d':' -f2'
         6196638,
        $ curl -k -s -u 'jfrickson:******' -X PATCH --data '{"tag_name":"git-test9ng-0.1.0","draft":false,"prerelease":false}' https://api.github.com/repos/jfrickson/git-testing/releases/6196638
        git tag --delete "release-0.1.0" && git push --delete origin "release-0.1.0"
```
