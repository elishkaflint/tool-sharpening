## File Permissions

`ls -l` on a file or directory, you’ll get
`-rw-rw-rw-  1 eflint  staff  0 21 Jun 11:10 file`

- 1st space - if blank it’s a file, if d it’s a directory
- Next 3 - owner permissions
- Next 3 - group permissions
- Next 3 - other permissions

Then shows user and group ownership for the file or directory

r = read, w = write, x = execute

So if a user is trying to access a file, we need to know which category they fall into to understand what permissions they have

Local user information is stored in the /etc/passwd file, less /etc/passwd to see:
- User name
- Encrypted password (x means that the password is stored in the /etc/shadow file)
- User ID number (UID)
- User’s group ID number (GID)
- Full name of the user (GECOS)
- User home directory
- Login shell (defaults to /bin/bash)

Use grep to get info for a specific user >> `$ grep "root" /etc/passwd`

Group information is stored in /etc/group file, `$less /etc/group` to see groups

Use groups command to see all the groups available on a local system. Use groups [username] to see the groups a user belongs to

`$ groups eflint` outputs:
```
staff everyone localaccounts _appserverusr admin _appserveradm _lpadmin com.apple.sharepoint.group.1 _appstore _lpoperator _developer _analyticsusers com.apple.access_ftp com.apple.access_screensharing com.apple.access_ssh
```

Update file or directory ownership using chown<br>
`$ chown [user]:[group] [filename/directoryname]`<br>
(use -R if directory, group is optional)<br>

Update permissions using chmod<br>
`$ chmod [permissions change] [filename/directoryname]`<br>
(use -R if directory)<br>

Have to use local root to update permissions, eg. in Dockerfile:
```
USER root
RUN chmod……
USER new_user
```

File permissions can be changed locally and pushed to a remote repository, but only by removing the file and re-adding it to the repo (copy it to home, run chmod, then copy it back in). BB does actually show the difference at the top of the file.  
