---
title: How to roll back your unwanted commit(s)
date: 2016-09-01
category: Version Control
tag: git
---

Sometimes we need roll back our git commit or change the commit history in project.

## Locally change the commit

1\. revise the latest commit

{% highlight bash %}
# only change the commit content
git commit --amend # it will bring you into a text editor interface to change latest commit content
# or only change the file operation
git add <filename> # or git rm
git commit --amend # reguard the unstaged as the latest commit
{% endhighlight %}

2\. revise multiple commits before the latest commit, rebasing

{% highlight bash %}
# view commit log
git log
# start the rebasing
git rebase -i HEAD~3 # then change the latest four commits, include HEAD~3 parent commit
# change the 'pick' into 'edit', save and exit editor, i.e. 'edit b928584 3rd commit changed'
# only change the commit content
git commit --amend
# or only change the file operation as above
git add <filename> # or git rm
git commit --amend
# end the rebase
git rebase --continue
# view commit log
git log
{% endhighlight %}

3\. reorder or remove some specific commit, rebasing

{% highlight bash %}
git rebase -i HEAD~3
# when editor opened, change the order of commit, 
# or remove a commit line, maybe need to use 'git add --all' and 'git commit --amend' to fix the conflict
{% endhighlight %}

4\. squash or compress multiple commits, rebasing

{% highlight bash %}
git rebase -i HEAD~3
{% endhighlight %}
when editor opened, change latest 2 commits, such as

{% highlight text %}
pick c8a735a 1st commit
squash c15d014 2nd commit
squash e9ab4f5 latest commit
{% endhighlight %}
these three commits will meld into the '1st commit'

{% highlight bash %}
git commit --amend # then edit the '1st commit'
git rebase --continue
{% endhighlight %}

5\. rebasing command also can splitting a commit

6\. reset the HEAD index

{% highlight bash %}
git reset <commit hash> [<filename>]
{% endhighlight %}

7\. revert a commit

{% highlight bash %}
git revert <commit hash> # commit a rollback to undo some specific commit
{% endhighlight %}

6\. ultimate solution: filter-branch
worked on the entire commit history, not just one commit

{% highlight bash %}
git filter-branch --tree-filter 'rm -f passwords.txt' HEAD # remove all passwords.txt in every commit
git filter-branch --tree-filter 'rm -f *~' HEAD # remove all unexpected backup file
git filter-branch --subdirectory-filter trunk HEAD # make the trunk directory as a new root directory of all the commit
{% endhighlight %}

## Change the remote commit
after the operation abvoe, you can change the remote commit

{% highlight bash %}
git push origin branch # used when git revert, recommended
git push --f origin branch # 'origin branch' is optional for 'master' branch by default, danger
{% endhighlight %}

## Danger command

{% highlight bash %}
git reset --hard <commit hash> [<filename>] # this is a danger command, to discard all works in working directory
git checkout -- <filename> # you will lose your primary file
{% endhighlight %}

## Recover your revision
{% highlight bash %}
git reflog # find the commit hash before your revision
git reset <commit hash> # move index to the commit
{% endhighlight %}

## Refer
- [Git-Tools-Rewriting-History](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)
- [git如何回滚远程仓库](http://blog.mtxcxin.cn/blog/git%E5%A6%82%E4%BD%95%E5%9B%9E%E6%BB%9A%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93.html)