# How To PR (Pull Request)

__"All change is hard at first, messy in the middle and so gorgeous at the end."__


## Before You start...

- *Git Flow* vs *Github Flow*
  - http://nvie.com/img/git-model@2x.png
  - http://cdn-ak.f.st-hatena.com/images/fotolife/s/shoma2da/20151104/20151104223339.png


## 1. `fork` target repository

- Github -> Fork -> `<YOUR_ID>`
- After the fork, `git clone` forked repository.
  - You may need ssh key registration.
```
$ git clone git@github.com:<YOUR_ID>/<YOUR_REPO>.git
```



## 2. Set your local git repository

- Go to the your cloned local git repository.
    ```
    $cd <YOUR_REPO>
    ```

- Check branch information.
    ```
    $ git branch -av
    * master                40b8510 Merge pull request #31 from hwamin/fix-case-sensitive
      remotes/origin/HEAD   -> origin/master
      remotes/origin/master 40b8510 Merge pull request #31 from hwamin/fix-case-sensitive

    $ git remote -v
    origin  git@github.com:<YOUR_ID>/<YOUR_REPO>.git (fetch)
    origin  git@github.com:<YOUR_ID>/<YOUR_REPO>.git (push)
    ```

- Add original repository. (I named it `upstream` repository.)
    ```
    $ git remote add upstream git@github.com:<GROUP_NAME>/<YOUR_REPO>.git
    $ git fetch --all
    Fetching origin
    Fetching upstream
    From github.com:<GROUP_NAME>/<YOUR_REPO>
     * [new branch]      master               -> upstream/master
    ```

- Check whether it added well.
    ```
    $ git remote -v
    upstream    git@github.com:<GROUP_NAME>/<YOUR_REPO>.git (fetch)
    upstream    git@github.com:<GROUP_NAME>/<YOUR_REPO>.git (push)
    origin  git@github.com:<YOUR_ID>/<YOUR_REPO>.git (fetch)
    origin  git@github.com:<YOUR_ID>/<YOUR_REPO>.git (push)

    $ git branch -av
    * master                            40b8510 Merge pull request #31 from <YOUR_ID>/fix-case-sensitive
      remotes/upstream/master           40b8510 Merge pull request #31 from <YOUR_ID>/fix-case-sensitive
      remotes/origin/HEAD               -> origin/master
      remotes/origin/master             40b8510 Merge pull request #31 from <YOUR_ID>/fix-case-sensitive
    ```

    - You can also check `.git/config`.
        ```
        [core]
                <...>
        [remote "origin"]
                url = git@github.com:YOUR_ID/<YOUR_REPO>.git
                fetch = +refs/heads/*:refs/remotes/origin/*
        [branch "master"]
                remote = origin
                merge = refs/heads/master
        [remote "upstream"]
                url = git@github.com:<GROUP_NAME>/<YOUR_REPO>.git
                fetch = +refs/heads/*:refs/remotes/upstream/*
        ```

## 3. In order to contribute, make a new local branch.

- For each small contribution, create with other branch.
  - This would be helpful when you manage entangled commits.
  - In this example, I named this branch `small-commit`
    ```
    $ git checkout -b 
    Switched to a new branch 'small-commit'

    $ git branch -av
      master                            40b8510 Merge pull request #31 from <YOUR_ID>/fix-case-sensitive
    * small-commit                      40b8510 Merge pull request #31 from <YOUR_ID>/fix-case-sensitive
      remotes/upstream/master           40b8510 Merge pull request #31 from <YOUR_ID>/fix-case-sensitive
      remotes/origin/HEAD               -> origin/master
      remotes/origin/master             40b8510 Merge pull request #31 from <YOUR_ID>/fix-case-sensitive
    ```

- Connect local branch to remote branch.
    ```
    $ git push -u origin small-commit
    Total 0 (delta 0), reused 0 (delta 0)
    To github.com:<YOUR_ID>/<YOUR_REPO>.git
     * [new branch]      small-commit -> small-commit
     Branch small-commit set up to track remote branch small-commit from origin.

    $ git pull
    Already up-to-date.

    $ git branch -av
      master                            40b8510 Merge pull request #31 from <YOUR_ID>/fix-case-sensitive
    * small-commit                      40b8510 Merge pull request #31 from <YOUR_ID>/fix-case-sensitive
      remotes/upstream/master           40b8510 Merge pull request #31 from <YOUR_ID>/fix-case-sensitive
      remotes/origin/HEAD               -> origin/master
      remotes/origin/master             40b8510 Merge pull request #31 from <YOUR_ID>/fix-case-sensitive
      remotes/origin/small-commit       40b8510 Merge pull request #31 from <YOUR_ID>/fix-case-sensitive
    ```
  - Instead of `git push -u origin small-commit`, you can use like belows,
    - `git push origin small-commit; git branch --set-upstream-to=origin/small-commit small-commit`


- Make Some commits.
    ```
    $ git add -- <SOME_FILES>
    $ git commit -m "Fix typo in README.md"
    $ git push
    $ git add -- <SOME_FILES>
    $ git commit -m "Fix typo in API documentation"
    $ git push
    <...>
    ```


- WARNING: Before creating PR, you may need `rebase` command.


- Go to the your forked git commit page.
  - Upload Pull Request.
  - Then just wait review and merge.
    - When it merged, your new PRs are merged to `upstream/master` branch.


- After merged, Apply PR from `upstream` repository to your local repository
    ```
    $ git checkout master
    $ git pull upstream master
    ```

- Apply your local repository to `origin` branch
    ```
    $ git push origin master
    ```

- Delete your temporary branch
  - Remove your origin repository in Github PR Page.
  - Fetch branch informations
    ```
    $ git fetch --all --prune
    ```
  - remove branch on your local repository
    ```
    $ git branch -d small-commits
    ```


## There are still some practical techniques...

- `git rebase`
  - Quick preview (Your current wokring branch name is `small-commit`)
      ```
      $ git checkout master
      $ git pull upstream master
      $ git rebase master small-commit
      $ git push -f origin small-commit
      ```
- How to manage conflicts.
- Get Pull-Request on your local side.
