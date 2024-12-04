# dollarsign_handling
Research into, how docker compose treats dollar signs.


## Templating

The .env-file states the environment variables that you can use, when the docker-compose file is evaluated.

The contents of the .env file are not available inside the running container.

If you want to use variables defined in your .env-file inside the container. You can refer them 
in the environments part of the the docker compose file.

## Example 

Refer to the following [docker compose file](./docker-compose.yml) and [environment file](./.env).

When running with `docker up`:

```sh
> docker compose up
[+] Running 1/1r Desktop   o View Config   w Enable Watch
 ✔ Container myapp-foo-1  Recreated                                                                                                                                                                                                   0.1s 
Attaching to foo-1


foo-1  | TO DAY NAME_WITH_ and NAME_FROM_ENV

foo-1  | ENV_NAME=ENV_NAMEX
foo-1  | NAME_WITH_DOLLAR_SIGN_FROM_ENV=NAME_WITH_$_SIGN_FROM_ENV
                                        
foo-1 exited with code 0
```

Only the environment variables mentioned in the `environments` part of the docker file are included (show running printenv). The variable NAME_FROM_ENV was only available when the docker compose file was evaluated.

Also, notice how the dollar signs are 'eaten'.

Docker `compose config` shows how the docker compose file is evaluated.

```sh
> docker compose config
name: myapp
services:
  foo:
    command:
      - sh
      - -c
      - echo TO DAY NAME_WITH_$$_SIGN_FROM_ENV and NAME_FROM_ENV && printenv
    environment:
      ENV_NAME: ENV_NAMEX
      NAME_WITH_DOLLAR_SIGN_FROM_ENV: NAME_WITH_$$_SIGN_FROM_ENV
    image: busybox
    networks:
      default: null
networks:
  default:
    name: myapp_default
```
## Try it out

If you want to try it out. The `.env.sample` file should be copied into the file `.env`.