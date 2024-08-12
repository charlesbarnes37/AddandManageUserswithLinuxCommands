<h1>ADD AND MANAGE USERS WITH LINUX COMMANDS</h1>

<h2>Responsible use of sudo</h2>

Authentication is the process of a user verifying their identity, and authorization is the process of determining what they have access to.  You can use the <b>sudo</b> command to temporarily run commands with elevated privileges to complete authentication and authorization management tasks.  Specifically, <b>useradd</b>, <b>userdel</b>, <b>usermod</b>, and <b>chown</b> can be used to manage users and file ownership.
<br />
<br />
To manage authorization and authentication, you need to be a root user, or a user with elevated privileges to modify the system.  The root user can also be called the “super user.”  You become a root user by logging in as the root user.  However, running commands as the root user is not recommended in Linux because it can create security risks if malicious actors compromise that account.  It’s also easy to make irreversible mistakes, and the system can’t track who ran a command.  For these reasons, rather than logging in as the root user, it’s recommended you use <b>sudo</b> in Linux when you need elevated privileges.
<br />
<br />
The <b>sudo</b> command temporarily grants elevated permissions to specific users.  The name of this command comes from “super user do.”  Users must be given access in a configuration file to use <b>sudo</b>.  This file is called the “sudoers file.”  Although using <b>sudo</b> is preferable to logging in as the root user, it's important to be aware that users with the elevated permissions to use <b>sudo</b> might be more at risk in the event of an attack.
<br />
<br />
You can compare this to a hotel with a master key.  The master key can be used to access any room in the hotel.  There are some workers at the hotel who need this key to perform their work.  For example, to clean all the rooms, the janitor would scan their ID badge and then use this master key.  However, if someone outside the hotel’s network gained access to the janitor’s ID badge and master key, they could access any room in the hotel.  In this example, the janitor with the master key represents a user using <b>sudo</b> for elevated privileges.  Because of the dangers of <b>sudo</b>, only users who really need to use it should have these permissions.
<br />
<br />
Additionally, even if you need access to <b>sudo</b>, you should be careful about using it with only the commands you need and nothing more.  Running commands with <b>sudo</b> allows users to bypass the typical security controls that are in place to prevent elevated access to an attacker.
<br />
<br />
<h2>Authentication and Authorization with sudo</h2>
You can use sudo with many authentication and authorization management tasks.  As a reminder, authentication is the process of verifying who someone is, and authorization is the concept of granting access to specific resources in a system.  Some of the key commands used for these tasks include the following:<br />
<br />
<br />
<b>useradd</b>
<br />
The <b>useradd</b> command adds a user to the system.  To add a user with the username of <b>fgarcia</b> with <b>sudo</b>, enter <b>sudo useradd fgarcia</b>.  There are additional options you can use with <b>useradd</b>:
<br />
<br />
<b>-g</b>: Sets the user’s default group, also called their primary group<br />
<b>-G</b>: Adds the user to additional groups, also called supplemental or secondary groups<br />
<br />
<br />
To use the <b>-g</b> option, the primary group must be specified after <b>-g</b>.  For example, entering <b>sudo useradd -g security fgarcia</b> adds <b>fgarcia</b> as a new user and assigns their primary group to be security.
<br />
<br />
To use the <b>-G </b>option, the supplemental group must be passed into the command after <b>-G</b>.  You can add more than one supplemental group at a time with the <b>-G</b> option.  Entering <b>sudo useradd -G finance,admin fgarcia</b> adds <b>fgarcia</b> as a new user and adds them to the existing <b>finance</b> and <b>admin</b> groups.<br />
<br />
<br />
<b>usermod</b>
<br />
The <b>usermod</b> command modifies existing user accounts.  The same <b>-g</b> and <b>-G</b> options from the <b>useradd</b> command can be used with <b>usermod</b> if a user already exists.
<br />
<br />
To change the primary group of an existing user, you need the <b>-g</b> option.  For example, entering <b>sudo usermod -g executive fgarcia</b> would change <b>fgarcia</b>’s primary group to the <b>executive</b> group.
<br />
<br />
To add a supplemental group for an existing user, you need the <b>-G</b> option.  You also need a <b>-a</b> option, which appends the user to an existing group and is only used with the <b>-G</b> option.  For example, entering <b>sudo usermod -a -G marketing fgarcia</b> would add the existing <b>fgarcia</b> user to the supplemental <b>marketing</b> group.
<br />
<br />
<b>Note:</b> When changing the supplemental group of an existing user, if you don't include the <b>-a</b> option, <b>-G</b> will replace any existing supplemental groups with the groups specified after <b>usermod</b>.  Using <b>-a</b> with <b>-G</b> ensures that the new groups are added but existing groups are not replaced.
<br />
<br />
There are other options you can use with <b>usermod</b> to specify how you want to modify the user, including:<br />
<br />
<b>-d</b>: Changes the user’s home directory<br />
<b>-l</b>: Changes the user’s login name<br />
<b>-L</b>: Locks the account so the user can’t log in<br />
<br />
<br />
The option always goes after the <b>usermod</b> command.  For example, to change <b>fgarcia</b>’s home directory to <b>/home/garcia_f</b>, enter <b>sudo usermod -d /home/garcia_f fgarcia</b>.  The option <b>-d</b> directly follows the command <b>usermod</b> before the other two needed arguments.<br />
<br />
<br />
<b>userdel</b>
<br />
The u<b>serdel</b> command deletes a user from the system.  For example, entering <b>sudo userdel fgarcia</b> deletes <b>fgarcia</b> as a user.  Be careful before you delete a user using this command.
<br />
<br />
The <b>userdel</b> command doesn’t delete the files in the user’s home directory unless you use the <b>-r</b> option.  Entering <b>sudo userdel -r fgarcia</b> would delete <b>fgarcia</b> as a user and delete all files in their home directory.  Before deleting any user files, you should ensure you have backups in case you need them later.
<br />
<br />
<b>Note:</b> Instead of deleting the user, you could consider deactivating their account with <b>usermod -L</b>.  This prevents the user from logging in while still giving you access to their account and associated permissions.  For example, if a user left an organization, this option would allow you to identify which files they have ownership over, so you could move this ownership to other users.<br />
<br />
<br />
<b>chown</b>
<br />
The <b>chown</b> command changes ownership of a file or directory.  You can use <b>chown</b> to change user or group ownership.  To change the user owner of the <b>access.txt</b> file to <b>fgarcia</b>, enter <b>sudo chown fgarcia access.txt</b>.  To change the group owner of <b>access.txt</b> to security</b>, enter <b>sudo chown :security access.txt</b>.  You must enter a colon (<b>:</b>) before <b>security</b> to designate it as a group name.
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
