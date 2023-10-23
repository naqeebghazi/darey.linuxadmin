# darey.linuxadmin

# Navigating the Linux Terminal

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

![useradd_passwd](https://github.com/naqeebghazi/darey.linuxadmin/blob/main/images/useradd_passwd.png?raw=true)

  ### How do we disable root access completely?
  - Prevent a root user from connecting remotely.

