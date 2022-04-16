<!-- 
Sample

-   <b>  <code></code> </b>
    ```

    ```


 -->

# Node JS ___installation:
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
