# dccp3 - CakePHP 3 Development Environment

This repository is to help teams to develop their CakePHP 3 projects in the same environment.

Only requirement is Docker (CE), if you don't have see [official installation](https://docs.docker.com/engine/installation/).

Heavily inspired from [docker-php7](https://github.com/shameerc/docker-php7).

## What is Included

The list below shows the programs that this repository will provide and corresponding version;

Program|Version
---|---
NGINX|1.13.5
PHP-FPM|7.0.23
MariaDB|10.3.1

Not so surprisingly though, CakePHP 3 is **not** included. You should get it by yourself. Although it is recommended to put CakePHP project files in a separate directory you can make a new directory under this repository after cloning.

## How to Use

1. Clone the repository and enter the directory;
    ```shell
    $ git clone git@github.com:ozanmuyes/dccp3.git
    $ cd dccp3
    ```
    NOTE: A little recommendation here; since this repository is to contain database files, it is good to separate each `dccp3` installment. For example if your CakePHP project's name (it's directory name) is `my_app` it is advised to rename the cloned `dccp3` directory as `dccp3-my_app`.

2. Change `/path/to/my_app` on `docker-compose.yml` to point your project's absolute path.
For example if your CakePHP 3 project is under this directory; `/home/john.doe/Code/my_beautiful_project` then the [related line](https://github.com/ozanmuyes/dccp3/blob/master/docker-compose.yml#L8) in the `docker-compose.yml` should read;
    ```yml
        - /home/john.doe/Code/my_beautiful_project:/var/app
    ```

3. Change database settings of your project as following;
    ```php
    // /path/to/my_app/config/app.php
    'Datasources' => [
            'default' => [
                // ...
                'host' => 'db', # On line 225-ish (was 'localhost')
                // ...
                'username' => 'root', # On line 232-ish (was 'my_app')
                // ...
            ]
        // ...
    ]
    ```

4. Be sure that 'logs' directory (i.e. `/path/to/my_app/logs/` directory) is empty (or at least there is no 'error.log' file exists). This prevents PHP errors that telling it cannot write to log files due to privileges, let the environment (this repository) create log files.

5. Start the environment;
    ```shell
    $ sudo docker-compose up
    ```

That's it. If this is the first time you ran this command be patient. After the installation you can visit [localhost](http://localhost:80) to see your application running.

To run `bin/cake` and Composer commands attach a shell to related `fpm` container and then run commands in this shell. For example if your clone directory is `/path/to/dccp3-my_app` then run this command;
```shell
$ sudo docker exec -it dccp3myapp_fpm_1 /bin/sh
```
And then, for example, run this command;
```shell
# bin/cake --help
```
