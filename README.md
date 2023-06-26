# Hangil IOT Backend
## Ubuntu WSL, Docker, MySql Database Container, Extra: Tmux console

[![N|Solid](https://static.wixstatic.com/media/f9f09b_0e2f9f82431f4ae694b39f6002acd35b~mv2.png/v1/crop/x_0,y_0,w_322,h_287/fill/w_47,h_42,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/f9f09b_0e2f9f82431f4ae694b39f6002acd35b~mv2.png)](https://www.hangilhc.com/)

New Version of Hangilo IOT server, set up to work on linux system or Windows with Ubuntu WSL (do not require Windows), make use of MySql as Database, a backend web service as Rest API on Js6 and Web frontend APP that uses React Js with Next Js and Material UI.

In this documentation would be seen how to set up the basic elements for running the system from the ubuntu console, start a db in a docker container, to actually run all system in parallel independently.

- [Ubuntu, Docker and DB ](https://github.com/Filip9322/HGIOT_Main)
- [Backend](https://github.com/Filip9322/hgIOTBack)
- [Frontend](https://github.com/Filip9322/front-ReactHG-IOT)

### CONTENTS

- [Ubuntu, Ubuntu WSL](#Ubuntu)
- [Docker](#docker)
- [MySql Container](#mysqlContainer)
- [Nodejs](#Nodejs)
- [Tmux](#tmux)

---
## Ubuntu
The most reliable and popular solutions for web servers are based in Linux and Ubuntu is the most recgnized distribution of Linux, This operative Sytem compared to windows does not require from a visual interface for the user instead all actions could be dictated under commands into the terminal, process that not only reduce the amount of steps to running or installing a program, but as well complex process could be automatize more easy.
Working with the terminal is a required skill that any programmer would be facing and would increase its set of skills, but in case the developer do not want to loose its current windows O.S it is possible to start to virtualize a machine Linux in Windows and both enviroments.

#### Ubuntu WSL
To start Using Ubuntu Wsl, In windows is needed to open the app 'Windows Store' and search for Ubuntu. There are gonna be 2 options and any of them are okay to use.
Click Install and restart computer once installation finish.

You could follow the next tutorial for further details.
[HERE-< Install Ubuntu WSL in Windows 11](https://pureinfotech.com/install-wsl-windows-11/#install_wsl_app_msstore)

- ##### Solving further Issues for Ubuntu WSl with docker 

Before continue to the next step is recommended to follow the next steps to set up correctly the enviroment of Ubuntu Wsl with docker

[HERE <- Tutorial in Korean](https://teang1995.tistory.com/19)


## Docker
- ##### Install Docker

Both operating systems required this step and for every O.S is different steps. For windows follow the next link and Download the installer from the official site.
[Docker Website](https://docs.docker.com/desktop/install/windows-install/)

Once installed open an Ubuntu Wsl terminal window and type `docker ps` to confirm the installatio was successfull.
``` sh
docker ps
```` 
And finally open Docker App while in the terminal window you type 
```sh
docker network create back_end
```
`back_end` is gonna be a private network between containers that would allow to connect each other.

## MySql Container
- ##### Calling the official container of MySql publish in docker Hub

After successfully installing docker and being able to move by terminal into folders and triggers actions with commands, download this repository and go into the root of local repository.
```sh
git clone git@github.com:Filip9322/HGIOT_Main.git
```
You could check all automization steps written in the `docker-compose.yml` file for details of the steps, but mostely already having the code, you could jump into the next step.

```sh
version: '3,3'

services:
  db:
    image: mysql:5.7
    restart: always
    environment: 
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      # So you dont have to use root, but you can if you like
      MYSQL_USER: ${MYSQL_USER_DATABASE}
      # You can user password you want
      MYSQL_PASSWORD: ${MYSQL_USER_PASSWORD}
      # Password for root access
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
    ports:
      # <Port exposed> : < MySQL port running inside container>
      - ${DB_PORT}:3306
    expose:
      # Opens port 3306 on the container
      - '3306'
      # Where our data will be persisted
    command: --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
    volumes:
      - my-db:/var/lib/mysql
    networks:
      - back_end
    env_file:
      - .env
# Names our volumes
volumes:
  my-db:
networks:
  back_end:
    external: true
    driver: bridge
```

- ##### Starting the MySql Docker Container

With the following command, the container automatically would look for the container published in docker hub, bring it into local, install dependencies, start the service in the port set-up in the file, it would create the database with user and password written into the file, and launch to run and perfectly find a Mysql Db Service in thye port :3306
``` sh
docker-compose up
```
To stop the container you could do it with `Ctrl + c` and once stopped with the following command the container could be taken down 
``` sh
docker-compose down
```
---
## NodeJs
In order to run the BackEnd and FrontEnd service oour Ubuntu System should have installed node and `npm`.
That could be possible following the next Tutorial
[HERE <- Install Node and npm into Ubuntu WSL](https://medium.com/@lucaskay/install-node-and-npm-using-nvm-in-mac-or-linux-ubuntu-f0c85153e173)
---
## Tmux
In Linux is possible to install Tmux that is a multi console, one thread terminal service that allow to open different terminals, to see the as small windoes where I choose its didtribution .
```sh
sudo apt-get update
sudo apt-get install tmux
```
Once installed the command 
```sh
tmux
```
Starts the process and with `Ctrl + B , % or " to split vertically or horizontally, I usually make a proportional distribuition of 3 consoles vertically, that I accomplish doing
```sh
Ctrl + b, %
Ctrl + b, %
Ctrl + b, Alt + 2
```

---

## License

MIT


