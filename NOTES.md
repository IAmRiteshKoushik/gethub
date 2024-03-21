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

## 4. Pushing and pulling to remote repository


## 5. .git WebUI


## 6. How does .git store files ?


