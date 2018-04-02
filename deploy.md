# Configuration of the server to auto-deploy

## Install the dependancies

```
sudo apt update
sudo apt install hugo node-stylus lftp rsync
```

## Configure git hook

```.sh
#!/bin/bash

# Arguments
read oldrev newrev refname

echo "Old revision: $oldrev, New rev: $newrev, Refname: $refname"

# Working directory
WORK_DIR=/home/gitea/gitea-working/rebeccameier.ch/website
GIT_DIR=/home/gitea/gitea-repositories/rebeccameier.ch/website.git
GIT_CMD="git --work-tree=$WORK_DIR --git-dir=$GIT_DIR"

# Recuperate name of receiving branch
branchName=${refname:11} # Remove "refs/heads/"

# Detecting push to "TEST" or/and "RELEASE"
mustCompile=false
case "$branchName" in
"test")
	echo "Automatic deploy of the test branch."
	mustCompile=true
	distDir="/httpnext"
	baseURL="https://next.rebeccameier.ch"
	;;
"release")
	echo "The automatic deploy for the release branch is not implemented yet."
	mustCompile=false
	distDir="/httpdocs"
	baseURL="https://rebeccameier.ch"
	;;
esac

if [ "$mustCompile" = true ]; then
	# Creating working directory
	$GIT_CMD checkout -f $branchName

	# Compile Stylus stylesheet
        echo "Compiling Stylus stylesheet"
	/usr/bin/stylus --compress --out $WORK_DIR/static/css $WORK_DIR/stylus/rebeccameier.styl

	# Compiling website with Hugo
        echo "Compiling with Hugo"
	compileDir=$WORK_DIR/publish_$branchName
	/usr/bin/hugo --destination $compileDir --source $WORK_DIR --cleanDestinationDir --baseURL $baseURL

        # Detect files change (hash)
        echo "Detecting file change with rsync"
        diffDir=${compileDir}_diff
        /usr/bin/rsync --delete -crhv $compileDir/ $diffDir/

	# Mirror compiled website to [next.]rebeccameier.ch
        echo "Mirroring with lftp"
	/usr/bin/lftp ftp://git-deploy@alpinecoaster.easygiga.com -e "\
            set ftp:ssl-force on;\
            set ftp:ssl-protect-data on;\
            set ftp:use-feat off;\
            set ssl:check-hostname on;\
            set ssl:verify-certificate on;\
            set ssl:ca-file \"$WORK_DIR/easygiga.com.crt\";\
            debug 1;\
            mirror -e -R $diffDir $distDir;\
            quit"
fi
```

## Configure lftp credentials

Create file at `~/.netrc`:

```
machine alpinecoaster.easygiga.com 
	login your-username-here
	password your-password-here
```
