# darey.linuxadmin

# Navigating the Linux Terminal

The root directory is at the location:

    $ cd /

A user's home directory is at location:

    $ cd ~

The folders in the root folder:

/boot : stores data relating to booting up the system
/dev : location of device file - these act as an interface between the computer and the device driver (e.g. a USB drive)
/etc : configuration files of programs
/home : user specific files and folders

To see the structure of a directory, install the *tree* program:

    $ sudo apt install tree

Then pick a folder to show the structure of and invoke the tree command like this:

![image](https://github.com/naqeebghazi/darey.linuxadmin/assets/59777921/a59419ad-d664-4d06-ad7f-b1c157bca47a)

Type the following for more information about the tree program and how to use it more extensively:

    $ man tree

The ls *ls* command lists all the objects (folders, files etc) within a directory. Adding the -a and -l flags lists their modification/creation date, read/write permissions and ownership (i.e. root or other users). The forwardd slash '/' is for the root directory while the tilde sign '⁓' directs to the current user's home folder.

    $ ls -al

![root_with_ls](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/rootwith_ls.png?raw=true)

bin folder: stores all binary files and is linked to every users' binaries

![tree-L1](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/tree-L1.png?raw=true)

The commands that you use to interact with Linux are all stored as binaries in the bin folder. e.g. *ls* 
The location of binaries can be found using the which command:

![lsinUsr_bin](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/whichbinaries.png?raw=true)

A binary : Is a file that is not human readable. Stored in the /usr/bin folder. It is a compiled version of the program written by programmers


## SSH and remote machines

This is a secure way of connecting to a remote machine. It requires TWO keys (i.e. a pair), one public and one private. This is akin to a loack that everyone can see (public key) and only one person has the key required to open the lock (private key). 
The SSH key key informsation is held in the shh folder 

![ssh_home](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/ssh_home.png?raw=true)

To generate a new ssh key pair:

    $ ssh-keygen

Linux will suggest a default location or you can specify your own. 
Passwords are optionals, no passwords = Passwordless-SSH. Avoid passwords unless necessary.

![ssh-keygen](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/ssh-keygen.png?raw=true)

The keys will be stored in ~/.ssh folder as the following:

    ~/.ssh/id_rsa.pub
    ~/.ssh/id_rsa

One is a publc key (pub) and the other is the private key. The public key is much shorter in length thatn the private key which is many lines long. 
The public key stored on your local client will need to be copied over to your remote client. Before this is done, ensure the openssh server is running on the remote client. The openssh-server is required to listen for ssh connections on port 22. It will usually come installed for cloud VMs but in case it isnt, run the following command on the remote client:

    $ sudo apt install openssh-server -y

The .ssh directory in the remote client will look like this:

![images](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/sshonRemoteClient.png?raw=true)
    

When you have the client and the remote computers during an SSH connection, you must have these details learned:
- Send the public key from the local to the remote client. The contents of the public key are not a security concern.
- ssh-copy-id -i /home/.ssh/id_rsa.pub ec2user@32.54.33.13
- Now you will have the public key on the remote host and the private key on the local host
- ssh from your local to your remote host:

      $ ssh ec2user@32.54.33.13
  
You should now have connectivity.
As long as a client has the public key, you will be able to connect using the corresponding private key.

f you need to troubleshoot connectivity:
- add flag -v to ssh command. Level of verbosity depends on number of v (e.g. -vv, -vvv).
- to know what the remote client is receiving, enter this into the remote client terminal:

      $ sudo tail -f /var/log/auth.log

 This will now show a live feed log of authentications into thr remote client. Then go back to host client and in a new terminal type:

      $ ssh ec2user@32.54.33.13 -vvv

  The local host will give an out like this:

  ![verboseSSH](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/sshVerboselocalhost.png?raw=true)

  While the remote host will show similar to this in the auth.log file feed:

  ![sshTAILlog](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/sshTAILlog.png?raw=true)

  If the connection authentication wasnt accepted, the auth.log file will give details as to why. 


  ## SSHD Configuration files

  ### How do we enable/disable a user to use sudo?

  Create a user:

    $ sudo useradd -m user1      # creates user, -m flag creates a home directory for the user
    $ sudo passwd user1          # sets passwword for new user
    $ sudo su user1              # switch to log in as the new user1
    $ id                         # to confirm identity of current user

![useradd_passwd_su](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/sudoVIMuser.png?raw=true)

  New users do not have root permissions by default. You can alter these in the /etc/sudoers file:

    $ sudo vim /etc/sudoers

  Then add the user under the root user while copying all the roots uers privileges:

![sudoVIMuser](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/sudoVIMuser.png?raw=true)

To test that this config file is correct:

    $ sudo visudo -c

![visudo](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/visudo.png?raw=true)

if it reads "parsed ok" then your configuration is functional. 
You can test this by logging into the relevant user and tyoing the ls command. If a list appears, sudoes has been successful. If permission denied appears, you must prompt sudo before running the other command as user 1, like this:

![suSudo](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/suSudo1.png?raw=true)

To modify the sudo permissions for groups, open the /etc/sudoers file again and make the changes as follows by adding a new group:

![editSudoersdevopsgrp](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/editSudoersdevopsgrp.png?raw=true)

The add User1 to the new group and test the updated sudoers file:

![visudoGrpadd](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/visudoGrpadd.png?raw=true)

Then when you switch to user1, you will see the following as user1 is part of the group 'devops'
  
### How do control root user remote access
We need to modify the file:
    
    $ ls -al /etc/ssh/sshd_config
    $ sudo vim /etc/ssh/sshd_config

  Then search for (using / once in the vim editor) 'PermitRootLogin'.
  Then, in insert mode, remove the comment hashtag, thus enabling that line. Then either type 'no' or 'without-password'
    - no : no root login allowed to this server
    - without-password : root login only with private key

![sshd_withoutpassword](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/sshd_withoutpassword.png?raw=true)
  
Then test the file:

    $ sudo sshd -t

Nothing returned = Syntax OK

![sudosshd-t](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/sshd-t.png?raw=true)

Then check the status of the ssh server:

    $ systemctl status sshd

![systemctlSSHD](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/systemctlSSHD.png?raw=true)


## SFTP: Securely uploading/downloading files remotely

Using SFTP or SCP

### To Upload

On the remote client, do this:

    $ sudo mkdir -p /var/www/html

This will be the destination for files from other clients. You also need to know the hostname so the local host can send to the correct dest:

    $ hostname -i

![hostnameIP]()  

On the local client:

    $ sftp user1@ipaddress-of-remote

sftp essentially allows you to login to the remote server and execute many commands. 
Then once you have sftp'd into the remote server, navigate to the folder you created in the remote host:

    $ cd /var/www/html/directoryname
    $ sudo chown user1:admin /var/www/html/directoryname
    $ put -r directoryname

This will upload all the files as ong as the same directory name is present on the remote host. 

### To Download

Go to directory in remote host from local host:

    $ sftp user1@ipaddress-of-remote
    $ cd /var/www/html/directoryname
    $ get -r directoryname

### SCP

To upload:
scp cannot run commands like in sftp. Run this in host to upload to a remote host:

    $ scp -r directoryname/ user1@ipaddress:/home/user1/uploadfiles 

/tmp tells us where the contents of directoryname should be uploaded. 

Check this folder in remote client:

    $ ls -al /tmp/directoryname

To download:

    $ scp -r user1@ipaddress:/home/user1/remotefile ./downloadlocationinLocal

### Processes

To liist all process:

    $ ps

![ps-l](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/ps-l.png?raw=true)

Processes have states: R, S, D, Z. Only one process can run ata a time on a single CPU. SO any other process needs to wait. 

![processstates](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/processStates.png?raw=true)

To kill process, get pid and:

    $ sudo kill <process_id>

Processes stop on if logged out or system is shut down. To ensure p[rocess runs continually, run 'nohup':

    $ nohup firefox &

To bring the application to the foreground:

    $ fg %2

The %2 means job ide number which can be obtained by running 'jobs'

![nuhup](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/nuhup.png?raw=true)

Signals: to puase or cancel a process

64 signals, but only 5 commonly used: 1, 2, 3, 9, 15

![64signals](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/kill-l.png?raw=true)
  
    $ kill -1 <process_id>

Replace the number 1 with any other sig number as bewlo for that particular command:

sighup(1): If you want to stop a process and ensure it restarts with the same process id, use :
sigint(2): gets sent when you do control C (weakest sig kill)
sigquit(3): kills procvess and does a core dump memory to a file. occurs with control blackslash
sigkill(9): absolute kill, force kills a process 
sigterm(15): default when other flags arent specified (kill -)



