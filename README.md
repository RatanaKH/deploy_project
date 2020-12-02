# HOW TO DEPLOY PROJECT TO SERVER BY USING GIT

Hi everyone!


Here is step to deploy your project to server side. I hope that can help you to deploy your project to your server.



> **Here is some keyword that you need to understand**
> - cd    : (Change Directory) This command enables you to change the current directory.
> - mkdir : (Make Directory) This command enables you to create new directory.
> - touch : This command used to create a file without any content. 
> - vi    : (visual editor) The default editor that comes with the UNIX operating system.
> - pwd   : (Present Working Directory) This used to check path of directory.

### Step 1: Remote to your server by using SSH
- Open terminal and then run

``` 
ssh username@hostname 
```

> Confirm with your password.


### Step 2: Create directory for project
> After that we need to create project directory in directory ***public_html*** run command in terminal as below.

`` cd  public_html && mkdir project_name.com``


### Step 3: Create git repository
> We need to create git bare repository.

run command 
``` 
cd ~/git && mkdir project_name.git 
```

### Step 4: Initial git bare
> Go to your repository and Initial git bare.

run command 
``` 
cd uat.project_name.git &&  git init --bare --shared 
```


### Step 5: Creare file post-receive in directory ***hooks***
> Go to ***hooks*** directory and create file `post-receive`

Run command 
```
cd hooks && touch post-receive
```


### Step 6: Edit file post-receive
> We need to add some script in here

Run command 
```
vi post-receive
``` 
> And then copy and past script as below. Press key `Insert` to edit text.

> After that edit path paramater TARGET and GIT_BARE.

```
#!/bin/bash

 TARGET="/home/ratana/public_html/project_name.com"
 GIT_BARE=""/home/ratana/git/project_name.git""
 BRANCH="master"

 while read oldrev newrev ref
 do
   # only checking out the master
   if [[ $ref = refs/heads/$BRANCH ]];
   then
     echo ""Ref $ref received. Deploy Live ${BRANCH} branch...""
     git --work-tree=$TARGET --git-dir=$GIT_BARE checkout -f
   else
     echo ""Ref $ref received. Doing nothing: only the ${BRANCH} branch may be deployed on this server.""
   fi
 done
```

> 1- Press key `Esc` to leave from edit text.

> 2- And after that press key `Shift + :` to save file.

> 3- Typing key: `wq` and `Enter` to exit and save file.


### Step 7: Go to local directory to add remote
> To add remote please run command

```
git remote add live ssh://ratana@hostname/~/git/project_name.git
```

> To push your code to server
```
git push live master
```

___


