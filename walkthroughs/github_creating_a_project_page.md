##Github
#Creating a Project Page

__Written by:__ [Nadav](https://github.com/nadavmatalon)
(May 2014@[Makers Academy](http://www.makersacademy.com/))


__Sources:__

* [Github Help: Creating Project Pages Manually](https://help.github.com/articles/creating-project-pages-manually)
* [StackOverflow: How to Fetch a Remote Branch with Git](http://stackoverflow.com/questions/9537392/git-fetch-remote-branch)


###General Notes

* This walkthrough describes the steps to deploy a personal project on [Github]()
using the [Github-io]() (aka [Github-Pages]()) server system.

* This is a great (and FREE!) way to present projects inloving static [HTML]() pages,
  [Javascript](), [jQuery](), [Ajax]() and other front-end only elements.
 
* Text in ALL_CAPITALS_AND_UNDERSCORES indicated a __placeholder__ for your own text 


###Disclaimer

Please note that the the following is meant only to offer guidance, advice, and helpful 
tips written by software development students as part of their learning process.  
As such, the author/s shall have responsibility whatsoever for any loss of data 
or any other damage caused to users and/or their code as a result of following the 
the steps presented in this walkthrough.


###Creating the Content for the New Project Page

The idea here is to keep things as simple a possible by creating a single, `HTML` file 
that will combine the content for the entire project, namely: HTML content, 
CSS styling and JavaScript code.

First, create a new file `PROJECT_NAME.html` and add the following content:

```html
<!DOCTYPE html>
<html>
	<head>
	    <title>APP_NAME</title>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<style>

		</style>
	</head>
	<body>

	</body>
	<script type="text/javascript">

	</script>
</html>
```


If you would like to use jQuery, add this `<script>` to the `<header>` section:

```
<script type="text/javascript" src='https://code.jquery.com/jquery-2.1.1.min.js'></script>
```


For 'jQuery.ui' add this `<script>` to the `<head>`:

```
<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jqueryui/1.11.1/jquery-ui.min.js"></script>
```


Next, fill-in the `<style>` tag with the __CSS__ syling, the `<body>` tag with the 
__HTML__ content, and the `<script>` tag with your __Javascript__ code. 


###Checking the Combined Project Page

To check that everything works, open the project page in the browser:

```bash
$> open ./PROJECT_NAME.html
```


###Clonning the Remote Repo

Make sure your remote repo is up-to-date with the local one, and then clone the remote repo 
to a new location in your file system (this is a safety measure to make sure the original 
local repo remains untouched for now):

```bash
$> git clone REMOTE_REPO_GITHUB_ADDRESS
```


Change directory into the newly cloned repo's folder:

```bash
$> cd REPO_NAME
```


###Creating a New Branch

Next, create a new branch using these specific flags:

```bash
$> git checkout --orphan gh-pages
=>Switched to a new branch 'gh-pages'
```

Note that the new branch, `gh-pages`, will not show on the branches list when 
running 'git branch' at this point, but 'master will no longer be showing as 
the active branch (i.e. marked with * and colored green).


###Editing the New Branch

Remove everything from the new branch:

```bash
$> git rm -rf .
```


Create a new 'index.html' file:

```bash
$> echo "My GitHub Page" > index.html
```


If you want, create a new README.md file:

```bash
$> touch README.md
```


###Pushing the New Branch

Stage, commit and push the new branch and files to the remote on Github:

```bash
$> git add .
$> git commit -a -m "First pages commit"
$> git push origin gh-pages
```


Note that after the first push, it can take up to ten minutes before the new GitHub page 
is available.


###First Look at the Project Page

Check that the page is available at:

```
http://GITHUB_USERNAME.github.io/REPO_NAME
```


###Pulling the New Branch to the Original Local Repo

Go back to the original local repo and pull the new branch:

```bash
$> git fetch origin gh-pages:gh-pages
```


Switch to the new branch:

```bash
$> git checkout gh-pages
```


###Pushing the Project to the New Branch


Add the content of the combined `HTML`, `CSS` and `JavaScript` file for your your 
project into the `PROJECT_NAME.html` and update the README.md file.

Then add, commit and push to the remote `gh-pages` branch:

```bash
$> git add .
$> git commit -m "Second pages commit"
$> git push origin gh-pages
```

Check that the page has been updated with the project at:

```
http://GITHUB_USERNAME.github.io/REPO_NAME
```


###Switching back to the Master Branch

Switch back to the `master` branch:

```bash
$> git checkout master
```

