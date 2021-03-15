# Commit style guide

## Overview

The following document provides a set of guidelines that should be followed by every developer who is working with git version control system on his projects in Syberry.

### Reference Redmine ticket in commit

Subject line of the commit must contain a reference to the ticket number if the commit was done as a result of resolving a task.

Example of `GOOD` style:
<pre>34331: Change styles for e-mail addresses</pre>
Example of `BAD` style:
<pre>Change styles for e-mail addresses</pre>

If a defect was related to a specific known feature that was reopened (due to this defect), subject line must include all affected ticket numbers separated with comma.

Example of `GOOD` style:
<pre>34332, 34331: Change styles for e-mail addresses</pre>
Example of `BAD` style:
<pre>Change styles for e-mail addresses</pre>

### Atomic commits

Every commit in git should be a logically connected change set. To keep commit changes set focused you should not include unrelated changes and create separate commits for them.
If you find it too difficult to succinctly describe your changes in a short commit message probably your commit is too big and should be split into multiple commits.
If you have worked on a bigger feature and create multiple commits, you should use git squash to create a single atomic commit for the feature implementation. It is okay to have separate commits with defect fixes for this feature or multiple commits for different stages of feature implementation if it is tested in parts.

### Pull requests

Pull request is a convenient method that allows technical leader to control the flow of development of all project code. Main development branch for a project should be protected to force every piece of code to pass code review before being integrated. There some basic rules that should be followed when you create merge request:

1. Merge request author is responsible for making his merge request integrated by fast-forward.
If you merge request requires manual conflict resolution, you must pull latest changes from the target branch and resolve every conflict manually yourself and then update your request.
2. Make sure you merge request is focused (the same way commit should be focused), do not create a big merge request with multiple features. 
This will ensure that your request will be processed faster.
3. Address every comment in merge request code review. If the comment was a simple fix, you may just write "Fixed" to make sure that you did not miss the comment accidentally.
4. It is your responsibility as the developer to pass code review and make your code integrated. Provide feedback and code updates as soon as possible.

### Commit message

The goal of a commit message is to concisely convey what is changed by this commit and why. There are seven rules to writing a great commit message:
1. Separate subject from body with a blank line
2. Limit the subject line to 50 characters
3. Capitalize the subject line
4. Do not end the subject line with a period
5. Use the imperative mood in the subject line
6. Wrap the body at 72 characters
7. Use the body to explain what and why vs. how

#### Separate subject from body with a blank line

From the git commit manpage:
<pre>
Though not required, it's a `GOOD` idea to begin the commit message with a single short (less than 50 character) 
line summarizing the change, followed by a blank line and then a more thorough description. The text up to the 
first blank line in a commit message is treated as the commit title, and that title is used throughout Git. 
For example, git-format-patch(1) turns a commit into email, and it uses the title on the Subject line and the 
rest of the commit in the body. </pre>

Firstly, not every commit requires both a subject and a body. Sometimes a single line is fine, especially when the change is so simple that no further context is necessary. For example:

<pre>
Fix typo in introduction to user guide
</pre>

Nothing more need be said; if the reader wonders what the typo was, he can simply take a look at the change himself, i.e. use git show or git diff or git log -p.

However, when a commit merits a bit of explanation and context, you need to write a body. For example:

<pre>
Derezz the master control program

MCP turned out to be evil and had become intent on world domination.
This commit throws Tron's disc into MCP (causing its deresolution)
and turns it back into a chess game.
</pre>

The separation of subject from body pays off when browsing the log. Here's the full log entry:
<pre>
$ git log
commit 42e769bdf4894310333942ffc5a15151222a87be
Author: Kevin Flynn <kevin@flynnsarcade.com>
Date:   Fri Jan 01 00:00:00 1982 -0200

 Derezz the master control program

 MCP turned out to be evil and had become intent on world domination.
 This commit throws Tron's disc into MCP (causing its deresolution)
 and turns it back into a chess game
</pre>

And now git log --oneline, which prints out just the subject line:

<pre>
$ git log --oneline
42e769 Derezz the master control program
</pre>

Or, git shortlog, which groups commits by user, again showing just the subject line for concision:

<pre>
Kevin Flynn (1):
      Derezz the master control program

Alan Bradley (1):
      Introduce security program "Tron"

Ed Dillinger (3):
      Rename chess program to "MCP"
      Modify chess program
      Upgrade chess program

Walter Gibbs (1):
      Introduce protoype chess program
</pre>

There are a number of other contexts in git where the distinction between subject line and body kicks in—but none of them work properly without the blank line in between.

#### Limit the subject line to 50 characters

50 characters is not a hard limit, just a rule of thumb. Keeping subject lines at this length ensures that they are readable, and forces the author to think for a moment about the most concise way to explain what's going on.
If you're having a hard time summarizing, you might be committing too many changes at once. Strive for atomic commits.

#### Capitalize the subject line

Example of `GOOD` style:
<pre>Accelerate to 88 miles per hour</pre>
Example of `BAD` style:
<pre>accelerate to 88 miles per hour</pre>


#### Do not end the subject line with a period

Trailing punctuation is unnecessary in subject lines.
Example of `GOOD` style:
<pre>Open the pod bay doors</pre>
Example of `BAD` style:
<pre>Open the pod bay doors.</pre>

#### Use the imperative mood in the subject line

A properly formed git commit subject line should always be able to complete the following sentence:

If applied, this commit will +*your subject line here*+

For example:
`_If applied, this commit will_ refactor subsystem X for readability`
`_If applied, this commit will_ update getting started documentation`

Notice how this doesn't work for the other non-imperative forms:
`_If applied, this commit will_ fixed bug with Y`
`_If applied, this commit will_ changing behavior of X`

Remember: Use of the imperative is important only in the subject line. You can relax this restriction when you're writing the body.

Example of `GOOD` style:
<pre>
Refactor subsystem X for readability
Update getting started documentation
</pre>

Example of `BAD` style:
<pre>
Fixed bug with Y
Changing behavior of X
</pre>

#### Wrap the body at 72 characters

Git never wraps text automatically. When you write the body of a commit message, you must mind its right margin, and wrap text manually.
The recommendation is to do this at 72 characters, so that git has plenty of room to indent text while still keeping everything under 80 characters overall.

#### Use the body to explain what and why vs. how

In most cases, you can leave out details about how a change has been made. Code is generally self-explanatory in this regard (and if the code is so complex that it needs to be explained in prose, that's what source comments are for). Just focus on making clear the reasons you made the change in the first place—the way things worked before the change (and what was wrong with that), the way they work now, and why you decided to solve it the way you did.


### Examples of `GOOD` commits

#### Large change

<pre>
Allow remote builds without sending the derivation closure

Previously, to build a derivation remotely, we had to copy the entire closure of the .drv file 
to the remote machine, even though we only need the top-level derivation. This is very wasteful: the 
closure can contain thousands of store paths, and in some Hydra use cases, include source paths that 
are very large (e.g. Git/Mercurial checkouts).

So now there is a new operation, StoreAPI::buildDerivation(), that performs a build from an in-memory 
representation of a derivation (BasicDerivation) rather than from a on-disk .drv file. The only files 
that need to be in the Nix store are the sources of the derivation (drv.inputSrcs), and the needed 
output paths of the dependencies (as described by drv.inputDrvs). "nix-store --serve" exposes this interface.

Note that this is a privileged operation, because you can construct a derivation that builds any store 
path whatsoever. Fixing this will require changing the hashing scheme (i.e., the output paths should be 
computed from the other fields in BasicDerivation, allowing them to be verified without access to other derivations). 
However, this would be quite nice because it would allow .drv-free building (e.g. "nix-env -i" wouldn't have 
to write any .drv files to disk).

Fixes #173.
</pre>
[Source](https://github.com/NixOS/nix/commit/1511aa9f488ba0762c2da0bf8ab61b5fde47305d)

Provides a nice summary of what the commit is doing, along with a more detailed explanation of why the change is useful and a glimpse into how it achieves it. The "how" is mostly useful for larger commits, and shouldn't be necessary for smaller units.

#### Small change

<pre>
Make OpenJDK release-critical

Currently there are no tests that depend on the JDK. Since we don't want a release with a broken JDK, make it an explicit dependency of the "tested" jobs.
</pre>
[Source](https://github.com/NixOS/nixpkgs/commit/4c0e44c34c84cd4d4764e9b48206b463d2fbef5a)

Says "what" and "why", and not much else. The commit message is longer than the diff, and that's fine.

#### Unrelated but focused changes

These two commits were made within a few minutes of one another, but developer made two of them because the changes were unrelated. Splitting the changes makes it easier to review the change later, and when running a git blame against a file, you don't have to waste time sorting through unrelated changes.

<pre>
nix-collect-garbage: Handle ENOENT

Don't barf trying to read a link that just got deleted.

Fixes #575.
</pre>
[Source](https://github.com/NixOS/nix/commit/7c9d0a596961bbb74b5b2c29cc3d8f2608e45d83)

<pre>
Make printValue() interruptible

Fixes #572.
</pre>
[Source](https://github.com/NixOS/nix/commit/f39979c6d3e49b09aa82fea5e167d4253f63d71f)