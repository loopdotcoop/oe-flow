oe-flow
=======

A development workflow compatible with git-flow and Semantic Versioning




Getting Started
---------------

Managing a project with [oe-flow](https://github.com/loopdotcoop/oe-flow) is easy. This section explains how to set up a new oe-flow project step-by-step.


### Set up GitHub

Create a new repository on GitHub:  

- Write a snappy description, 120 characters or less including the period `.` on the end
- Select “Initialize this repository with a README”
- Add .gitignore Meteor, for a Meteor project
- Add GPL v2 License

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




Changelog
---------

+                   Initial commit
+ 0.0.1-1           create ‘package.json’; add ‘Changelog’ section to README.md; 
```

Commit the changes:  
`$ git commit -am '+ 0.0.1-1           create ‘package.json’; add ‘Changelog’ section to README.md; '`


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

At this point __oe-flow__ diverges from [Vincent Driessen’s branching model](http://nvie.com/posts/a-successful-git-branching-model/), which recommends that development continues without incrementing the version:

- `"0.1.0"` continues as `"0.1.0-1"`, `"0.1.0-2"`, ✖
- `"4.7.22"` continues as `"4.7.22-1"`, `"4.7.22-2"`, ✖

If we followed that convention, the [Semantic Versioning](http://semver.org/) precedence of these development versions would actually be _lower_ than the current stable release. Therefore, oe-flow requires the following:

- `"0.1.0"` continues as `"0.1.1-1"`, `"0.1.1-2"`, ✔
- `"4.7.22"` continues as `"4.7.23-1"`, `"4.7.23-2"`, ✔

To reiterate:  
_Immediately after a release, the __develop__ patch-level is bumped._

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
+ 0.0.1-1           create ‘package.json’; add ‘Changelog’ section to README.md; 
+ 0.0.1-2           improve .gitgnore; 
+ 0.1.0-rc.1        prepare for initial release; 
+ 0.1.0             ready for initial release; 
+ 0.1.1-1           improve .gitgnore (correction); 
+ 0.1.1-2           ‘Getting Started’ section added to ‘README.md’ from ‘oe-flow-getting-started.md’; 



