# OpenDev contribution

This guide aims to give you an easy intro on how to contribute to OpenDev using Gerrit and Git-review.
It is assumed, that you have created yourself an account on OpenDev already AND that you have accepted the ICLA agreement
<https://review.opendev.org/settings/#Agreements>.

## Getting started on your machine

First you need to install git-review, an extension to git that allows interaction with Gerrit. Pick the suiteable installation
command from below.

```sh
brew install git-review
sudo apt install git-review
sudo yum install git-review
sudo dns install git-review
sudo zypper install git-review
```

Once done, git-review needs to be configured to use the username you have configured already in OpenDevs Gerrit web-page.
This might look like this:

```sh
git config --global gitreview.username {{YOUR-GERRIT-USERNAME}}
```

## Working on a repository

Every time you checkout a new repository and want to work on it, you should initialize **git-review** first:

```sh
git review -s
```

To check if this worked properly, you may use:

```sh
git remote -v
gerrit  ssh://tibeer@review.opendev.org:29418/openstack/kolla-ansible.git (fetch)
gerrit  ssh://tibeer@review.opendev.org:29418/openstack/kolla-ansible.git (push)
origin  https://opendev.org/openstack/kolla-ansible.git (fetch)
origin  https://opendev.org/openstack/kolla-ansible.git (push)
```

As you can see above, you should have two additional remotes just for gerrit. From this point, you may basically use the same git
commands you were used to with just one exception: Do not use **git push**.

## How to push changes?

First, check if there is a folder *releasenotes*. If so, you may want to create a releasenote which reflects reasons for your
changes. To create a releasenote you should use the tool *reno*. It can be installed via pip:

```sh
pip3 install reno
```

*reno* provides you with template functions and proper naming with a few additional functions that are helpful. An example call
of *reno* might look like this:

```sh
reno new deprecate-sanity-checks --from-template releasenotes/templates/features.yml
```

The above creates a new release note based on the templates **releasenotes/templates/features.yml**. The name
**deprecate-sanity-checks** should be eqivalent to the branch name you used for your change. In this case the branch name might
have looked like this: **feat/deprecate_sanity_checks**

:::note

>**Note:** reno names should never use underscores.

:::

## How to re-push changes?

You pushed you changes and get notified that some pipeline failed, maybe due to a trailing whitespace. So you need to fix this,
but how? Just like before, use normal git commands to add your changes **git add .** . When commiting use **git commit --amend**
and make sure that the last line in your commit still containts the *Change-Id* which was generated by the inital **git review**.
If you remove this, Gerrit get's confused and cannot connect you change with the previous one.