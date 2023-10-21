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
- A
