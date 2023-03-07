# Git submodule workflow example..

## Monday 10:00 AM. (adding the submodule to the base repo)

- You have a project (exisiting repo), you want to add/host another repo inside your project.. 
    - This project might be yours, ex. you have a library and many projects that you want to use the library inside them, so you want to keep just one-updated copy of the library..
    - Or, You've forked a repo, You want to do a lot of changes to the code and you want to keep up to date with the owners edits and bug fixes..

- In your project (that will host the submodules):

```
mkdir submodules # best practice, specify a directory to put all submodules' code inside it.
git submodule add [submodule-repo-url]
git submodule init
```

## Monday 10:30 AM. (Edit the code inside the submodule)

- Till now, You've not pushed any code to remote yet..
- You've edited the files of the hosted submodule:

```
git add . # inside submodule directory
git commit -m "submodule code edits are done"
git push # the submodule remote repo is updated
cd ../.. # back to the root directory
git status # will tell you that the pointer of the submodule is updated
git add .
git commit -m "submodule code pushed 2 minutes ago"
git push # the root project online repo is updated
```


- So, **push the submodule code to the submodule repo first, then push the main project code to its repo**
- Online, You've created a pull request, the code reviewe is done, pull request is accepted and merged into origin/develop.. 
- Now, your teammates should merge the new code to their local copies.. **WHAT ARE THE NEXT STEPS**? (LET'S FIND OUT..)

## Tuesday 10:00 AM (Update the project + submodule's code)

- Before you continue working on your local branch, you have to check if origin/develop has any new updates.. 

```
git fetch
```

- Ok, you've find that there are some updates.. let's get them into the local (feature) branch

```
git merge origin/develop
```

- Let's say, your teammates have edit the code of the submodule (**as you've done in Monday 10:30 AM)** :smile:
- **Question**. What is the state of the local submodule on your side after merging the code from origin/develop?
    - **IMPORTANT IF** : **If** the submodule code is not updated after mergin from origin/develop, then the project might not even build successfully!!
        - So how to make sure the submodule is updated after a merge from origin/develop is performed?

- Answer. 
    - After merging origin/develop into your local branch, you'll have two issues: 
    1. The code in the submodule is old (not updated)..
    2. The Submodule pointer is deattached from HEAD to a commit that does not exist..
        - You can verify this by running `git status` in both: the root directory and the submodule directory).. 

    - **The fix**:

```
git submodule update --recursive --remote 
```

- Now, try `git status` in both the root directory and inside the submodule directory and you'll find them both clean..


## To conclude:

1. Update your local copy of both base project and the submodules: 
   1. Merge develop/origin into local/feature
   2. Perform submodule update
  
2. Edit a submodule's code
    1. Perform add, commit, and push inside submodule directory, 
    2. Then perform add, commit and push inside the base project directory..

---

## Remove a submodule

1. `git rm path/to/submodule # you're at the root directory of the git repo`

2. Manual clean up:
    1. Remove the submodule code directory
    2. Open .git hidden directory, 
        1. In modules directory, remove Submodule-name directory..
        2. Open cofig file and remove the text that mentions the submodule-name
    3. Open .gitmodules file and remove the text that refers to the submodule-name

3. [Optional]: `git rm --cached mymodule # may not be needed`


## Misc

1. You want to clone a repo with its submodules

```
git clone --recurse-submodules [repo-url]
```

2. You've cloned a repo with `git clone [repo-url]` but forgot about the submodules..

```
git submodule update --init --recursive
```

3. You've opend the terminal and wonder if there are any updates to the remote submodules 

```
git fetch --recurse-submodules
```
