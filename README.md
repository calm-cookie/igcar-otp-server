# Configuring T-OTP on IGCAR's CentOS 7.9

### User Guide

1. Log in to the server.
2. Install git.

    ```bash
    yum install git
    ```

3. Clone the repository.

    ```bash
    git clone https://github.com/calm-cookie/igcar-otp-server
    ```

4. Enter the cloned repository.

    ```bash
    cd ssh-otp
    ```

5. Install packages.

    ```bash
    yum -y install gcc automake autoconf libtool make openssl-devel
    ```

6. Build the executable.

    ```bash
    make
    ```

7. Get the current working directory.

    ```bash
    pwd
    ```

    Let us assume that the output is :

    ```bash
    /home/username/ssh-otp
    ```

    Note down the output of this command.

8. Go to the ssh authorized keys.

    ```bash
    nano ~/.ssh/authorized_key
    ```

    The file will look like this:

    ```bash
    ssh-rsa AAA...
    ```

    Change it to:

    ```bash
    command="<output-of-pwd-command>/ssh-otp <secret-key>" ssh-rsa AAA...
    ```

    In our example, `<output-of-pwd-command>` is `/home/username/ssh-otp` .

    `<secret-key>` is a Base32 encoded string of **length 16.** You can choose any secret key of your choice. **Remember to use only numbers from 2-7 and lowercase alphabets from a-z**. 
    
    Some valid secret keys are:

    ```2o5un37s5b23hgsh```

    ```234567qwertyasdf```

    ```qwertyasdf234567```

    ```27uh3gfh4j5h67jh```


    Generate your own unique secret key. Let us say we choose `h3k5h44hgdhj567l`.

    The authorized_keys file would be changed to -

    ```bash
    command="/home/username/ssh-otp/ssh-otp h3k5h44hgdhj567l" ssh-rsa AAA...
    ```

### **Warning**

1. There must be no spaces before and after `=` .

    This is the wrong way to write:

    ```bash
    command = "..."
    ```

    This is the right way to write:

    ```bash
    command="..."
    ```

2. The `secret-key` must be 16 characters long and use only 2-7 and **lowercase** a-z.

### For accessing the server using TOTP

1. Download the IGCAR TOTP mobile app.
2. Put the same `<secret-key>` that was used in `sshd_config` to generate your 7 digit TOTP.
3. `ssh` into the server and proceed.