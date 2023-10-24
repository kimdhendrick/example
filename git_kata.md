# git kata

**Ideals**

- short lived branches
- integrate often!
- linear history

Setup:
```bash
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative"
```

Create a repo to play with or clone my example repo:

```bash
git clone git@github.com:kimdhendrick/example.git
cd example
git lg

* aaf416b - (HEAD -> main, origin/main) Initial commit (49 minutes ago) <Kim Hendrick>
```

**Rebasing 101**

Think of rebasing as "changing your starting point”

1. pull main
    
    ```bash
    git pull -r
    ```
    
2. create branch
    
    ```bash
    git checkout -b short-lived-branch
    ```
    
3. make changes
    
    ```bash
    echo "I am fine, thank you" >> README.md
    ```
    
4. commit wip #1
    
    ```bash
    git add README.md
    git commit -m "wip #1"
    ```
    
5. make more commits
    
    ```bash
    echo "how are you" >> README.md && git commit -am "wip #2"
    echo "goodbye" >> README.md && git commit -am "wip #3"
    ```
    
6. check out log
    
    ```bash
    ~/workspace/example $: git lg
    * 36ce133 - (HEAD -> short-lived-branch) wip #3 (2 seconds ago) <Kim Hendrick>
    * f9de025 - wip #2 (53 seconds ago) <Kim Hendrick>
    * 0e110d0 - wip #1 (3 minutes ago) <Kim Hendrick>
    * c062cf2 - (origin/main, main) Update README.md (15 minutes ago) <Kim Hendrick>
    * aaf416b - (tag: start) Initial commit (22 minutes ago) <Kim Hendrick>
    ```
    
7. Isn’t that ugly?! Let’s squash commits!
    
    ```bash
    git rebase -i HEAD~3
    ```
    
    I like to use a relative reference to `HEAD` as above (I wanna squash the most recent 3) but these are all equivalent:
    
    ```bash
    git rebase -i c062cf2
    git rebase -i main
    git rebase -i origin/main
    ```
    
    - pick
    - reword
    - fixup
    
    ```bash
    ~/workspace/example $: git lg
    * aa2588a - (HEAD -> short-lived-branch) Update README again (1 second ago) <Kim Hendrick>
    * c062cf2 - (origin/main, main) Update README.md (22 minutes ago) <Kim Hendrick>
    * aaf416b - (tag: start) Initial commit (28 minutes ago) <Kim Hendrick>
    ```
    
8. get latest from main - incorporate others’ changes (put your changes **********after********** theirs)
    
    ```bash
    git checkout main
    git pull -r
    ```
    
9. rebase main into your branch - put others’ changes into your branch ****************************************test the integration****************************************
    
    ```bash
    git checkout short-lived-branch
    git rebase main
    ```
    
10. rebase your branch into main
    
    ```bash
    git checkout -
    git rebase short-lived-branch
    ```
    
11. push main
    
    ```bash
    git push --set-upstream origin main
    ```
    
12. delete branch locally
    
    ```bash
    git branch -d short-lived-branch
    ```
    

**Rebasing 201**

- Reorder commits
- Merge conflicts
- Sharing branches *********************caution*********************
    - `—force-with-lease`
    - delete branches remotely `git push origin --delete short-lived-branch`

**Independent study**

- Nice reference: https://content.initialcapacity.io/git-commands-for-a-trunk-based-workflow-972f8ed3a1c4
- What do the newish commands `git switch` and `git restore` do?
