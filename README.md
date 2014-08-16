oe-flow
=======

### A development workflow compatible with git-flow and Semantic Versioning.

The patch-level of every development version is an odd number. The patch-level of every release is an even number. Hence ‘odd-even-workflow’, or ‘oe-flow’.




Usage
-----

[oe-flow](https://github.com/loopdotcoop/oe-flow) follows a subset of [Semantic Versioning 2.0.0](http://semver.org/), combined with the [git-flow](https://github.com/nvie/gitflow/wiki/Command-Line-Arguments) flavor of Vincent Driessen’s [branching model](http://nvie.com/posts/a-successful-git-branching-model/). 


### Using the ‘develop’ branch

On the __develop__ branch, versions use a pre-release string in one of three forms, `-*`, `-alpha.*`, or `-beta.*`, where `*` is one or more digit:

1. The working directory initially just contains GitHub’s ‘README.md’ file. The initial commit message is `Initial commit`

2. ‘package.json’ is then created in the top-level of the working directory, its `"version"` field is set to `"0.0.1-1"`, and the second commit message is `+ 0.0.1-1 create ‘package.json’; add ‘Changelog’ section to README.md; `

3. From then on, the ‘package.json’ `"version"` field must be manually incremented before each commit, `"0.0.1-2"` → `"0.0.1-3"` ... `"0.0.1-146"`

4. At some point, the __develop__ branch is ready for alpha testing. The pre-release string becomes `-alpha.1`, which means the `"version"` field must be bumped `"0.0.1-146"` → `"0.0.1-alpha.1"`. After that it increments before each commit, `"0.0.1-alpha.2"` → `"0.0.1-alpha.3"` ... `"0.0.1-alpha.29"`

5. Later on, the __develop__ branch is ready for beta testing, `"0.0.1-alpha.29"` → `"0.0.1-beta.1"` → `"0.0.1-beta.2"` ... etc


### Using ‘feature’ branches

On a __feature__ branch, versions use a pre-release string in the form `-x.*`, where `x` is all lowercase ASCII letters, and is not `alpha | beta | rc | fix`:

1. `$ git flow feature start zippier` creates a new branch named __feature/zippier__, based on the __develop__ branch, and switches to it

2. ‘package.json’ `"version"` continues to increment on the __develop__ branch, but the new __feature/zippier__ branch bumps `"4.3.3-alpha.22"` → `"4.3.3-zippier.1"`

3. So the __develop__ branch continues `"4.3.3-alpha.22"` → `"4.3.3-alpha.23"`, while in parallel the __feature/zippier__ branch increments before each commit, `"4.3.3-zippier.1"` → `"4.3.3-zippier.2"` ... `"4.3.3-zippier.53"`

4. The final __feature/zippier__ branch commit must be preceded by replacing the `"version"` field with an incremented __develop__ branch version, so if __develop__ had reached `"4.3.7-14"`, then `"4.3.3-zippier.53"` → `"4.3.7-15"`

5. `$ git flow feature finish zippier` merges the new feature into __develop__, removes the __feature/zippier__ branch, and switches back to __develop__


### Using ‘release’ branches

On a __release__ branch, versions use a pre-release string in the form `-rc.*`:

1. Assuming the ‘package.json’ `"version"` field in the __master__ branch is `"1.1.7-beta.53"`, decide whether the release will bump it to `"1.1.8"`, `"1.2.0"` or `"2.0.0"`. For more info, see [semver.org](http://semver.org/)

2. `$ git flow release start 1.2.0` creates a new __release__ branch based on the __develop__ branch named __release/1.2.0__, and switches to it

3. The `"version"` field must be manually bumped `"1.1.7-beta.53"` → `"1.2.0-rc.1"`, and then incremented before each subsequent commit, `"1.2.0-rc.2"` → `"1.2.0-rc.3"` ... `"1.2.0-rc.26"`

4. `-rc.*` must be removed from the `"version"` field, `"1.2.0-rc.41"` → `"1.2.0"`, just before the final __release/1.2.0__ commit

5. `$ git flow release finish 1.2.0` merges __release/1.2.0__ into __master__ and also into __develop__, deletes the __release/1.2.0__ branch, tags the release with __1.2.0__, and switches back to __develop__. `git flow` opens the current `GIT_EDITOR` three times:
  - __master__ commit text, appends `Merge branch 'release/0.2.0'`
  - __0.2.0__ tag description
  - __develop__ commit text, appends `Merge branch 'release/0.2.0' into develop`

  > If using `ed` as `GIT_EDITOR`:  
  > - Enter `a` at the prompt to start appending  
  > - Write one or more lines of descriptive text  
  > - Enter just `.` on a line, to get the prompt back  
  > - Enter `w` at the prompt to write the message to output  
  > - Enter `q` at the prompt to finish  

6. `$ git push --tags origin master` will update GitHub, including tags

7. Immediately after a release, the __develop__ patch-level is _must_ be bumped, so the  `"version"` field is changed from `"1.2.0"` → `"1.2.1-1"`


### Using ‘hotfix’ branches

On a __hotfix__ branch, versions use a pre-release string in the form `-fix.*`:

1. Assuming the ‘package.json’ `"version"` field in the __master__ branch is `"1.2.0"`, decide whether the hotfix will bump it to `"1.2.2"`, `"1.3.0"` or `"2.0.0"`. Generally, a patch-level bump is appropriate for hotfixes

2. `$ git flow hotfix start 1.2.2` creates a new hotfix branch based on the __master__ branch named __hotfix/1.2.2__, and switches to it

3. The `"version"` field must be manually bumped `"1.2.0"` → `"1.2.2-fix.1"`, and then incremented before each subsequent commit, `"1.2.2-fix.1"` → `"1.2.2-fix.2"` ... `"1.2.2-fix.29"`

4. `-fix.*` must be removed from the `"version"` field, `"1.2.2-fix.29"` → `"1.2.2"`, just before the final __hotfix/1.2.2__ commit

5. `$ git flow hotfix finish 1.2.2` merges __hotfix/1.2.2__ into __master__ and also into __develop__, deletes the __hotfix/1.2.2__ branch, tags the release with __1.2.2__, and switches back to __develop__. For help with entering text at this point, see [__Using ‘release’ branches__](#using-release-branches)

6. `$ git push --tags origin master` will update GitHub, including tags

7. Immediately after a hotfix, the __develop__ patch-level is _must_ be bumped, so the  `"version"` field is changed from `"1.2.2"` → `"1.2.2-1"`



### The ‘master’ branch

Every commit on __master__ is a new release. The ‘package.json’ `"version"` field always matches a Git tag, and is always in three parts, eg `"0.7.16"` or `"1.2.0"`. 




Getting Started
---------------

Managing a project with [oe-flow](https://github.com/loopdotcoop/oe-flow) is easy. This section explains how to set up a new oe-flow project step-by-step.


### Set up GitHub

Create a new repository on GitHub:  

- Write a snappy description, 120 characters or less including the period `.` on the end
- Select “Initialize this repository with a README”
- Add .gitignore Meteor, for a Meteor project
- Add GPL v2 license

In the ‘HTTPS clone URL’ sidebar, click ‘Copy to clipboard’


### Set up a local development repo

For a faster dev experience (though less secure), paste the URL into a text document…  
`https://github.com/[account-name]/[repo-name].git`  
…and add your username and password separated by a colon:  
`https://[my-username]:[my-password]@github.com/[account-name]/[repo-name].git`

You can change this at any time:  
`$ git remote set-url origin https://[new-username]:[new-password]@github.com/[new-account-name]/[new-repo-name].git`

Note that punctuation in your password may break things… the carat-character `^` is allowed, but many other punctuation characters are not.

Clone the GitHub repo to your local machine:  
`$ git clone https://[my-username]:[my-password]@github.com/[account-name]/[repo-name].git`

Change to the working directory:  
`$ cd [repo-name]/`

Initialize git-flow, using [default](https://github.com/nvie/gitflow/wiki/Command-Line-Arguments#git-flow-init--fd) branch names…  
`$ git flow init -d`  
…which adds the __develop__ branch, and switches to it.


### First local commit: `"0.0.1-1"`

Create a ‘package.json’ file in the top-level of the working directory, with the `"version"` field set to `"0.0.1-0"`:
```json
{
  "name": "[repo-name]",
  "version": "0.0.1-1",
  "description": "[repo-description]",
  "scripts": {
    "test": "echo 'Error: no test specified' && exit 1"
  },
  "author": "[author-name] <[author-email]>",
  "contributors": [
  ],
  "license": "GPL v2"
}
```

Start tracking ‘package.json’:  
`$ git add .`

Append the following to ‘README.md’:
```




Workflow
--------

This project uses the [oe-flow](https://github.com/loopdotcoop/oe-flow) development workflow, which is compatible with 
[git-flow](https://github.com/nvie/gitflow/wiki) and [Semantic Versioning 2.0.0](http://semver.org/).




Changelog
---------

+                   Initial commit
+ 0.0.1-1           create ‘package.json’; add ‘Changelog’ and ‘Workflow’ sections to ‘README.md’; 
```

Commit the changes:  
`$ git commit -am '+ 0.0.1-1           create ‘package.json’; add ‘Changelog’ and ‘Workflow’ sections to ‘README.md’; '`


### Second local commit: `"0.0.1-2"`

Increment the ‘package.json’ `"version"` field:  
`  "version": "0.0.1-2",`

Improve .gitgnore for a Meteor project:
```
# OS
.DS_Store
*.bak
*.swp
Thumbs.db

# Versioning software
.svn
.cvs

# Meteor
.meteor/local
.meteor/meteorite
```

Append the ‘Changelog’ section:  
`+ 0.0.1-2           improve .gitgnore; `

Commit the changes:  
`$ git commit -am '+ 0.0.1-2           improve .gitgnore; '`


### Set up `$ git push`

Tell Git where to push the __develop__ branch:  
`$ git push --set-upstream origin develop`

You may like to run…  
`$ git config --global push.default matching`  
…to squelch Git 2.0 warnings on your machine.


### Initial release `"0.1.0"`

We could release development version `"0.0.1-2"` as `"0.0.2"`, `"0.1.0"`, or `"1.0.0"`.
We have (slightly) changed application behaviour, because it now responds to:  
`$ npm test`

So according to [Semantic Versioning](http://semver.org/), the release must be version `"0.1.0"`:  
`$ git flow release start 0.1.0`

Bump the ‘package.json’ `"version"` field:  
`  "version": "0.1.0-rc.1",`

Append the ‘Changelog’ section:  
`+ 0.1.0-rc.1        prepare for initial release; `

Commit the changes:  
`$ git commit -am '+ 0.1.0-rc.1        prepare for initial release; '`

At this point, we could make several rounds of amends, incrementing the `"version"` field and adding to the Changelog as we go. 
When ready to release, remove `-rc.*` from the ‘package.json’ `"version"` field:  
`  "version": "0.1.0",`

Append the ‘Changelog’ section:  
`+ 0.1.0             ready for initial release; `

Commit the release:  
`$ git commit -am '+ 0.1.0             ready for initial release; '`

Use the git-flow extension to update the __master__ and __develop__ branches, add the tag, etc:  
`$ git flow release finish 0.1.0`

If your `GIT_EDITOR` is `ed`, repeat this sequence of keystrokes three times:  
`a [return] Initial release [return] . [return] w [return] q [return]`

Push the release to GitHub…  
`$ git push`  
…which will push both __develop__ and __master__…  
`$ git push --tags`  
…which will push the new tag


### The next round of development

At this point __oe-flow__ diverges from Vincent Driessen’s [branching model](http://nvie.com/posts/a-successful-git-branching-model/), which recommends that development continues without incrementing the version:

- `"0.1.0"` continues as `"0.1.0-1"`, `"0.1.0-2"`, ✖
- `"4.7.22"` continues as `"4.7.22-1"`, `"4.7.22-2"`, ✖

If we followed that convention, the [Semantic Versioning](http://semver.org/) precedence of these development versions would actually be _lower_ than the current stable release. Therefore, oe-flow requires the following:

- `"0.1.0"` continues as `"0.1.1-1"`, `"0.1.1-2"`, ✔
- `"4.7.22"` continues as `"4.7.23-1"`, `"4.7.23-2"`, ✔

__*IMPORTANT:*__  
Immediately after a release, the __develop__ patch-level is bumped.

The __develop__ branch may reach `"0.1.1-53"` or `"4.7.23-beta.22"` before it is ready to be released. At that point, the developer should release to the __master__ branch as:

- `"0.1.2"` for a bug-fix, `"0.2.0"` if backwards-compatible, `"1.0.0"` if not
- `"4.7.24"` for a bug-fix, `"4.8.0"` if backwards-compatible, `"5.0.0"` if not

For oe-flow projects:

- An _odd_ patch-level signifies a __feature__ or __develop__ branch
- An _even_ patch-level signifies a __release__, __hotfix__, or __master__ branch

> Note: the ‘patch-level’ is the third number in a version.




Changelog
---------

+                   Initial commit
+ 0.0.1-1           create ‘package.json’; add ‘Changelog’ section to ‘README.md’; 
+ 0.0.1-2           improve .gitgnore; 
+ 0.1.0-rc.1        prepare for initial release; 
+ 0.1.0             ready for initial release; 
+ 0.1.1-1           improve .gitgnore (correction); 
+ 0.1.1-2           ‘Getting Started’ section added to ‘README.md’ from ‘oe-flow-getting-started.md’; 
+ 0.1.1-3           ‘Usage’ section added to ‘README.md’ from ‘git-flow-test-2/README.md’; 
+ 1.0.0-rc.1        minor text amends; 
+ 1.0.0             oe-flow is ready to use; 
+ 1.0.1-1           minor text amends; suggest a ‘Workflow’ section for new projects; 

