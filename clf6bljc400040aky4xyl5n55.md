---
title: "Flask + Docker + VSCode: How to Simplify Debugging and Improve Your Development Experience"
datePublished: Mon Mar 13 2023 04:24:18 GMT+0000 (Coordinated Universal Time)
cuid: clf6bljc400040aky4xyl5n55
slug: flask-docker-vscode-how-to-simplify-debugging-and-improve-your-development-experience
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1678516372196/70a40226-161b-49d1-abb2-bfb979c3dbc9.jpeg
tags: python, debugging, flask, vscode-extensions

---

## **Introduction**

Debugging Flask Python applications can be challenging, and things get even more complicated when running Flask inside a Docker container. Fortunately, with the right setup, debugging Flask running inside Docker locally in VSCode can be made much easier. In this article, we'll guide you through the process of setting up your Flask Python application to run inside a Docker container and show you how to leverage VSCode's powerful debugging features to troubleshoot any issues that arise. With our step-by-step instructions, you'll be able to streamline your development workflow and solve even the most complex bugs in no time!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678516397356/59ed9009-eace-4825-8177-bb1c1e0784ca.gif align="center")

## **What is debugpy?**

Debugpy is a Python debugger that provides a simple way to debug Python code remotely. It can be used to debug Python code running inside a Docker container, as well as code running on a remote machine. Debugpy can be used with a variety of editors and IDEs, including VSCode.

## **Step 1: Create a Flask app**

Before we can start debugging, we need a Flask app to work with. Here is a simple Flask app that we will use as an example:

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run()
```

## **Step 2: Create a Dockerfile**

Next, we need to create a Dockerfile that will define the Docker image for our Flask app. Here is an example Dockerfile:

```bash
FROM --platform=linux/amd64 ubuntu:latest

COPY . /app

WORKDIR /app

RUN apt-get update \
  && apt-get install -y python3-pip python3-dev \
  && cd /usr/local/bin \
  && ln -s /usr/bin/python3 python \
  && pip3 install --upgrade pip

RUN pip install -r requirements.txt
EXPOSE 5000

RUN pip install debugpy

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1
ENV DEBIAN_FRONTEND=noninteractive
CMD ["python", "-m", "debugpy", "--listen", "0.0.0.0:5679", "app.py"]
```

In this Dockerfile, we start with an Ubuntu base image and copy the contents of our current directory to the `/app` directory inside the Docker image. We install the necessary dependencies, including debugpy, and expose port 5000. Finally, we start debugpy and set it to listen on port 5679.

## **Step 3: Create a docker-compose file**

We will use a docker-compose file to simplify the process of running our Docker container. Here is an example docker-compose file:

```yaml
version: '3'
services:
  api:
    container_name: api
    command: python -u app.py
    build:
      context: ./api
      dockerfile: Dockerfile
    restart: unless-stopped
    networks:
      - frontend
      - backend
    ports:
      - 12005:5000
      - 5679:5679 
    volumes:
      - ./api:/app
      - /etc/localtime:/etc/localtime
    entrypoint: [ "python", "-m", "debugpy", "--listen", "0.0.0.0:5679", "-m", "app",  "--wait-for-client", "--multiprocess", "-m", "flask", "run", "-h", "0.0.0.0", "-p", "5000" ]
networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge
```

In this docker-compose file, we define a `networks` section to specify the communication between containers. In this case, we define two networks: `frontend` and `backend`. The `frontend` network will allow communication between the `web` container and any other container in the `frontend` network, while the `backend` network will allow communication between the `web` container and any other container in the `backend` network.

Lastly, we define the `volumes` section to mount the application code directory (`./app`) on the host to the `/app` directory in the container. This allows us to make changes to the application code on the host and have those changes reflected in the container immediately without needing to rebuild the container.

Now that we have our `docker-compose.yml` file set up, we can use the following command to start our application:

```bash
docker-compose up
```

This will start the `api` container, along with any other containers specified in the `docker-compose.yml` file.

## **Debugging the Flask application with VSCode and Docker**

Now that we have our Flask application running inside a Docker container, let's see how we can debug it using VSCode.

The first thing we need to do is to add a `.vscode/launch.json` configuration file to our project. This file specifies the configuration for the debugger, such as the type of debugger, the port to connect to, and the source code location. Here is an example `launch.json` configuration file:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Remote Attach",
            "type": "python",
            "request": "attach",
            "port": 5679,
            "host": "0.0.0.0",
            "pathMappings": [
                {
                    "localRoot": "${workspaceFolder}/app",
                    "remoteRoot": "/app"
                }
            ]
        }
    ]
}
```

In this configuration file, we specify that we want to attach to a remote Python process on port 5679, which is the same port that we exposed in our `docker-compose.yml` file. We also specify the `pathMappings` property to map the local source code directory (`${workspaceFolder}/app`) to the `/app` directory in the container.

With our `launch.json` configuration file set up, we can now start the Flask application with the `--wait-for-client` option and the `debugpy` module. This will start the application in debug mode and wait for a debugger to connect before executing any code.

Now that we have our application running in debug mode, we can go back to VSCode and start the debugger by selecting the `Python: Remote Attach` configuration from the VSCode debugger dropdown menu and clicking the green "play" button.

This will attach the VSCode debugger to the running Flask application inside the Docker container, allowing us to set breakpoints

## Conclusion

In this article, we've walked through the process of debugging a Flask application running inside a Docker container in VSCode. By leveraging the power of debugpy, we're able to connect the VSCode debugger to our running container, set breakpoints, and step through our code as if it were running locally.

With the use of a Dockerfile, we've created a containerized environment with all the necessary dependencies to run our Flask application, including debugpy. We've also updated our docker-compose file to include debugpy as the entrypoint for our container and created a new port mapping for the debugger to connect to.

By following these steps, we can easily debug our Flask application running inside a Docker container in VSCode, making it easier to identify and resolve any issues that may arise during development.

If you'd like to try this out for yourself, you can check out our example repository at [repo](https://github.com/kartikpuri95/flask-vscode-debug)([https://github.com/kartikpuri95/flask-vscode-debug](https://github.com/kartikpuri95/flask-vscode-debug)) and see how we've implemented these techniques to debug a Flask application inside a Docker container using VSCode.