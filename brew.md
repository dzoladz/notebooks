Homebrew - MacOS Package Manager
================================

1. Get it!

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## Command Cheat Sheet

#### Globals

`brew update`	update packages
`brew list --versions`	show installed packages
`brew doctor` check system for potential problems
`brew help` print help info

#### Packages

`brew install hugo`	install package
`brew upgrade hugo`	upgrade a package
`brew switch hugo 0.5.3` switch package to specific version

`brew info hugo`	list versions, caveats
`brew cleanup hugo`	remove old versions
`brew edit hugo`	edit formula
`brew cat hugo`	print formula
`brew home hugo`	open project homepage
`brew pin hugo` prevent the specified formulae from being upgraded
`brew unpin hugo` unpin package and allow upgrades
