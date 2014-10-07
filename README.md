sshall
======

Run commands on remote hosts, return status codes and allow stdout/stderr parsing

# Examples:

## Basic usage
    $ HOSTS="user@hostonline user@hostoffline" sshall true
    sshall: user@hostonline status 0
    sshall: user@hostoffline stderr Permission denied (publickey,password).
    sshall: user@hostoffline status 255
    

## More complex example

Send commands from stdin -> then call "bash" to remove server banner
add -t -t to force pseudo tty allocation and be able to parse command output
optionally quote EOF to disable substitutions 

    $ HOSTS=user@host bin/sshall -t -t bash << 'EOF'
    $ pwd
    $ echo $HOSTNAME
    $ exit
    $ EOF
    sshall: user@host stdout pwd
    sshall: user@host stdout echo $HOSTNAME
    sshall: user@host stdout date
    sshall: user@host stdout user@host:~$ pwd
    sshall: user@host stdout /home/user
    sshall: user@host stdout user@host:~$ echo $HOSTNAME
    sshall: user@host stdout host
    sshall: user@host stdout user@host:~$ exit
    sshall: user@host stdout exit
    sshall: user@host stderr Connection to host closed.
    sshall: user@host status 0
