+++
title = "The Annotated Hello World Example"
description = "A first practical introduction into using Giant Swarm. This will take you to a simple Hello World example."
date = "2015-02-10"
type = "page"
weight = 10
categories = ["basic"]
+++

# The Annotated Hello World Example

<p class="lead">This should be your first step with Giant Swarm, a quick check that everything is working just fine.</p>

If you already followed the __Quick Start__ on the [documentation home page](/) without any trouble, you can skip this guide in favor of creating [your first real application](/guides/your-first-application/), unless you want some more explanation of what you just accomplished.

## What you need

In order to run applications on Giant Swarm, you need a Giant Swarm account. If you don't have one yet, [request an invite](https://giantswarm.io) first. Please note that we currently have a waiting list. It may take a while before you get an invite.

Besides that, you should be on a Linux (64 bit) or Mac OS machine and be a little bit comfortable with using the command line.

## Installing `swarm`

The Giant Swarm command line interface (CLI) is called `swarm`. For __Mac users__ it's recommended to install it via Homebrew using

```nohighlight
$ brew tap giantswarm/swarm
$ brew install swarm-client
```

__Linux users__ take the manual approach and download and unpack manually and move the binary to a place in their PATH, for example `/usr/local/bin`.

```nohighlight
$ curl -O http://downloads.giantswarm.io/swarm/clients/{{% cli_latest_version %}}/swarm-{{% cli_latest_version %}}-linux-amd64.tar.gz
$ tar xzf swarm-{{% cli_latest_version %}}-linux-amd64.tar.gz
$ sudo cp swarm /usr/local/bin/
```

Check our [installation reference page](/reference/installation/) for details.

## Logging in

Before you can do anything useful with `swarm`, you have to log in once. This is done by calling the `swarm login` command with your username as argument:

```nohighlight
$ swarm login yourusername
```

You will be prompted to enter your Giant Swarm password. Note that the password won't be displayed while you type.

## Getting the code

We provide a very simple [example application](https://github.com/giantswarm/helloworld) for you on GitHub. To checkout the source via git, do

```nohighlight
$ git clone https://github.com/giantswarm/helloworld.git
$ cd helloworld
```

Alternatively you can [download a ZIP file](https://github.com/giantswarm/helloworld/archive/master.zip) file, so you don't even need `git` installed.

When looking at the contents, you'll find there is a file called `swarm.json`, which is really all we need for now.

## Checking the application config

In case you want to understand what your application is doing, let's have a look at the `swarm.json` file.

```json
{
    "app_name": "helloworld",
    "services": [
        {
            "service_name": "helloworld-service",
            "components": [
                {
                    "component_name": "helloworld-component",
                    "image": "giantswarm/helloworld",
                    "ports": [8080],
                    "domains": {"$domain": 8080}
                }
            ]
        }
    ]
}
``` 

The file configures an application called `helloworld` with a single service. This service contains a single component named `helloworld-component`. This component uses a [custom helloworld image](https://registry.hub.docker.com/u/giantswarm/helloworld/) from the public Docker registry. The image is a simple HTTP server written in Go returning a HTML file. If you are interested in the implementation details have a look at the source code on our [Github page](https://github.com/giantswarm/helloworld). The `ports` definition exposes port 8080 of the component, which is finally exposed via the `domains` entry via standard HTTP port 80.

There is a nifty thing: We use a variable `$domain` in this file, so that you can pick your own domain name for your application without even having to edit that file.


## Running the application

We create and start the application in one step, using the `swarm up` command. See how we now set the `$domain` variable with a custom domain name using your swarm username.

```nohighlight
$ swarm up --var=domain=hello-$(swarm user).gigantic.io
```

Now your application is created on Giant Swarm. The `python:3` image you are using in your component is loaded from the Docker registry and started as a container on some machine of our cluster. Access to the application via your configured domain name is set up.

Once everything is done, you get a success notice on the terminal.

```nohighlight
Creating 'helloworld' in the 'yourusername/dev' environment...
Application created successfully!
Starting application helloworld...
Application helloworld is up.
You can see all services and components using this command:

    swarm status helloworld

```

The app should be running, so let's check it in a browser. Note that the URL contains your Giant Swarm username and looks like `hello-yourusername.gigantic.io`. If everything went fine, you see something like this:

```nohighlight
Hello Giant Swarm. \o/
```

Cool. Take a breath and enjoy your achievement, before it's time to clean up.

The following two commands will stop and delete the application to free resources on the cluster.

```nohighlight
$ swarm stop helloworld
$ swarm delete helloworld
```

Congratulations! You are now prepared for greater achievements.

## Next steps

Get real! Check out [Your First Application - in your language](/guides/your-first-application/) and learn to create your first real Giant Swarm application.
