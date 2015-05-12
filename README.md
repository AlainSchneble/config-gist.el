# Introduction to gist.el configuration
gist.el is an elisp package which integrates GitHub Gist into Emacs.  It allows listing, creating, modifying and deleting Gists from within Emacs.  See https://github.com/defunkt/gist.el for more details.

What's not so obvious without looking into gist.el's sources is how to configure it to support multiple GitHub Gist back ends.  For example, given the case you are working against your company's own GitHub Enterprise server in addition to the public github.com server and you want to be able to access Gists on both servers.  This guide will give you some instructions on how to configure gist.el to support such multi back end environments.

# Multiple gist.el GitHub profiles
gist.el uses gh.el elisp package to access GitHub via its API. gh.el provides support for multiple back ends.  A configuration is stored in a profile and it supports multiple ones.  By default, it comes preconfigured with a single profile called github, which accesses GitHub's API via https://api.github.com.

## Listing profiles
`M-x customize-group gh-profile` will allow you to inspect Gh Profile Alist (`gh-profile-alist` variable).  As mentioned before, it should have a single entry called github.

## Adding additional profiles
Adding a new back end is as easy as adding a new entry to the `gh-profile-alist` using the customize interface.  For example, call it work, specify the url to access it, e.g. https://github.mycompany.com/api/v3.  Do not enter a slash at the end, as one gets added by gh.el using string concat.  You might get an errer similar to `if: Wrong type argument: listp, "https://developer.github.com/enterprise/2.2/v3"` if you do so and it's not so obvious that this could be the reason why.  Furthermore, copy and adapt the remote-regex appropriately.

## Selecting a profile
If you call `gist-list` the first time and `gh-profile-current-profile` is unset or `nil`, you will get a prompt in the minibuffer to specify the profile you want to use.  In any buffer, if you set `gh-profile-current-profile` to a profile name (e.g. `"github"` or `"work"`) using setq and call a gist.el function (e.g. `gist-list`) from this buffer as current, it will select this profile rather than popping up the prompt.

## Persisted Credentials in .gitconfig
Credentials (user, password and/or oauth-token) are automatically persisted in the global git config using `git config --global` (i.e. `~/.gitconfig` or `$XDG_CONFIG_HOME/git/config`) when they have been entered the first time in the prompt.  The default profile is stored without a descriminator like so:
```
[github]
	user = GitHubUsername
	password = HardToGuess
	oauth-token = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```
while additional profiles (e.g. `"work"`) are stored with the profile name as descriminator:
```
[github "work"]
	user = GitHubUsernameAtWork
	password = HardToGuess
	oauth-token = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```
Passwords are cached in `gh-auth-alist` once entered or read from git config.

## Persisted Credentials in `gh-profile-alist`
Passwords can also be specified in the entries of `gh-profile-alist`. Just use the keys `:username`, `:password` and `:token`, respectively. If specified there, git config won't be used at all for the corresponding profile.

# Appendix
This guide assumes that you installed gist.el and all of its dependencies as documented at https://github.com/defunkt/gist.el. It was written at the time Version 20150505.1341 was available from http://melpa.org/.
