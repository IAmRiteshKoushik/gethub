# A Few Important Questions to Ask :

## 1. What is a .git Server ?
GitHub, GitLab and BitBucket are just git servers. It is git that keeps track
    of our entire version history. They are just UI providers for Git and provide 
    cloud storage of the code. Now, in order to differentiate themselves in the 
    market they bring forth additional features like : Pull Requests, Issues, 
    Actions, Projects, Wiki, etc etc.

## 2. Creating our own .git Server on AWS
- Let's say you are working on your local machine with Git CLI installed. 
    The problem comes when there is another person who wants to operate on the 
    same code-base. 
- So, we need a centralized server inside which "git" is installed.

## 3. Developer SSH Setup
- Launch an EC2 instance in AWS
- Create a new user - "git"

```bash
sudo adduser git
# Add any other relavant details
```

- We shall store all repositories inside `/var/lib/git/repo-name-here.git`
- Follow the convention of using ".git" at the end of repo names inside server
- In-case you are unable to do so, allow the user permissions to create and 
    edit directories, files etc in  `/var/lib`

```bash
sudo chown -R git /var/lib/git
```

- Next we are creating a bare git repository. In a repository like this, git 
    clone does not work. This only houses the contents which were supposed to be 
    inside the `.git` directory 

```bash
git init --bare
```

## 4. Pushing and pulling to remote repository

- Now let us try to clone this repository that we have created.

```bash
git clone git@ip-address-here:/var/lib/git/repo-name-here.git
# You will encounter a permission denied error
# This happens because your local machine's git profile
# did not have access to the SSH rights of that repo.

```

- Read through the generate-SSH keys documentation offered by GitHub
- Generate the public key and private key for SSH-ing into the git server
- Create a file `~/.ssh/authorized_keys` and drop your public key inside the 
    server manually.
- Now, git clone should work.
- Try creating an `index.html` file, make changes and a couple of pushes
- Inside the git server, if you go into your `objects/` directory, you should
see 

## 5. .git WebUI
We are not building a UI (a GitHub clone). Instead what we can do is leverage 
gitweb

```bash
sudo apt install gitweb
git instaweb --httpd=webrick
# Run this inside your server
```
- Now, this will begin a service in `127.0.0.1:PORT_NUMBER` but if we try to 
    access this using `IP.ADD.OF.EC2:PORT_NUMBER` we would not be able to do 
    so because we have not configured inbound traffic rules and security groups
    on the server.
- Add `Custom TCP` security policies by allowing it for `PORT_NUMBER` and open
    it to Ipv4 and Ipv6 traffic.
- Better to restart your machine once or wait for 5 minutes.
- Now if you try to hit `IP.ADD.OF.EC2:PORT_NUMBER`, then you would be able to 
    view `gitweb` (do not forget to launch it)

## 6. How does GitHub store files ?

- Now, let us try to reverse engineer GitHub a little bit. A github clone 
URL looks something like the following when done through SSH

```bash
git@github.com:IAmRiteshKoushik/lox-interpreter.git
```
- If we observe the structure of this URL then we can notice that `git` is the 
    username used on a server whose IP has been configured to use the domain 
    name `github.com`.
- Instead of `/var/lib/git/repo-name`, GitHub has created a directory for 
    every user with their username as the directory name
- And all GitHub repositories stay within that repository

## 7. How does Git store files ?

`objects/`    : This creates each commit-id stored as a encyprted file which 
                basically contains the details shown during `git log`. Overall,
                details of a commit. It also contains `blobs` (files), and 
                `trees`(directories). All of these are collectively termed as 
`info/`       : Contains files that are to be excluded based on patterns
`refs/`       : Contains pointers to commit objects
`description` : This is what decides the description of the repsitory which 
                GitHub displays in the repository overview section along 
                with the tags.
`config/`     : 
`branches/`   : This folder contains a list of all of our branches and their
                relavant details.
