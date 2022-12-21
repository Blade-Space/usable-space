# Using git filter-branch

[source🤍](https://www.git-tower.com/learn/git/faq/change-author-name-email)

Another way is to use Git's "filter-branch" command. 
It allows you to batch-process a (potentially large) number of commits with a script.
You can run the below sample script in your repository 
(filling in real values for the old and new email and name):

```bash
$ git filter-branch --env-filter '
WRONG_EMAIL="wrong@example.com"
NEW_NAME="New Name Value"
NEW_EMAIL="correct@example.com"

if [ "$GIT_COMMITTER_EMAIL" = "$WRONG_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$NEW_NAME"
    export GIT_COMMITTER_EMAIL="$NEW_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$WRONG_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$NEW_NAME"
    export GIT_AUTHOR_EMAIL="$NEW_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```

The same warning applies to this method as to the others mentioned: you are rewriting history with this command, creating new commit objects along the way!

Preferably, you should only do this in repositories that haven't been published / shared, yet. In any other case you should use it with extreme care - and only if you're aware of the side effects!