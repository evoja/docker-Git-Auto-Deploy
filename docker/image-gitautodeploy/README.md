# Image "git-auto-deploy"
Indifferent base image, just OS and GitAutoDeploy.
Consumers should add configs, keys, users etc.

People prefer different ways of configuring:
volumes, ONBUILD instructions, secrets, configs, evironment vairables.
This image doesn't have any proposals to let everyone do what they want.

Possible usage can look like a child image with following Dockerfile:

    FROM docker.pkg.github.com/evoja/docker-git-auto-deploy/git-auto-deploy:dv0.17

    ENV GAD_USER=gad GAD_UID=1001
    RUN \
        mkdir -p /home/$GAD_USER && \
        adduser -s /bin/sh -D -u $GAD_UID $GAD_USER && \
        mkdir /home/$GAD_USER/.ssh && chmod 500 /home/$GAD_USER/.ssh && \
        touch /home/$GAD_USER/.ssh/known_hosts && \
        chmod 600 /home/$GAD_USER/.ssh/known_hosts && \
        chown -R $GAD_USER:$GAD_USER /home/$GAD_USER && \
        chown -R $GAD_USER:$GAD_USER /app

    USER $GAD_USER
    COPY config.json .

    CMD ["--ssh-keyscan"]

Also you can look at the [example](../examples/gitautodeploy-base-usage)
