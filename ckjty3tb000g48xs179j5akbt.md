## Partial file staging with git

After typing `git status` you can see files in three states:
* added to the index
* not added to the index
* not tracked at all  

It's likely to have the same file added and not added to the index.
How is that possible?

Let me introduce partial file staging. What does it do?
It allows you to add only specific changes to the index.
For example, you want to add only new code without format changes in the whole file.

I'm a big fan of this approach because it allows you to create small semantic commits. 
There is one catch that always makes me search the docs, though. 
Git will try to split all the changes for you. You will see each change and decide if you want to stage it or not.
But what happens when git is wrong or the change is too big, and it can't split it further?

Of course, you can edit the so-called _hunk_ manually. And this is the place where I always fail.

Let's take a look at an example.
After typing `git add -p`, we see this.
![change.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1610451983193/xTLabvNAk.png)

I want to add only one endpoint. This change is too big, so I press `e` and edit hunk manually.
![edited-hunk.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1610451995265/U5bc4F7f7c.png)

This picture always looks scary to me.
There is some manual under the code, but I never understood it.
What to do to keep just one of the changes?
Just delete the parts that you don't need!
![edited-hunk.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1610452018516/eAJMYWyYe.png)

Now we can save it and close the editor.
The current status looks like this:
![status-after-edit.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1610452024665/tIi-4uzP-.png)

I have two changes, and only one ready to be committed. I can verify it with `git diff --cached`
![diff-cached.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1610452030614/E23_bJKKO.png)

Perfect!
And where is the other change?
You should see it after `git diff`.
![diff.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1610452036932/cLI8MaaFg.png)

I'm ready to create two distinct commits.  
`git commit -m 'Added default endpoint'`  
And after adding the second change to the index.  
`git add .; git commit -m 'Ping endpoint'`  
The final stage looks like this:
![git-log.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1610452801084/dekkDFiBV.png)


Partial file staging is a powerful feature. At first glance, it can look cumbersome and not very intuitive. I use it daily, and after a few tries, it became my second nature.
