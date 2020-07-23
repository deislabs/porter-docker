# A Docker Mixin for Porter

This is a Docker mixin for Porter. The mixin provides the Docker CLI. 

## Mixin Declaration

To use this mixin in a bundle, declare it like so:

```yaml
mixins:
- docker
```

## Mixin Commands
The commands available are docker pull, push, build, run, remove, and login. 

## Mixin Syntax & Examples
The same syntax applies for install, upgrade, and uninstall.

### Docker pull

#### Syntax
You can specify either the tag or the digest.
```yaml
- docker:
    description: "Description of the command"
    pull:
      name: IMAGE_NAME
      tag: IMAGE_TAG
      digest: IMAGE_DIGEST
      arguments:
      - arg1
      - arg2
      flags:
        FLAGNAME: FLAGVALUE
        REPEATED_FLAG:
        - FLAGVALUE1
        - FLAGVALUE2
      outputs:
        - name: NAME
          hostname: HOSTNAME
```

#### Example
````yaml
- docker:
    description: "Install Whalesay"
    pull:
      name: docker/whalesay
      tag: latest
````

### Docker push

#### Syntax
```yaml
- docker:
    description: "Description of the command"
    push:
      name: IMAGE_NAME
      tag: IMAGE_TAG
      arguments:
      - arg1
      - arg2
      flags:
        FLAGNAME: FLAGVALUE
        REPEATED_FLAG:
        - FLAGVALUE1
        - FLAGVALUE2
      outputs:
        - name: NAME
          hostname: HOSTNAME
```

#### Example
````yaml
- docker:
    description: "Push image"
    push:
      name: gmadhok/cookies
      tag: v1.0
````

### Docker build

#### Syntax
```yaml
- docker:
    description: "Description of the command"
    build:
      tag: IMAGE_TAG
      file: Dockerfile #OPTIONAL
      path: PATH #defaults to "." OPTIONAL
      arguments:
      - arg1
      - arg2
      flags:
        FLAGNAME: FLAGVALUE
        REPEATED_FLAG:
        - FLAGVALUE1
        - FLAGVALUE2
      outputs:
        - name: NAME
          hostname: HOSTNAME
```

#### Example
````yaml
- docker:
    description: "Build image"
    build:
      tag: "gmadhok/cookies:v1.0"
      file: Dockerfile
````

### Docker run

#### Syntax
```yaml
- docker:
    description: "Description of the command"
    run:
      image: IMAGE
      name: NAME 
      detach: BOOL #defaults to false
      ports:
        - host: HOST_PORT
          container: CONTAINER_PORT
      env:
        variable: VALUE
      privileged: BOOL #defaults to false
      rm: BOOL #defaults to false
      command: COMMAND
      arguments:
      - arg1
      - arg2
      flags:
        FLAGNAME: FLAGVALUE
        REPEATED_FLAG:
        - FLAGVALUE1
        - FLAGVALUE2
      outputs:
        - name: NAME
          hostname: HOSTNAME
```

#### Example
```yaml
 - docker:
    description: "Run Whalesay"
    run:
      name: mixinpractice
      image: "docker/whalesay:latest"
      detach: true
      ports:
        - host: 8080
          container: 80
      env:
        myvar: "whales"
      privileged: true
      rm: true
      command: cowsay
      arguments:
         - "Hello World"
```

### Docker remove

#### Syntax
```yaml
- docker:
    description: "Description of the command"
    remove:
      container: CONTAINER_NAME
      force: BOOL #defaults to false
      arguments:
      - arg1
      - arg2
      flags:
        FLAGNAME: FLAGVALUE
        REPEATED_FLAG:
        - FLAGVALUE1
        - FLAGVALUE2
      outputs:
        - name: NAME
          hostname: HOSTNAME
```

#### Example
```yaml
- docker:
    description: "Remove mixinpractice"
    remove:
      container: mixinpractice
      force: true
```

### Docker login

#### Syntax
Username and password are optional because the mixin will default to using environment variables provided by DOCKER_USERNAME and DOCKER_PASSWORD from a parameter or a credential.
```yaml
- docker:
    description: "Description of the command"
    login: 
      username: USERNAME #OPTIONAL
      password: PASSWORD #OPTIONAL
      arguments:
      - arg1
      - arg2
      flags:
        FLAGNAME: FLAGVALUE
        REPEATED_FLAG:
        - FLAGVALUE1
        - FLAGVALUE2
      outputs:
        - name: NAME
          hostname: HOSTNAME
```

#### Example
```yaml
- docker:
    description: "Login to docker"
    login:
```

## Invocation

Use of this mixin requires opting-in to Docker host access via a Porter setting.  See the Porter [documentation](https://porter.sh/configuration/#allow-docker-host-access) for further details.

Here we opt-in via the CLI flag, `--allow-docker-host-access`:
```shell
$ porter install --allow-docker-host-access
```