<!-- 
Sample

-   <b>  <code></code> </b>
    ```

    ```


 -->
<!-- ############################################################# -->
# Node JS ___installation:

## How to install Node.js via binary archive on Linux?
1. Unzip the binary archive to any directory you wanna install Node, I use /usr/local/lib/nodejs
```
 VERSION=v16.15.1
 DISTRO=linux-x64
 sudo mkdir -p /usr/local/lib/nodejs
 sudo tar -xJvf node-$VERSION-$DISTRO.tar.xz -C /usr/local/lib/nodejs
``` 
2. Set the environment variable ~/.profile, add below to the end
```
# Nodejs
VERSION=v16.15.1
DISTRO=linux-x64
export PATH=/usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin:$PATH
```
3. Refresh profile
```
. ~/.profile
```
4. Test installation using

<code>$ node -v</code>

<code>$ npm version</code>

<code>$ npx -v</code>

the normal output is:
```
➜  node -v
v10.15.1
➜  npm version
{ npm: '6.4.1',
 ares: '1.15.0',
 cldr: '33.1',
 http_parser: '2.8.0',
 icu: '62.1',
 modules: '64',
 napi: '3',
 nghttp2: '1.34.0',
 node: '10.15.1',
 openssl: '1.1.0j',
 tz: '2018e',
 unicode: '11.0',
 uv: '1.23.2',
 v8: '6.8.275.32-node.12',
 zlib: '1.2.11' }
➜  npx -v
6.4.1
```

<!-- ############################################################# -->
## Install Nodejs using server :-
-   <b>On linux :  <code>Debian</code> </b>
    -   <b>1. Keep the server Up to date</b>
        ```bash
        apt update -y

        apt upgrade -y
        ```  
    -   <b>2. Install build-essential </b>
        ```bash
        apt-get install build-essential
        ```
    -   <b>3. Install required package </b>
        ```bash
        apt install curl -y
        ```
    -   <b>4 . Install NodeJS and NPM</b><br>Add the latest stable release of NodeJS.
        ```bash
        curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash –
        ```
        -   Install NodeJS.
            ```bash
            sudo apt-get install nodejs
            ```

        -   Verify Installation :
            ```bash
            node -v
            npm -v
            ```

    -   <b>5. Initialize the <code>Next.js</code> project development environment</b><br>
    Now, we’ll itialize the Next.js project development environment with the npx CLI build tool.
        ```bash
        npx create-next-app project-name
        ```
        Note: Replace the project-name with you desired project name.

        Change to the project directory list the directory contents.

        ```bash
        cd project-name
        ```
        Start the development server.
        ```bash
        npm run dev
        ```
        After the console reports:
        ```bash
        ready – started server on http://localhost:3000
        ```
        Navigate to your server IP on port 3000 in a web browser.
        ```bash
        http://192.168.1.10:3000/
        ```

        Verify the web page says Welcome to Next.js!.

        To create an optimized production build of your project, close the development server with CTRL + C then run:
        ```bash
        npm run build && npm run start
        ```
<!-- ############################################################# -->

# Install Python 3.10 on debian :
-   <b>Web Site link:</b> https://computingforgeeks.com/
how-to-install-python-on-ubuntu-linux-system/
----------------------------------------------------------------------
-   <b>Install the required dependencies to build Python 3.10 from the source:</b>
    ```
    $ sudo apt update

    $ sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev wget libbz2-dev
    ```

-   <b>Download <code>Pyton3.10.* (.tgz)</code> file</b>
  
    
-   <b>1.Extract the <code>.tgz</code> file:</b>
    ```
    $ tar xzf Python-3.10.2.tgz 
    ```
-   <b>2.Change to the extracted directory with cd command, then prepare the Python source code for the compilation on your system:</b>
    ```
    $ cd Python-3.10.2 
    $ ./configure --enable-optimizations 
    ```
    ![](Images/python_install_configure.png)

-   <b>Now initiate the Python 3.10 build process as below.</b>
    ```
    make -j 4
    ```

-   <b>3.Finally, run the following command to complete the Python installation on the Debian system. The altinstall prevents the compiler to override default Python versions :</b>
    ```
    $ sudo make altinstall 
    ```
    
-   <b>Check Python Version</b>
    ```
    $ python3.10 -V 
    ```
-   <b>Install pip3</b>
    ```
    $ sudo apt install python3-pip
    ```
<!-- ############################################################# -->