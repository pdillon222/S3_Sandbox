# S3_Sandbox
#### A Dockerized Environment for AWS S3 Testing

#### -Getting Started-
- The `sourcery` file contains shell functions, allowing for simple interaction with the container image.
- Source the file to add the functions to your current shell:
```
$ source sourcery
```
- This adds the following container-specific functions to your current shell:
  - `build_s3_sandbox`: Build the container
  - `run_s3_sandbox`: Run the container in background mode
  - `stop_s3_sandbox`: Stop the running container
  - `remove_s3_sandbox`: Delete the current container image
  - `enter_s3_sandbox`: Enter the running container environment
  - `cmd_s3_sandbox`: Send shell command to container (stdout sent back to current shell)
- Thus - first steps are to build and run the container:
```
$ build_s3_sandbox
... lots of output ...

$ run_s3_sandbox
... hash ref to container image ...

$ enter_s3_sandbox
root@316857cf254c:/s3#
```
#### Container is running, what do we do with it?
- Having sourced the `sourcery` file in our current shell, we now have the command aliases:
  - `user_setup`: Add AWS keys to running container
  - `s3_sandbox`: This is equivalent to the AWS cli prefix `aws s3`, but will instead be run inside of the container.  The command takes any arguments available to the [aws s3 cli command](https://docs.aws.amazon.com/cli/latest/reference/s3/index.html)
- A quick example:
```
$ s3_sandbox ls
Unable to locate credentials. You can configure credentials by running "aws configure".
```
- Per execution and output above - we attempted to `ls` buckets available to the user.  
- However, we have yet to add our AWS keys, thus the AWS cli in the container is ignorant to the user, account, or permitted buckets the user has access to.
- Let's set up our AWS keys in the container:
```
$ user_setup
Enter AWS Access Key: AKIA..........
Enter AWS Secret Key: 2qCi.............
Enter default AWS region: us-west-2
```
- Now let's try our `s3_sandbox ls` command once again
```
$ s3_sandbox ls
2020-06-15 21:59:00 pdillon222-learning
2020-05-31 14:53:06 pdillon222-plume
2020-04-07 22:15:01 pdillon222-rhce
2020-10-24 23:18:32 pdillon222-tfstate
2020-06-15 21:59:00 pdillon222bucket
```
- We see that having properly configured the AWS user profile, we can now see their buckets
- The .aws directory, has been set up in `/root`:
```
$cmd_s3_sandbox ls -la /root
total 24
drwx------ 1 root root 4096 Jul 30 23:15 .
drwxr-xr-x 1 root root 4096 Jul 30 21:32 ..
drwxr-xr-x 2 root root 4096 Jul 30 23:15 .aws
-rw-r--r-- 1 root root 3106 Dec  5  2019 .bashrc
drwxr-xr-x 3 root root 4096 Jul 30 21:32 .cache
-rw-r--r-- 1 root root  161 Dec  5  2019 .profile
```
