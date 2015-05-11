# Introduction to gist.el configuration
gist.el is an elisp package which integrates GitHub Gist into Emacs.  It allows listing, creating, modifying and deleting Gists from within Emacs.  See https://github.com/defunkt/gist.el for more details.

What's not so obvious without looking into gist.el's sources is how to configure it to support multiple GitHub Gist back ends.  For example, given the case you are working against your company's own GitHub Enterprise server in addition to the public github.com server and you want to be able to access Gists on both servers.  This guide will give you some instructions on how to configure gist.el to support such multi back end environments.

# Multiple gist.el GitHub profiles
gist.el uses gh.el elisp package to access GitHub via its API. gh.el provides support for multiple back ends.  A configuration is stored in a profile and it supports multiple ones.  By default, it comes preconfigured with a single profile called github, which accesses GitHub's API via https://api.github.com.

## Listing profiles
`M-x customize-group gh-profile` will allow you to inspect Gh Profile Alist (`gh-profile-alist` variable).  As mentioned before, it should have a single entry called github.

## Adding a profiles
Adding a new back end is as easy as adding a new entry to this alist.  For example, call it work, specify the url to access it, e.g. https://github.mycompany.com/api/v3.  Do not enter a slash at the end, as one gets added by gh.el using string concat.  You might get an errer similar to `if: Wrong type argument: listp, "https://developer.github.com/enterprise/2.2/v3"` if you do so and it's not so obvious that this could be the reason why.  Furthermore, copy and adapt the remote-regex appropriately.

## Selecting a profile
If you call `gist-list` the first time and `gh-profile-current-profile` is unset or `nil`, you will get a prompt in the minibuffer to specify the profile you want to use.  In any buffer, if you set `gh-profile-current-profile` to a profile name (e.g. `"github"` or `"work"`) using setq and call a gist.el function (e.g. `gist-list`) from this buffer as current, it will select this profile rather than popping up the prompt.

# Appendix
This guide assumes that you installed gh.el and all of its dependencies as documented at https://github.com/defunkt/gist.el. It was written at the time Version 20150505.1341 was available from http://http://melpa.org/.
