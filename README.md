# SSHPass

SSHpass offers you the ability to automatically offer a password via SSH when
you are prompted for it.

# Using (composer.json) 
```
"require": {
        "lpkapil/sshpass": "1.0.0"
    },
    "repositories": [
        {
            "type": "package",
            "package": {
                "name": "lpkapil/sshpass",
                "version": "1.0.0",
                "source": {
                    "url": "https://github.com/lpkapil/sshpass.git",
                    "type": "git",
                    "reference": "main"
                }
            }
        }
    ]
```
## Install

```bash
git clone https://github.com/lpkapil/sshpass.git
cd sshpass
./configure
make && make install
```

## Notes 

This repository is a mirror of https://sourceforge.net/projects/sshpass/

## Example 1: SSH

Use sshpass to log into a remote server by using SSH. Let's assume the password is!4u2tryhack. Below are several ways to use the sshpass options.

A. Use the -p (this is considered the least secure choice and shouldn't be used):

```bash
$ sshpass -p !4u2tryhack ssh username@host.example.com
```
The -p option looks like this when used in a shell script:

```bash
$ sshpass -p !4u2tryhack ssh -o StrictHostKeyChecking=no username@host.example.com
```
B. Use the -f option (the password should be the first line of the filename):

```bash
$ echo '!4u2tryhack' >pass_file
$ chmod 0400 pass_file
$ sshpass -f pass_file ssh username@host.example.com
```
The $ chmod 0400 pass_file is critical for ensuring the security of the password file. The default umask on RHEL is 033, which would permit world readability to the file.

Here is the -f option when used in shell script:

```bash
$ sshpass -f pass_file ssh -o StrictHostKeyChecking=no username@host.example.com
```
C. Use the -e option (the password should be the first line of the filename):

```bash
$ SSHPASS='!4u2tryhack' sshpass -e ssh username@host.example.com
```
The -e option when used in shell script looks like this:

```bash
$ SSHPASS='!4u2tryhack' sshpass -e ssh -o StrictHostKeyChecking=no username@host.example.com
```

## Example 2: Rsync

Use sshpass with rsync:

```bash
$ SSHPASS='!4u2tryhack' rsync --rsh="sshpass -e ssh -l username" /custom/ host.example.com:/opt/custom/
```

The above uses the -e option, which passes the password to the environment variable SSHPASS

We can use the -f switch like this:

```bash
$ rsync --rsh="sshpass -f pass_file ssh -l username" /custom/ host.example.com:/opt/custom/
```

## Example 3: Scp

Use sshpass with scp:

```bash
$ scp -r /var/www/html/example.com --rsh="sshpass -f pass_file ssh -l user" host.example.com:/var/www/html
```
