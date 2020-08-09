## Unix and Linux System Administration Handbook Chapter3

### Standard Unix Access Control
1. Standard unix access control model 
    1. Access control decisions depend on which user performing an operation, or user's membership
    2. Objects have owners
    3. You own the objects you create
    4. Root can act as the owner of any object
    5. Only root can perform certai sensitive admin ops
2. Locations in system
    * Goups are defind in the **/etc/group** file
    * UID -> /etc/passwd 
    * GID -> /etc/group
    * root UID = 0
3. Process ownership 
    * When the kernel runs an executable file that has its “setuid” or “setgid” permis- sion bits set, it changes the effective UID or GID of the resulting process to the UID or GID of the file containing the program image rather than the UID and GID of the user that ran the command. The user’s privileges are thus promoted for the execution of that specific command only.

### Root
4. Root account management 
    * Disadvantage: 
        1. leaves no record of what operations were performed as root 
        2. several people logged in, not sure who's actually performing the work
    * **su**: su - username (switched to login mode and log to another user)
    * **bash**: ~/.bash_profile in login mode; ~/.bashrc on nonlogin mode
    * **sudo**: limited su, only people in /etc/sudoers are authorised to use sudo; obeys last matching line if multiple rule exists for same user
    * **passwd -l** locks an account

### Extensions to the standard access control model 
5. Drawbacks to the standard model
    * Root account is a potential single point of failure
    * Security on a network is not represented in the standard model
    * The standard model has little or no support for logging 
6. Extensions:
    * **PAM**: Pluggable Authetication Modules: An autheicatoin technology, not an access control technology
    * **Kerberos**: network cryptographic authentication. A trusted third party(server) to authenticate the entire network. 
        (So require internet access; and machine login(minimum function) + network authentication?)
    * (PAM:framework, Kerberos:method)
    * **ACL**: Access control lists
    * **Linux capabilities**: divide the power of root into ~30 permissions
    * **Linux namespaces**: Processes are segragated into hierarchical partitions; the kernel simply denies the existence of objects(system's files, ports, processes) that are not visible from inside a given box(partitions)
7. Modern Access Control
    * Security-enhanced Linux(SELinux): pluggable and loadable
    * Mandatory access control (MAC): the override power
    * AppArmor, RBAC(role-based access control)
    
### Hand-on
1. ``find -perm -option`` : find file with certain permissions
       * -option = "666" (exact permission; "-666" (at least this permission); "/666" (either the owner, the group, or other should have permission to the file)
2. ``setuid`` : Giving a user the ability to run a setuid program can give that user the ability to run a program with root privileges. Normally you do not want an ordinary user running a program as a privileged user.
3. ``find / -user root -perm -4000`` : find files with setuid permissions (usally execute bit is s)
       e.g. /usr/bin/write
            /usr/bin/top
            /usr/bin/atq
            /usr/bin/crontab
            /usr/bin/atrm
            /usr/bin/newgrp
            /usr/bin/su
            /usr/bin/batch
4. ``touch -a -m -t [formatteddate] file.txt`` will change both the access time and the modification time of the file.txt to self-defined date