GitRedmineResolver
==================

Description
-----------
GitRedmineResolver is a git _post-receive_ hook that will scan commit messages for messages in the form of "Resolves issue #294", and will update the corresponding issue in your Redmine system.

How it works
------------

GitRedmineResolver will scan commit messages after you push for strings in the form of:

(Resolving verb) (optional issue type noun) (comma or space separated list of issue numbers)

The verbs we look for merging/resolving are merge, merged, merging, resolves, resolved, fixes, fixed.
The verbs we look for in progress/commits are in progress, commit, committed, committing, working, working on, worked on, doing, did, continuing.
The verbs we look for feedback/pull requests are pull request, pull requested, pr, feedback, feedback for.
The issue nouns are issue,task,feature, and bug
Issue numbers can be singular, or comma or space separated.  Hash signs are safe to use but optional.

Examples of matching strings for merging/resolving:
* Resolves issue #142
* Fixes bug 148
* Merged feature #123 456 789

Examples of matching strings for in progress/commits:
* In progress issue #142
* Committed bug 148
* Working feature #123 456 789

Examples of matching strings for feedback/pull requests:
* Pull requested issue #142
* Feedback for bug 148
* Feedback feature #123 456 789

GitRedmineResolver will then resolve and add a comments to the corresponding issues, with a link to the commit in the Redmine repository browser.  It would be trivial to extend this script to accept more verbs that take different actions in Redmine (e.g., close issues), but since this is not part of our workflow that exercise is left to the reader.


Dependencies
------------

You have python and setuptools installed, and you should already have your repository syncing to a directory on your Redmine server.  If you don't, I recommend also doing that with a hook.

Install
-------

1. Install the dependencies (pyactiveresource and gitpython)

		pip install -r requirements.txt
	
	
2. Clone GitRedmineResolver somewhere

		cd /var/lib
		sudo mkdir gitredmine
		sudo chmod 777 gitredmine
		cd gitredmine
		git clone git@github.com:kwood/GitRedmineResolver.git

3. Set up a user in Redmine with permissions to resolve and comment on issues.

4. Set up the post-receive hook in your repository that calls GitRedmineResolver and passes stdin to it:

	Move the post-receive file into your hook directory and make sure to edit the information so it matches you.
	Clarification: this hook directory is where you are pushing to, whether you set up a local repo or one on our servers.
	In the hooks folder after creating or moving the post-receive file there, run this.

		chmod +x post-receive
