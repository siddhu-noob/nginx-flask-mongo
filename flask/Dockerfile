# syntax=docker/dockerfile:1.4 

## BUILDING THE BASE IMAGE STAGE 

## BUILD PLATFORM is the build argument, which is set to different architectures such as AMD64, ARM64, etc.. Docker will use this argument to build the image for the specified architecture.
FROM --platform=$BUILDPLATFORM python:3.11-alpine AS baseimage


## Setting the working directory to /app
WORKDIR /usr/src/app


## Copying the requirements.txt file to the working directory
COPY requirements.txt /usr/src/app/

## Docker inbuilt cache feature to store the pip cache in a specific location. This will help to speed up the build process by reusing the cached packages in subsequent builds.
## The --mount=type=cache,target=/root/.cache/pip option creates a cache mount at /root/.cache in the container, which is used by pip to store downloaded packages.
## installing the required packages using pip3. The -r option specifies that pip should install the packages listed in the requirements.txt file.
RUN --mount=type=cache,target=/root/.cache/pip \
pip3 install  -r requirements.txt

COPY . /usr/src/app/

CMD [ "python3","server.py" ]





## DEVELOPING THE ENVIRONMENT STAGE 

FROM baseimage AS dev

# multi line shell script inside the RUN command which updates the package index and installs git.
RUN <<EOF
    apk update 
    apk add git
    apk add --no-cache bash
EOF

# multi line shell script which created a new group as docker and a new user as vscode and added the user to the docker group.
RUN <<EOF
    addgroup -S docker
    adduser -S vscode -G docker
EOF



# copying the other image docker file to the current image
COPY --from=glourdocker/docker / /usr/src/app/


CMD [ "python3", "server.py" ]