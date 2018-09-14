change-github-author-multiple

An update of github's script to allow you to update multiple repo's author emails at a time.

## Usage

Place `githubEmailChanger.sh` in any directory and edit the stated fields in the file.

When you have successfully updated the file with your specific user information simply run the script in GitBASH or any other shell capable terminal.

```
#!/bin/sh

REPOS="repo-name-1 repo-name-2 repo-name-3 repo-name-4"

for REPO in $REPOS
	do
		git clone --bare https://github.com/YOU_USERNAME/$REPO.git

		cd $REPO.git

		git filter-branch --env-filter '
		OLD_EMAIL="ENTER YOUR OLD EMAIL"
		CORRECT_NAME="ENTER YOUR CORRECT NAME"
		CORRECT_EMAIL="ENTER YOUR NEW EMAIL"
		if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
		then
			export GIT_COMMITTER_NAME="$CORRECT_NAME"
			export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
		fi
		if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
		then
			export GIT_AUTHOR_NAME="$CORRECT_NAME"
			export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
		fi
		' --tag-name-filter cat -- --branches --tags

		git push --force --tags origin 'refs/heads/*'

		cd ..

		rm -rf $REPO.git
done
```
