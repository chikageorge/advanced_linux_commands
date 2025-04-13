# Advanced Linux Commands

## File Permissions and Access Rights

Understanding how to manage file permissions and ownership is crucial in Linux. This knowledge empowers you to control access to files and directories, ensuring the security and integrity of your system. Let’s explore some essential commands and concepts related to file permissions and ownership.

In Linux, managing file permissions and ownership is vital for controlling who can access, modify, or execute files and directories. Understanding these concepts allows you to maintain the security and integrity of your system. Let’s delve into the key commands and concepts related to file permissions and ownership.

# Numeric Representation of Permissions

In Linux, permissions are represented using numeric values. Each permission (no permission, read, write, and execute) is assigned a numeric value:

- **no permissions = 0**
- **read = 4**
- **write = 2**, and
- **execute = 1.**

These values are combined to represent the permissions for each user class.  
Lets consider a few examples.

# Permissions Represented by 7

- 4 (read) + 2 (write) + 1 (execute) = 7  
- Symbolic: rwx  
- Meaning: Read, write, and execute permissions are all granted.  
- Example Context: A script file that the owner needs to read, modify, and execute.  

# Permissions Represented by 5

- 4 (read) + 1 (execute) = 5  
- Symbolic: r-x  
- Meaning: Read and execute permissions are granted, but write permission is not.  
- Example Context: A shared library or a command tool that users can execute and read but not modify.  

# Permissions Represented by 6  

- 4 (read) + 2 (write) = 6  
- Symbolic: rw-  
- Meaning: Read and write permissions are granted, but execute permission is not.  
- Example Context: A document or a configuration file that the owner needs to read and modify but not execute.  

# Shorthand Representation of Permissions  

In addition to the numeric way of showing permissions, Linux also has a shorthand, or symbolic, method for representing file permissions.  

**Understanding User Classes from a Permissions Perspective**  

Before diving into shorthand permissions, it’s important to understand the concept of “user classes” in the context of Linux permissions. Think of user classes as categories of users that Linux recognizes when deciding who can do what with a file. There are three main classes:  

- **Owner**: The person who created the file. Often referred to as 'user'.  
- **Group**: A collection of users who share certain permissions for the file.  
- **Others**: Anyone else who has access to the computer but doesn't fall into the first two categories.  

**The Role of Hyphens (-) in Permission Representation**  

When discussing permissions, you might notice hyphens (-) being mentioned. In the context of Linux file permissions, a hyphen doesn't actually represent a user class. Instead, it's used in the symbolic representation of permissions to show the absence of a permission.  

Lets get a bit practical with examples. Get onto your Linux terminal and run `ls -latt`.  
``` bash
 ls -lator  
total 24  

-rw-r--r--  1 dare  staff  6332 Jan 29 22:44 README.md  
drwxr-xr-x  7 dare  staff  224 Feb  4 11:56 Hands-On-Projects  
drwxr-xr-x  8 dare  staff  256 Feb  4 11:56 .  
-rwxr-xr-x  1 dare  staff  133 Feb  4 11:56 sync_img_to_s3.sh  
drwxr-xr-x  37 dare  staff  1184 Feb  4 12:52 image  
drwxr-xr-x  8 dare  staff  256 Feb  4 13:52 Quizzes  
drwxr-xr-x  5 dare  staff  160 Feb  5 09:28 ...  
drwxr-xr-x  15 dare  staff  480 Feb  6 21:34 .git  
```
Let’s break it down to understand what each part means:  

- In the output above, you will notice that some of the first character can be a `-` or `d`: `d` means it’s a directory, `-` means it’s a file.  
- The next three characters (rwx) show the permissions for the owner. `r` stands for read, `w` for write, and `x` for execute.  
- If a permission is not granted, you’ll see a `-` in its place (e.g., `r-x` means read and execute permissions are granted, but write permission is not).  
- The hyphen separates owner, group, and others.  
- The following three characters after the owner’s permissions represent the group’s permissions, using the same `r`, `w`, and `x` notation.  
- The last three characters show the permissions for others.  

The order the user class is represented is as follows:  
- The first hyphen "-" is the user.  
- The second hyphen "-" is the group.  
- The third hyphen "-" is others.  

# File Permission Commands  

To manage file permissions and ownership, Linux provides several commands:  

## chmod command  

The `chmod` command allows you to modify file permissions. You can use both symbolic and numeric representations to assign permissions to the user, group, and others.  

Let’s see an example.  

Create an empty file using the `touch` command:  

```bash
touch script.sh
```
Check the permission of the file:
``` bash
ls -latr script.sh
-rw-r--r-- 1 dare staff @ Jul  6 23:38 script.sh
```
What do you think the permission of the above output represent?

Now let’s update the permission so that all the user classes will have execute permission:
``` bash
chmod +x script.sh
```
The above command uses the chmod command with the +x option to grant execute permission to the file script.sh. The +x option adds the execute permission to the existing permissions for all the user classes.

Now let’s check what the file permissions look like:
``` bash
ls -latt script.sh
-rwxr-xr-x 1 dare staff @ Jul  6 23:38 script.sh
```
The same command can be executed to achieve the same result using the numbers approach:
``` bash
chmod 755 script.sh
```
To add execute permissions for all (user, group, others), you would add 1 to each of the three categories, resulting in 755:

    (4+2+1) = 7 for the user (read, write, and execute),

    (4+1) = 5 for the group (read and execute),

    (4+1) = 5 for others (read and execute).

Let’s consider another example. Imagine the owner of a file is currently the only one with full permissions to note.txt.

To allow group members and others to read, write, and execute the file, change it to the -rwxrwxrwx permission type, whose numeric value is 777:
``` bash
chmod 777 note.txt
```
Check the output:
``` bash
ls -lair note.txt
-rwxrwxrwx 1 dare staff @ Jul  6 23:53 note.txt
```
Now, notice the dash ("-") in the first position represents the file type and not a user class. It indicates that the entry is a regular file.
chown command

The chown command allows you to change the ownership of files, directories, or symbolic links to a specified username or group.

Here’s the basic format:
``` bash
chown [option] owner[:group] file(s)
```
For example, let’s assume there is a user on the server called "john", a group on the server called "developers", and you want the owner of filename.txt changed from "dare" to "john", and to also ensure that any user in the developer group has ownership of the file as well:
``` bash
chown john:developer filename.txt
```
Check the output with ls -latt command on this file to then see the new changes.
Superuser Privileges

It is often necessary to become the superuser to perform important tasks in Linux, but as we know, we should not stay logged in as the superuser. In most Linux distributions, there is a command that can give you temporary access to the superuser’s privileges. This program is called sudo (short for super user) and can be used in those cases when you need to be the superuser for a small number of tasks. To use the superuser privileges, simply type sudo before the command you will be invoking.

To switch to the root user, simply run:
``` bash
sudo -i
```
You can type exit to leave the shell:
``` bash
ubuntu@ip-172-31-19-250:~$ sudo -i
root@ip-172-31-19-250:~# exit
logout
ubuntu@ip-172-31-19-250:~$
```
User Management on Linux

As a DevOps engineer, you are also going to be doing systems administration which involves managing different users on the servers. You should know how to create a new user, or group, modify their permissions, update password and such similar tasks.
Creating a User

To create a new user on Ubuntu Server, you can use the adduser command. Assuming the name of the user to be created is joe. Open the terminal and run the following command:
``` bash
sudo adduser johndoe
```
Running this command will prompt you to enter and confirm a password for the new user. You will also be asked to provide some additional information about the user, such as their full name and contact information. Once you provide the necessary details, the user account will be created, and a home directory will be automatically generated for the user.

The home directory represents a file system directory created in the name of the user, such as /home/johndoe. This is where each user created on the server will store their respective data.
``` bash
ubuntu@ip-172-31-19-250:~$ sudo adduser johndoe
Adding user 'johndoe' ...
Adding new group 'johndoe' (1001) ...
Adding new user 'johndoe' (1001) with group 'johndoe' ...
Creating home directory '/home/johndoe' ...
Copying files from '/etc/skel' ...
New password:
Retype new password:
passwd: password updated successfully
Changing the user information for johndoe
Enter the new value, or press ENTER for the default
    Full Name []: John Doe
    Room Number []: 1
    Work Phone []: +23412345678
    Home Phone []: +23412345678
    Other []: N
Is the information correct? [Y/n] y
ubuntu@ip-172-31-19-250:~$
```
Granting Administrative Privileges

By default, newly created user accounts do not have administrative privileges. To grant administrative access to a user, you can add the user to the sudo group. Users in the sudo group can run commands with administrative privileges. To add the johndoe user to the sudo group, run:
``` bash
sudo usermod -aG sudo johndoe
```
    usermod: This is a command that modifies user account properties.

    -aG: These are flags used with the usermod command.

        -a stands for "append" and is used to add the user to the specified group(s) without removing them from other groups they may already belong to.

        -G stands for "supplementary groups" and is followed by a comma-separated list of groups. It specifies the groups to which the user should be added or modified.

In the given command, -aG sudo is used to add the user johndoe to the sudo group.

The sudo group is typically associated with administrative or superuser privileges. By adding johndoe to the sudo group, the user gains the ability to execute commands with elevated privileges.

## Tasks for you:

    Log out and log back in as the newly created user.

    Navigate to the /home/johndoe directory to explore what has been created. Tip: Use the cd command.

Switching User Accounts

To start using the system as another user, you will need to use the su command to switch.

To switch to another user account, use the su command followed by the username. For example, to switch to the johndoe account, run:
``` bash
su johndoe
```
You will be prompted to enter the password for the user. Once authenticated, you will switch to the user’s environment.
Modifying User Accounts
Changing User Password

To change the password for a user, use the passwd command followed by the username. For example, to change the password for johndoe, run:
``` bash
sudo passwd johndoe
```
You will be prompted to enter and confirm the new password for the user.

## Tasks for you:

    Test the updated password by logging on to the server, using the newly updated password.

Creating a Group

To create a new group, use the groupadd command. For example, to create a group named developers, use:
``` bash
sudo groupadd developers
```
Adding Users to the Group

Use the usermod command to add users to the group. For instance, to add users "john" and "jane" to the "developers" group:
``` bash
sudo usermod -aG developers johndoe
```
The -aG options append the "developers" group to the users' existing group memberships.
Verifying Group Memberships

To confirm the group memberships for a specific user, use the id command. For example, to check the group memberships for the user "johndoe":
``` bash
id johndoe
```
This command displays information about the user "johndoe," including the groups they belong to, such as "developers."
``` bash
ubuntu@ip-172-31-19-250:~$ sudo groupadd developers  
ubuntu@ip-172-31-19-250:~$ sudo usermod -aG developers johndoe  
ubuntu@ip-172-31-19-250:~$ id johndoe  
uid=1001(johndoe) gid=1001(johndoe) groups=1001(johndoe),1002(developers)  
ubuntu@ip-172-31-19-250:~$
```
Deleting a User

To delete a user, run the command below:
``` bash
sudo userdel username
```
Ensuring Proper Group Permissions

Groups in Linux are often used to manage permissions for files and directories. Ensure that the relevant files or directories have the appropriate group ownership and permissions. For example, to grant the “developers” group ownership of a directory:
``` bash
sudo chown :developers /path/to/directory
```
And to grant read and write permissions to the group:
``` bash
sudo chmod g+rw /path/to/directory
```

## Side Hustle Task 3:

    Create a group on the server and name it devops.

    Create 5 users ["mary", "mohammed", "ravi", "tunji", "sofia"], and ensure each user belongs to the devops group.

    Create a folder for each user in the /home directory. For example, /home/mary.

    Ensure that the group ownership of each created folder belongs to "devops".