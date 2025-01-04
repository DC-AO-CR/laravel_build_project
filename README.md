# HOWTO: Build a Laravel project with Sail

This how-to is about building your web application with Laravel in a Laravel Sail container. This installation guide contains step-by-step instructions on how to install the necessary tools, configure your environment, and build a Laravel project from the ground up.

## Install the Windows Subsystem for Linux (WSL)

Laravel Sail works under Linux but you can only run Linux under Windows if you install the Windows Subsystem for Linux (WSL). This creates a Linux environment inside Windows, so that you can use tools that were previously only available for Linux.

[Install WSL](https://docs.microsoft.com/en-us/windows/wsl/install)

(1) Run the following command in Powershell as the administrator:

```
wsl --install
```

(2) Reboot your workstation to let all changes take effect.

## Install Windows Terminal

Install the Windows Terminal app from the Windows Store App.

You will need to access your Linux environment through a terminal. The Windows Terminal app is an improvement on the Windows Powershell app or the CMD terminal already installed in Windows. The Windows Terminal app has many features needed to make it easier to run a Linux environment inside Windows.

[Windows Terminal](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701?hl=nl-nl&gl=NL)

## Install Docker Desktop

The Docker Desktop app is necessary in order for Laravel Sail to work. This is because Laravel Sail is actually a Docker container in disguise.

[Install Docker Desktop on Windows](https://docs.docker.com/desktop/windows/install/)

(1) Download Docker Desktop and run the installer.

(2) During the installation, ensure that the 'Use WSL 2 instead of Hyper-V' option is selected on the Configuration page.

(3) Start Docker Desktop.

*NOTE*: You don't have to have an account or sign in with Docker Desktop to use Laravel Sail or use it for your daily development in general.

(4) Check if the checkbox 'Use the WSL 2 based engine' is marked under ``Settings->General``.

(5) *OPTIONAL*: You can mark the 'Start Docker Desktop when you log in' checkbox under ``Settings->General``. This isn't necessary but if you're developing a Laravel app for several weeks, then it could be convenient after each startup.

## Install a MySQL database client.

You use a database client to connect with the MySQL server running inside the Laravel Sail container. You manage the database of your app on that server inside the container in order to modify its tables and their data.

There are several applications for accessing the database:
* [MySQL workbench](https://www.mysql.com/products/workbench/)
* [TablePlus](https://tableplus.com/)
* [DataGrip](https://www.jetbrains.com/datagrip/)
* And dozens more. Pick one that you like.

Every database client has basically the same functionality, so try one or two out and see which one you like best.

## Build a Laravel project with Sail

You will need to build your own Laravel project for your web application. Your Laravel project will be inside the Laravel Sail container. You interact with the Sail container through the terminal in order to develop, build and run your project. This section explains how to build and run your project, but not to develop your project. The commands to develop your project are not present in this installation guide, but you can find them in the Laravel Sail documentation.

* [Laravel Docs: installation page](https://laravel.com/docs/9.x/installation)
* [Laravel Docs: Starter Kits](https://laravel.com/docs/9.x/installation)
* [Laravel Docs: Sail](https://laravel.com/docs/9.x/sail)

(1) Start Docker Desktop (if you haven't already). Laravel Sail is a Docker container. The Docker Desktop needs to run if you want to build a project with Laravel Sail.

(2) Start the Windows Terminal. Previously installed at [Install Windows Terminal](#install-windows-terminal).

(3) In the Windows Terminal app, open the tab 'Ubuntu 20.04.4 LTS' (or a higher version) under the top-left 'v' sign to start a session in the WSL environment.

This will open a console prompt in a separate Linux environment in which you will build the Laravel Sail container.

(4) Create your project directory.

You will need a directory in which the Sail container will be stored. Replace the '{NAME_OF_YOUR_APP}' in the following commands with the name of your project. For example 'JukeBox' or 'GameStore'.

```
cd ~ (This command will change the current directory to your home directory.)
mkdir {NAME_OF_YOUR_APP}
cd {NAME_OF_YOUR_APP}
```

(5) Set up the project.

Go inside your project directory if you aren't already in it:
```
cd {NAME_OF_YOUR_APP}
```

Download the Sail container in your project directory with the following command:

```
curl -s https://laravel.build/{NAME_OF_YOUR_APP} | bash
```

If 'curl' is not installed, then install it and try the above command again:

```
sudo apt install curl
```

(6) Start the project.

Start the Sail container in your project directory.

```
cd {NAME_OF_YOUR_APP}
./vendor/bin/sail up
```

You can also see the Laravel container running in the Docker Desktop App.

*NOTE*: You can execute commands in the Sail container by preceding each command with './vendor/bin/sail {COMMAND}'.

(7) Install the Breeze starters kit.

Laravel Breeze is a starters kit to quickly set up authentication features, such as registration and login. Your web application will probably be used by multiple users, so installing it right away is easier than to do it later.

While the Sail container is running, you can install Breeze with the following commands:

```
./vendor/bin/sail composer require laravel/breeze --dev
./vendor/bin/sail artisan breeze:install
./vendor/bin/sail npm install
./vendor/bin/sail npm run dev
```

*NOTE*: The 'composer' and 'artisan' commands play a big part in the development of your web application, so read about them in the Laravel Sail documentation.

(8) Build the database.

You will have to build the database after you have installed Breeze or else your web application will not run.

```
./vendor/bin/sail artisan migrate
```
Your Lavavel project is built and your web application is now running on localhost.

(9) Navigate to 'https://localhost' in your browser. 

## Create a Git repository for your Laravel project.

When you've build a fresh Laravel project, it's best to immediately create a Git repository as well. 

*NOTE*: Don't commit your .env in your repository! This would reveal your user credentials, your usernames and passwords of your web application. See [Laravel configuration: Environment File Security](https://laravel.com/docs/9.x/configuration#environment-file-security). If you push this to a public Github, then everyone can see your username and password of your webapp.

## Connect to the database

You can connect to the database with a database client. The database runs on a MySQL server in the Sail container.

You can immediately log in with the default username 'sail' and the default password 'password' when the Sail container is running. The user credentials in the .env file in your project should be changed to something else.

*NOTE*: Don't commit your .env in your repository! This would reveal your user credentials, your usernames and passwords of your web application. See [Laravel configuration: Environment File Security](https://laravel.com/docs/9.x/configuration#environment-file-security). If you push this to a public Github, then everyone can see your username and password of your webapp.

## Start coding

You develop your web application inside the Laravel Sail container. Your IDE (or text editor) cannot reach that directory inside the WSL environment, so you will have to enable remote development in your IDE.

[Visual Studio Code: Remote development in WSL](https://code.visualstudio.com/docs/remote/wsl-tutorial)
[PHPStorm: WSL](https://www.jetbrains.com/help/phpstorm/how-to-use-wsl-development-environment-in-product.html)

You need to connect to WSL to be able to access the project directory in your IDE. When you make any changes to the code, then these will automatically show up in your web application on 'https://localhost'.

And you're done.
