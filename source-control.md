Source Control standards
========================

The standard tool for source control on all projects is git.

Commit Messages
---------------

A [good commit message][1] looks like this:

	Header line: explain the commit in one line (use the imperative)

	Body of commit message is a few lines of text, explaining things
	in more detail, possibly giving some background about the issue
	being fixed, etc etc.

	The body of the commit message can be several paragraphs, and
	please do proper word-wrap and keep columns shorter than about
	74 characters or so. That way "git log" will show things
	nicely even when it's indented.

	Make sure you explain your solution and why you're doing what you're
	doing, as opposed to describing what you're doing. Reviewers and your
	future self can read the patch, but might not understand why a
	particular solution was implemented.

[1]: https://github.com/torvalds/subsurface/blob/master/README

Release Branching Strategy
--------------------------

The goal of this document is to outline a typical path for releases in git.

We'll start with the following working tree:

    A---B (master)

Now we'll start working on a feature. At this point, we'll open up a topic
branch to work on.

    git checkout -b topic

After doing some work in our topic branch, we make a commit and our working tree
will look like the following:

          C (topic)
         /
    A---B (master)

Now our Topic is ready to be reviewed. We'll open up a Pull Request targeting
the `master` branch for peer review. At this point someone points out a typo in
a comment. We'll fix the issue and make a commit. Our local working Tree now
looks like the following:

          C---D (topic)
         /
    A---B (master)

Before we push, we'll clean up these commits. Since the changes made in D were a
mistake, they should have been in C to begin with. For a situation like this it
makes sense to combine these commits together. We'll do that with an interactive
rebase.

    git rebase -i HEAD~2

This will bring up an editor that looks something like the following:

    pick C some message for changes in C
    pick D Fixing typo in C

    # Rebase 1f057e2..d965c4b onto 1f057e2

This editor allows us to re-arrange, modify and squash commits. In this case we
want to squash C up the tree into D. We do this by changing the word `pick` in
front of the D commit to `squash` like the following:

    pick C some message for changes in C
    squash D Fixing typo in C

    # Rebase 1f057e2..d965c4b onto 1f057e2

After this, it will allow you to change the commit message, and afterwards we'll
have a new commit in place of C and D. Our tree will look something like this:

          E (topic)
         /
    A---B (master)

Next we'll push our changes for approval. We've re-written the tree, evident by
the fact that commit C no longer exists. So we'll need to do a force push here.
We'll only want to force push our topic branch so we'll be specific in our push
command:

    git push --force origin topic

Once our new Changes have been approved, we can merge these changes from github
on the pull request. This is the equivalent of running the command from the
master branch:

    git merge --no-ff topic

In either case, we'll end up with the following tree:

          E
         / \
    A---B---F (master)

Now we think our branch is ready for release. Since there are bound to be some
QA fixes before it's actually released, we'll open up a separate branch for the
minor release. This will allow us to work on new functionality in the `master`
branch while we work on QA fixes in the release branch.
When we do this, our tree will initially look like this, with master and the
release branch pointing at the same commit:

          E
         / \
    A---B---F (master / 1.6.x)

Now we'll start working on a new feature. Something that will not be in our
upcoming release, but we want to start working on. Just like before we'll open
up a new topic branch and start committing to it:

          E   G---H (new-topic)
         / \ /
    A---B---F (master / 1.6.x)

Meanwhile, we'll also make a bugfix to the release branch fixing some QA issues.
We'll branch off of the release branch `1.6.x` and start making changes.
Our Tree may look like this:

          E   G---H (new-topic)
         / \ /
    A---B---F (master)
             \
              I (1.6.x)

Next we need to synchronize our release and master branch. This must happen for
two man reasons.
Firstly, This bug may also exist in our `master` branch, so we'll need to merge
it upwards.
Secondly, we want to ensure that all of these commits can be reached from our
master working tree. This will become important when our release gets tagged
later.
Assuming there are no conflicts, this may be done without a pull request, since
the changes will have already been approved for the release branch. We can
merge our changes in the release branch by running:

    git merge --no-ff 1.6.x

Afterwards we'll have a commit tree like the following:

         E   G---H (new-topic)
        / \ /
    A---B--F---J (master)
            \ /
             I (1.6.x)


QA Has now approved our changes, and our release is being published. We'll mark
the commit that was released with a tag.
We'll do that by making a [signed tag][2] with the format `vX.Y.Z` and pushing
it.

    git tag -s v1.6.9

Now our tree will look something like:

          E   G---H (new-topic)
         / \ /
    A---B---F---J (master)
             \ /
              I (1.6.x)
               \
                * (Tag: v1.6.9)

At this point we can also close our release branch `1.6.x`. If there are more
bugfixes that are needed on that release, we can always re-open the branch using
the tag.

          E   G---H (new-topic)
         / \ /
    A---B---F---J (master)
             \ /
              I
               \
                * (Tag: v1.6.9)

Now our new feature is finished. However, since our bugfix was merged at G,
there have been changes in master since we started. We could merge these into
our branches as well, but since those changes are not public yet, it makes more
sense to do a rebase.
This can be accomplished by running the following on our `new-topic` branch:

    git pull --rebase origin master

This will pick up our changes and re-apply them on the new tree. In this case,
Where previously our topic started at commit F, it will now start at commit J:

          E       G---H (new-topic)
         / \     /
    A---B---F---J (master)
             \ /
              I
               \
                * (Tag: v1.6.9)

Now our topic is ready to be reviewed and merged. Just like before, we'll end up
with a tree like:


          E       G---H
         / \     /     \
    A---B---F---J-------K (master)
             \ /
              I
               \
                * (Tag: v1.6.9)

Next the inevitable happens, and we need to do a bugfix on our release.
We'll re-open the branch on the tag with the following command:

    git checkout -b 1.6.x v1.6.x

We'll make our changes and merge them upwards again ending with a working tree:


          E       G---H
         / \     /     \
    A---B---F---J-------K---M (master)
             \ /           /
              I-----------L (1.6.x)
               \
                * (Tag: v1.6.9)

Finally, just as before, when the changes are published, we'll tag the results
and close our release branch again:


          E       G---H
         / \     /     \
    A---B---F---J-------K---M (master)
             \ /           /
              I-----------L
               \           \
                \           * (Tag: v1.6.10)
                 \
                  * (Tag: v1.6.9)

[2]: https://ariejan.net/2014/06/04/gpg-sign-your-git-commits/

### Release Tags

Releases should be designated with Tags, and all Git tags should be signed by
the tagger with a trusted PGP key.
