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

