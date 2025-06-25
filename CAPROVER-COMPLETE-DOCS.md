# CapRover Complete Documentation Reference

This is a comprehensive reference guide compiled from the official CapRover documentation at https://caprover.com/docs/

## Table of Contents

1. [Getting Started](#getting-started)
2. [CaptainDuckDuck Migration](#captainduckduck-migration)
3. [Captain Definition File](#captain-definition-file)
4. [Deployment Methods](#deployment-methods)
5. [App Configuration](#app-configuration)
6. [Persistent Apps](#persistent-apps)
7. [CLI Commands](#cli-commands)
8. [One-Click Apps](#one-click-apps)
9. [Complete Webapp Tutorial](#complete-webapp-tutorial)
10. [Resource Monitoring](#resource-monitoring)
11. [NGINX Configuration](#nginx-configuration)
12. [Service Update Override](#service-update-override)
13. [App Scaling & Cluster](#app-scaling-cluster)
14. [Pre-deploy Script](#pre-deploy-script)
15. [Play with CapRover](#play-with-caprover)
16. [Run Locally](#run-locally)
17. [Certbot Overrides](#certbot-overrides)
18. [Custom Themes](#custom-themes)
19. [Sample Apps](#sample-apps)
20. [Zero Downtime](#zero-downtime)
21. [Database Connection](#database-connection)
22. [Best Practices](#best-practices)
23. [Backup & Restore](#backup-restore)
24. [Static React App](#static-react-app)
25. [Stateless with Persistent Data](#stateless-with-persistent-data)
26. [Docker Compose](#docker-compose)
27. [CI/CD Integration](#ci-cd-integration)
28. [Server Purchase](#server-purchase)
29. [Disk Clean-Up](#disk-clean-up)
30. [Firewall & Port Forwarding](#firewall-port-forwarding)
31. [Troubleshooting](#troubleshooting)
32. [Support](#support)

---

## Getting Started

### Simple Setup

The recommended method to install CapRover is via DigitalOcean one-click app. CapRover is available as a One-Click app in DigitalOcean marketplace.

Note that if you are a new DigitalOcean user, you will receive **$100 Free Credit** once you sign up for the first two months. This is enough for two months of multiple servers!

If you use this method, you can skip **Prerequisites** section and step 1 of **CapRover Setup** below!

[![CreateDroplet](https://caprover.com/img/do-btn-blue.svg)](https://marketplace.digitalocean.com/apps/caprover?action=deploy&refcode=6410aa23d3f3)

### Prerequisites

#### A) Domain Name

During installation, you'll be asked to point a wildcard DNS entry to your CapRover IP Address. This will cost you as low as $2 a year (or [even less](https://www.reddit.com/r/selfhosted/comments/sp8etq/comment/hwdgztx/?utm_source=reddit&utm_medium=web2x&context=3)!)

Note that you can use CapRover without a domain too. But you won't be able to setup HTTPS.

#### B) Server

##### B1) Public IP

_Side note: You can [install CapRover locally](https://caprover.com/docs/run-locally.html) on your laptop on a private network which is behind NAT (your router). But if you want to enable HTTPS and/or access the apps from outside of your private network, it requires some special setup, like port forwarding._

In standard installation, CapRover has to be installed on a machine with a public IP address. If you need help with Public IP, see [Server & Public IP address](https://caprover.com/docs/server-purchase/digitalocean.html). This will cost you as low as $5 a month. If you use the DigitalOcean referral code, you'll get $100 credit - two months worth of free server: [https://m.do.co/c/6410aa23d3f3](https://m.do.co/c/6410aa23d3f3)

##### B2) Server Specs

_**CPU Architecture**:_ CapRover source code is compatible with any CPU architecture and the Docker build available on Docker Hub is built for AMD64 (X86), ARM64, and ARMV7 CPUs.

_**Recommended Stack**:_ CapRover is tested on Ubuntu 22.04 and Docker 19.03. If you're using CapRover on a different OS, you might want to look at [Docker Docs](https://docs.docker.com/engine/userguide/storagedriver/selectadriver/#supported-storage-drivers-per-linux-distribution).

_**Ubuntu 24.04**:_ This version [has been tested](https://github.com/caprover/caprover/issues/2244) by multiple people and there seems to be no known issues with this version.

_**Minimum RAM**:_ Note that the build process sometimes consumes too much RAM, and 512MB RAM might not be enough (see [this issue](https://github.com/caprover/caprover/issues/28)). Most providers offer a minimum of 1GB RAM on $5 instance including DigitalOcean, Vultr, Scaleway, Linode, SSD Nodes and etc.

##### B3) Docker

Your server must have Docker installed on it. If you get your server from DigitalOcean, you can select a server with CapRover one-click app and everything will be installed for you automatically. Otherwise, you can install Docker CE by following [this instruction](https://docs.docker.com/engine/installation). Note that your Docker version needs to be, at least, version 25.x+.

**AVOID snap installation** [snap installation of Docker is buggy](https://github.com/caprover/caprover/issues/501#issuecomment-554764942). Use the official installation instructions for Docker.

##### B4) Configure Firewall

Some server providers have strict firewall settings. To disable firewall on Ubuntu:

```bash
ufw allow 80,443,3000,996,7946,4789,2377/tcp; ufw allow 7946,4789,2377/udp;
```

See [firewall settings](https://caprover.com/docs/firewall.html) if you need more details.

### CapRover Setup

#### Step 1: CapRover Installation

Just run the following line, sit back and enjoy!

```bash
docker run -p 80:80 -p 443:443 -p 3000:3000 -e ACCEPTED_TERMS=true -v /var/run/docker.sock:/var/run/docker.sock -v /captain:/captain caprover/caprover
```

NOTE: do not change the port mappings. CapRover only works on the specified ports.

You will see a bunch of outputs on your screen. Once the CapRover is initialized, you can visit `http://[IP_OF_YOUR_SERVER]:3000` in your browser and login to CapRover using the default password `captain42`. You can change your password later. **However, do not make any changes in the dashboard**. We'll use the command line tool to setup the server (recommended).

#### Step 2: Connect Root Domain

Let's say you own `mydomain.com`. You can set `*.something.mydomain.com` as an `A-record` in your DNS settings to point to the IP address of the server where you installed CapRover. Note that it can take several hours for this change to take into effect. It will show up like this in your DNS configs:

- **TYPE**: A record
- **HOST**: `*.something`
- **POINTS TO**: (IP Address of your server)
- **TTL**: (doesn't really matter)

To confirm, go to [https://mxtoolbox.com/DNSLookup.aspx](https://mxtoolbox.com/DNSLookup.aspx) and enter `randomthing123.something.mydomain.com` and check if IP address resolves to the IP you set in your DNS. Note that `randomthing123` is needed because you set a wildcard entry in your DNS by setting `*.something` as your host, not `something`.

> **NOTE**: CapRover requires A Record to be pointing to CapRover's IP Address. If you use proxy services, such as Cloudflare, you may face difficulties. CapRover does not officially support such use cases.

#### Step 3: Configure and initialize CapRover

##### With CLI (recommended)

Assuming you have npm installed on your local machine (e.g., your laptop), simply run (add `sudo` if needed):

```bash
npm install -g caprover
```

Then, run

```bash
caprover serversetup
```

Follow the steps and login to your CapRover instance. When prompted to enter the root domain, enter `something.mydomain.com` assuming that you set `*.something.mydomain.com` to point to your IP address in step #2. Now you can access your CapRover from `captain.something.mydomain.com`. You can read more about hiding the root domain [here](https://caprover.com/docs/best-practices.html#hidden-root-domain).

> **NOTE**: **It will not be possible to carry through with the `caprover serversetup` if you've already forced https on your CapRover instance.**
> In such case go straight to logging in with the `caprover login` command. To change the password go to the settings menu in the app.

##### With the web interface (doesn't require npm)

1. Login to `http://[IP_OF_YOUR_SERVER]:3000`
2. Configure the root domain
3. Enable HTTPS, then force it
4. Once you are connected through HTTPS, change the default password ( `captain42`)

#### Step 4: (Optional) Set up Swap file

In some cases you may run into problems due to not having enough physical RAM.
For example, when building a Docker image, if it starts to take up too much memory, the build will fail.
To work around these problems (without purchasing more RAM) you can set up a Swap file (which is used as virtual RAM),
by following these instructions on [How To Create A Linux Swap File](https://linuxize.com/post/create-a-linux-swap-file/).

#### Step 5: Deploy the Test App

Go to the CapRover in your browser, from the left menu select Apps and create a new app. Name it `my-first-app`. Then, download any of the test apps [here](https://github.com/caprover/caprover/tree/master/captain-sample-apps), unzip the content. and while inside the directory of the test app, run:

```bash
/home/Desktop/captain-examples/captain-node$  caprover deploy
```

Follow the instructions, enter `my-first-app` when asked for app name. First time build takes about two minutes. After build is completed, visit `my-first-app.something.mydomain.com` where `something.mydomain.com` is your root domain.
CONGRATS! Your app is live!!

You can connect multiple custom domains (like `www.my-app.com`) to a single app and enable HTTPS and do much more in the app's settings page.

Note that when you run `caprover deploy`, the current git commit will be sent over to your server.

> **IMPORTANT**: Uncommitted files and files in `gitignore` WILL NOT be sent to the server.

You can visit CapRover in the browser and set custom parameters for your app such as environment variables, and do much more! For more details regarding deployment, please see [CLI docs](https://caprover.com/docs/cli-commands.html). For details on `captain-definition` file, see [Captain Definition File](https://caprover.com/docs/captain-definition-file.html).

---

## CaptainDuckDuck Migration

Note: This section is only intended if you want to upgrade your CaptainDuckDuck server to CapRover.

### Migration Script

Simply run [this script](https://raw.githubusercontent.com/caprover/caprover/master/dev-scripts/migrate-from-cdd.sh) to upgrade your CaptainDuckDuck server to CapRover. It automatically creates a backup of your config directory `/captain` in case something goes wrong.

To migrate, you can simply run the following lines:

```bash
wget https://raw.githubusercontent.com/caprover/caprover/master/dev-scripts/migrate-from-cdd.sh
chmod +x migrate-from-cdd.sh
./migrate-from-cdd.sh
```

### Tips for Migration:

Make sure you have enough disk space. CapRover image is around 400MB and the script automatically backs up the config directory.

#### Without Self-hosted Registry

You are most probably fine if you have around 1.5GB of free space on your server.

#### With Self-hosted Registry

Self-hosted Registry can consume many many GB of disk space. Since Migration Script automatically creates a backup for your config directory, you may have problems during upgrade.

To save space, if you had Self-hosted Registry enabled, you have two options:

- you can manually edit the migration script and remove the backup line ( `tar -cvf /captain-bk-$(date +%Y_%m_%d_%H_%M_%S).tar /captain`),
- or, you can remove all the content of registry by running `rm -rf /captain/registry/*` as it consumes a lot of disk space. Note that if you perform this action, you have to redeploy your apps in order for other nodes to be able to access it. If you only have one node, no extra action is needed.

### Breaking Changes from CaptainDuckDuck to CapRover:

- `schemaVersion` for captain-definition file is changed to `2`.
- If you previously had to edit the custom port to something other than 80 for your specific app, you no longer need to edit NGINX config, you can simply set the container port to any port from the UI.
- If you previously used a customized dockerfileLines, you have prefixed all `ADD` and `COPY` statements with `./src`. This is no longer needed with CapRover. For example, you previously had

```bash
COPY ./src/package.json /usr/app/
```

With CapRover you should change this to

```bash
COPY ./package.json /usr/app/
```

---

## Captain Definition File

### Basics

One of the key components of CapRover is the `captain-definition` file that sits at the root of your project. In case of NodeJS app, it sits next to package.json, or next to index.php in case of PHP, or requirements.txt for Python app. It's a simple JSON like this:

```json
{
  "schemaVersion": 2,
  "templateId": "node/8.7.0"
}
```

`schemaVersion` is always 2. And `templateId` is the piece which defines the foundation you need in order to run your app. It is in `LANGUAGE/VERSION` format. LANGUAGE can be one of these: `node`, `php`, `python-django`, `ruby-rack`. And VERSION is the version of the language you want to use - see [below](https://caprover.com/docs/captain-definition-file.html#versions-for-templateid).

Note that although the `templateId` can be one of the 4 most popular web app languages: NodeJS, PHP and Python/Django, Ruby/Rack, you are NOT LIMITED to these predefined languages! With CapRover, you have the ability to define your own Dockerfile. With a customized Dockerfile, you can deploy any laguage, Go, Java, .NET, you name it! Dockerfiles are quite easy to write. For example, the two captain-definition files below generate **the exact same result**.

#### Simple version

```json
{
  "schemaVersion": 2,
  "templateId": "node/8.7.0"
}
```

#### Advanced Version

```json
{
  "schemaVersion": 2,
  "dockerfileLines": [
    "FROM node:8.7.0-alpine",
    "RUN mkdir -p /usr/src/app",
    "WORKDIR /usr/src/app",
    "COPY ./package.json /usr/src/app/",
    "RUN npm install && npm cache clean --force",
    "COPY ./ /usr/src/app",
    "ENV NODE_ENV production",
    "ENV PORT 80",
    "EXPOSE 80",
    "CMD [ \"npm\", \"start\" ]"
  ]
}
```

### Use Dockerfile in captain-definition:

Note that the simple version of `captain-definition` with `templateId` is good as a starting point. But as your project gets more complex you may want to perform more complicated tasks with your base image, such as installing PHP extensions, installing `uWSGI`, installing particular version of `curl` and etc. In these cases, you can leverage the Dockerfile. Using custom Dockerfile allows you to build a very customized base image. If you're not familiar with Docker, you can simply use google to find something that's similar to your requirement and tweak it. Finally, if you're stuck, don't be shy to ask a question on our Slack channel, or StackOverflow.

To use a Dockerfile that's in your repository, you can simply reference it in the captain-definition file:

```json
{
  "schemaVersion": 2,
  "dockerfilePath": "./Dockerfile"
}
```

Dockerfiles are so simple and easy to read. Even if you don't know anything about Docker, you can get an idea what this does. Some examples of advanced methods: [PHP Composer](https://github.com/githubsaturn/captainduckduck/issues/94) and [Meteor](https://github.com/githubsaturn/meteor-captainduckduck/blob/master/captain-definition)

Using this approach (pure Dockerfile) you can deploy Ruby, Java, Scala, literally anything! If you need more details on dockerfile, please see [Dockerfile Help](https://docs.docker.com/engine/reference/builder) and [Best Practices](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices).

### Use Image Name

If you are an advanced Docker user, you may know that there are plenty of pre-built applications sitting on DockerHub. You can deploy these images using captain-definition. For example to deploy [https://hub.docker.com/r/nginxdemos/hello/](https://hub.docker.com/r/nginxdemos/hello/), you use:

```json
{
  "schemaVersion": 2,
  "imageName": "nginxdemos/hello"
}
```

Tip: You can simply copy and paste the captain-definition file above on CapRover web dashboard under the deploy tab.

### Monorepo:

You can use one git repo to deploy multiple different apps. For example, you may have a frontend and backend app in one repository. In this case, you can define multiple `captain-definition` files and have them deploying separate apps, for example, a directory structure, like this:

```
/project
   /frontend
      /src/index.js
      package.json
   /backend
      /src/index.js
      package.json
captain-definition-backend
captain-definition-frontend
```

With this content:
`captain-definition-backend`

```json
{
  "schemaVersion": 2,
  "dockerfileLines": [
    "FROM node:12-alpine",
    "RUN mkdir -p /usr/src/app",
    "COPY ./backend /usr/src/app",
    "RUN npm install && npm cache clean --force",
    "CMD [ \"npm\", \"start\" ]"
  ]
}
```

You can alternatively point to a Dockerfile. Note that the build context will be always the root of your project, so in the Dockerfile, you'll have to point to that specific directory, for example, `COPY ./backend /usr/src/app`

Next, you need to instruct your CapRover to use the correct `captain-definition` for each app. Navigate to your app, go to DEPLOYMENT tab, and edit your captain-definition path to `./captain-definition-backend`

### Versions for templateId:

NOTE: Versions get pulled from official repositories at runtime, therefore you do not need to update your Captain in order to use a new version of NodeJS. For example, see [here](https://hub.docker.com/_/node/).

**IMPORTANT:** Versions mentioned below are for **reference only**. For example, at the time that this document was generated Node 10 was not available, but it is available now. Therefore, you can use `node/10` or `node/10.15` or `node/10.15.0` as your templateId despite the fact that it is not mentioned below.

**Available Node versions:**
```
node/
carbon, 8, 8.9, 8.9.4, boron, 6, 6.12, 6.12.3, 9, 9.3, 9.3.0, 8.9.3, 9.2, 9.2.1, argon, 4, 4.8, 4.8.7, 6.12.2, 8.9.2, 6.12.1, 4.8.6, 6.12.0, 8.9.1, 9.2.0, 9.1, 9.1.0, 8.9.0, 9.0, 9.0.0, 4.8.5, 6.11, 6.11.5, 8.8, 8.8.1, 8.8.0, 8.7, 8.7.0, 6.11.4, 8.6, 8.6.0, 8.5, 8.5.0, 4.8.4, 6.11.3, 6.11.2, 7, 7.10, 7.10.1, 8.4, 8.4.0, 8.3, 8.3.0, 8.2, 8.2.1, 6.11.1, 8.2.0, 8.1, 8.1.4, 4.8.3, 6.11.0, 8.1.3, 8.1.2, 8.1.1, 8.1.0, 8.0, 8.0.0, 6.10, 6.10.3, 7.10.0, 4.8.2, 6.10.2, 7.9, 7.9.0, 7.8, 7.8.0, 4.8.1, 6.10.1, 7.7, 7.7.4, 4.8.0, 6.10.0, 7.7.3, 7.7.2, 7.7.1, 7.7.0, 7.6, 7.6.0, 4.7, 4.7.3, 6.9, 6.9.5, 7.5, 7.5.0, 4.7.2, 6.9.4, 7.4, 7.4.0, 4.7.1, 6.9.3, 7.3, 7.3.0, 6.9.2, 4.7.0, 7.2.1, 7.2, 4.6, 4.6.2, 7.2.0, 6.9.1, 7.1, 7.1.0
```

**Available PHP versions:**
```
php/
7, 7.2, 7.2.1, 7.0, 7.0.26, 7.1, 7.1.12, 5, 5.6, 5.6.32, 7.2.0, rc, 7.2-rc, 7.2.0RC6, 7.0.25, 7.1.11, 7.2.0RC5, 7.2.0RC4, 5.6.31, 7.0.24, 7.1.10, 7.2.0RC3, 7.1.9, 7.0.23, 7.2.0RC2, 7.2.0RC1, 7.0.22, 7.1.8, 7.2.0beta3, 7.2.0beta2, 7.1.7, 7.2.0beta1, 7.0.21, 7.2.0alpha3, 5.6.30, 7.0.20, 7.1.6, 7.1.5, 7.0.19, 7.0.18, 7.1.4, 7.0.17, 7.1.3, 7.0.16, 7.1.2, 7.1.1, 7.0.15, 5.6.29, 7.0.14, 7.1.0, 5.6.28, 7.0.13, 7.1-rc, 7.1.0RC6, 7.1.0RC5, 7.0.12, 5.6.27, 7.1.0RC4, 7.1.0RC3, 5.6.26, 7.0.11, 7.1.0RC2, 5.6.25, 7.0.10, 7.1.0RC1, 5.6.24, 7.0.9, 5.5.38, 5.5, 5.5.37, 5.6.23, 7.0.8, 5.5.36, 5.6.22, 7.0.7, 7.0.6, 5.6.21, 5.5.35, 7.0.5, 5.6.20, 5.5.34, 7.0.4, 5.6.19, 5.5.33, 7.0.3, 5.6.18, 5.5.32, 7.0.2, 5.6.17, 5.5.31, 7.0.1, 5.6.16, 5.5.30, 7.0.0, 5.4, 5.4.45, 7.0.0RC8, 5.6.15, 7.0.0RC7, 7.0.0RC6, 7.0.0RC5, 5.6.14, 7.0.0RC4, 7.0.0RC3, 5.6.13, 5.5.29, 7.0.0RC2, 7.0.0RC1, 7.0.0beta3, 5.6.12, 5.5.28, 5.4.44, 7.0.0beta2, 5.6.11, 5.5.27, 5.4.43, 7.0.0beta1, 5.5.21, 5.5.19, 5.5.16, 5.4.40, 5.4.41, 5.4.39, 5.5.17, 5.6.3, 5.6.0, 5.6.8, 5.6.4, 5.4.42, 5.5.20, 5.4.38, 5.5.22, 5.6.5, 5.6.2, 5.4.35, 5.4.36, 5.4.33, 5.3.29, 5.3, 5.5.26, 5.5.18, 5.4.32, 5.4.37, 5.6.1, 5.6.6, 5.6.9, 5.6.10, 5.4.34, 5.6.7, 5.5.24, 5.5.23, 5.5.25
```

**Available Python-Django versions:**
```
python-django/
2, 2.7, 2.7.14, 3, 3.6, 3.6.4, 3.6.3, rc, 3.7-rc, 3.7.0a3, 3.7.0a2, 3.7.0a1, 2.7.13, 3.6.2, 3.6-rc, 3.6.2rc2, 3.6.1, 3.6.2rc1
```

**Available Ruby-Rack versions:**
```
ruby-rack/
2.4, 2.4.3, 2, 2.5, 2.5.0, rc, 2.5-rc, 2.5.0-rc1, 2.4.2, 2.5.0-preview1
```

---

## Deployment Methods

Regardless of your deployment method, make sure that you have a 'captain-definition' file in your project. See docs on [Captain Definition](https://caprover.com/docs/captain-definition-file.html) for more details.

### Deploy via CLI

Simply run `caprover deploy` in your git repo and follow the steps. This is the best method as it's the only method that reports potential build failures to you. Read more about it here:
[Get Started - Step 5](https://caprover.com/docs/get-started.html#step-5-deploy-the-test-app).

### Deploy via Web Dashboard

Convert the content of your project into a tarball ( `.tar`), go to your Captain web dashboard and upload the tar file. This deployment method is typically used for testing purposes only.

For captain-definition files that do not require any source code, like [this](https://caprover.com/docs/captain-definition-file.html#use-image-name), you can simply copy and paste the captain-definition content on web dashboard.

![deployapp](https://caprover.com/img/docs/app-deploy.png)

### One Click Rollback

Let's say you deployed a new version of your app. But you realize that it's buggy. You don't have time to go back, revert your changes or fix the bug, what would you do? Simple! Just go to deployment tab and click on the revert icon next to the version that you want to revert to. CapRover automatically starts a new build and deploy that version! Note that this **DOES NOT** revert changes that you made to Environment Variables, and other app configs such as persistent directories and etc. It just reverts your image (deployed source code).

### Automatic Deploy using Github, Bitbucket and etc.

This method is perhaps the most convenient one. This method automatically triggers a build with a `captain-definiton` file when you push your repo to a specific branch (like `master` or `staging` or `release` or etc). To setup this, go to your apps settings and enter the repo information:

- repo: This is the main HTTPS address of repo, in case of github, it is in `github.com/someone/something` format. Make sure it does NOT include `https://` prefix and `.git` suffix.
- branch: The branch you want to be tracked, for example `master` or `staging` or `release`...
- github/bitbucket username(email address): This is username that will be used when Captain downloads the repo.
- github/bitbucket password: You can enter any non-empty text, like `123456`, for public projects.
- Or, instead of username/password, use SSH Key: Make sure to use PEM format as other formats may not work. Use the following command if unsure:

```bash
ssh-keygen -m PEM -t ed25519 -C "yourname@example.com" -f ./deploykey -q -N ""
```

After you enter this information, save your configuration. And go to your apps page again. Now, you'll see a new field call webhook. Simply copy this webhook to your github/bitbucket repo webhooks (see below). Captain listens to POST requests on this link and triggers a build.

#### Github

Create a webhook here:

- Project > Settings > Add Webhook > URL: Captain Webhook from your apps page, Content Type: `application/json`,
Secret: , Just the `push` event.
Furthermore add the contents of your generated public key to your repositories deploy keys.

#### Bitbucket

Webhooks can be added here:

- Project > Settings > Webhooks > Add Webhook > Title: Captain Server, URL: Captain Webhook from your apps page.

#### GitLab and Others

Webhooks can be added in a similar fashion. As long as the webhook fires a POST request, CapRover is able to pick it up and starts a build from the latest commit on the specified branch.

---

## App Configuration

### HTTP Settings

This is where all HTTP related stuff sits. If your app is not an HTTP app, you can simply check "Do not expose as web app". This is used for anything that is not a webapp, like a database such as MongoDB or MySQL.

![httpsettings](https://caprover.com/img/docs/app-http.png)

By default, any webapp that you deploy gets a Captain domain assigned to it in this format: `appname.root.domain.com`. However, you have the option to add as many domains as you want to this app. For example, you can add `www.myawesomeapp.com` and `myawesomeapp.com`.

There are also some advanced options such as Edit Default Nginx config and Container HTTP Port which you usually do not need to edit.

#### Enabling HTTPS

CapRover has built-in support for Let's Encrypt and it enables you to easily put your websites behind secure HTTPS without being concerned with the cost of SSL certificates (Let's Encrypt is free) and without any hassle of setting up configs and renewing certificates.

To enable HTTPS for any domain, just click on enable HTTPS! It takes a few seconds and it's done!

After enabling HTTPS, you can optionally, although very recommended, enforce HTTPS for all requests, i.e. denying plain insecure HTTP connections and redirect them to HTTPS.

### App Config

This is where you can set runtime configuration and settings.

![appconfig](https://caprover.com/img/docs/app-vars.png)

#### Environment Variables

One of the most basic configuration that you can set for your app is environment variables. These variables are usually used to pass in data that does not live in the code. Examples, include API key for a 3rd party service, database connection URI and etc.

You can simply set the environmental variables on the dashboard and use them dynamically in your code, e.g., `process.env.VAR_NAME_HERE` for NodeJS, or `$_ENV["VAR_NAME_HERE"]` in PHP.

If you'd like to access these variables during build time, you can use ARG command to your Dockerfile.

```dockerfile
FROM imagename....
ARG VAR_NAME_HERE=${VAR_NAME_HERE}
ENV VAR_NAME_HERE=${VAR_NAME_HERE}

## At this point, "VAR_NAME_HERE" is available as an env var during your build,
## you can do something like this:
## RUN echo $VAR_NAME_HERE
```

As well as the variables you set yourself, CapRover will also set a `CAPROVER_GIT_COMMIT_SHA` environment variable to the full git commit SHA that is being deployed. This is only available during the Docker build and is not available inside your app by default. If you want to use it inside your app then you can use something like the following:

```dockerfile
FROM imagename....
ARG CAPROVER_GIT_COMMIT_SHA=${CAPROVER_GIT_COMMIT_SHA}
ENV CAPROVER_GIT_COMMIT_SHA=${CAPROVER_GIT_COMMIT_SHA}
```

#### Port Mapping

CapRover allows you to map ports from a container to the host. You should use this feature if you want a specific port of your apps/containers to be publicly accessible. The most common use case is when you want to **connect to a database container from your local machine**.

Note that even if you don't set any port mapping, all ports are accessible from other containers on the same Captain cluster. Therefore, you should only use this option if you want the port to be publicly accessible. Make sure to have the port open, see [firewall settings](https://caprover.com/docs/firewall.html).

For example, if you want your NodeJS app to access your MongoDB database, and you do not need to access your MongoDB from your laptop, you don't need Port Mapping. Instead, you can use the fully qualified name for the MongoDB instance which is `srv-captain--mongodb-app-name` (replace `mongodb-app-name` with the app name you used).

#### Persistent Directories

Only used for [persistent apps](https://caprover.com/docs/persistent-apps.html).

#### Node ID

Only used for [persistent apps](https://caprover.com/docs/persistent-apps.html). Persistent apps need to be locked down to a particular node (if you have a cluster of servers). NodeId defines what node this app should be locked down to.

#### Service Tags

_available as of 1.11_

You can mark caprover services with special tags. This allows you to better group and view your apps in the table.

#### Instance Count

How many instances of this app should run at the same time. You may run as many instances as you'd like. However, you are limited by your hardware. If you increase this number and you don't have enough RAM or Disk space, your system may crash. It is advised to consider performance implications before increasing this number.

#### Predeploy Function

This is a [very dangerous and advanced option](https://caprover.com/docs/pre-deploy-script.html). Do not use it unless you really know what you are doing.

---

## Persistent Apps

### Persistent or Not

When you want to create an app you have the option of creating the app with "Persistent Data" or not. By default, you should always prefer no persistence. However, they are cases where you need to create an app with persistence. Also, if you have a massive amount of static data that you don't want to bundle with your repository and have it shipped to the server everytime you build, you can map a directory on the host to a directory inside the container and FTP to your server and move your files there. This is generally not needed unless the amount of static data that you need to send to your server is extremely large.

#### Persistent Apps:

These are the apps that have some data that need to survive restart, crash, container update and etc. Because these apps store data on disk, once they get created they get locked down on a specific server (if you have multiple servers). You can still change the constraint so that they will be moved to another machine, but if you do, they will lose anything that might have been stored on the current host. Examples that use persistent apps include:

- Any database that stores data on disk has to have persistent data enabled. Otherwise all data will be lost when the container restarts (due to crash, host restart and etc...)
- A photo upload app which does not use third party storages like Amazon S3 to store images. Instead, it locally stores uploaded images.
- A webapp that needs to store some user uploaded files and plugins locally on disk (like WordPress)

The main limitation of apps with Persistent Data is that they cannot be run as multiple instances. That's because they would access the same storage area and the data can be corrupted if multiple apps try to write on the same path.

Note that even for Persistent Apps, NOT ALL DIRECTORIES will be treated as persistent directories. After you created the app as an app with persistent data, you'll have to define directories that you want to be persistent in the app details page on web dashboard. You can let CapRover manage the stored directories for you (use labels), or use a specific path on the host (server).

##### Using label

In that case, they will be placed in `/var/lib/docker/volumes/YOUR_VOLUME_NAME/_data` on your server. The path inside the container is completely customizable. By default, the volume name will have `captain--` prepended to the field you enter (e.g. `my-volume` will become `captain--my-volume`)

##### Using specific path

For example, you can map `/var/usr` on your server to `/my-host-usr-something` in your container (app). This way you can save a file in your container at `/my-host-usr-something/myfile.txt` and the file will be available on your server (host) at `/var/usr/myfile.txt`. **Note** that, if you choose to use this option (specifying a specific host path), you'll have to make sure that the path already exists in your host before assigning it.

#### Removing Persistent Apps:

Persistent directories need to be manually removed after you remove an app from Captain dashboard. This is to avoid accidental deletion of important data. To delete persistent directories, depending on the type of persistent directories, steps are different:

- Volumes (persistent directories mapped to a label):
For this type, you need to run `docker volume ls` to see the names of the volumes, and then run `docker volume rm NAME_OF_VOLUME` to remove the volume
- Mapped directories on host: these are directories from your server that are mapped to a directory in your container (app). To remove them, simply remove the directory from your server via `rm -rf /path/to/directory`

#### Non-Persistent Apps:

Generally speaking, anything that does not directly store data on disk can be made non-persistent. You should always prefer to have non-persistent apps as they are much more flexible. Let's say you have multiple servers, if a server becomes unhealthly, all "non-persistent" apps on that server will automatically get moved to other servers whereas persistent apps are locked down to that server due to some data that they saved on that server.

Also, multiple instances of non-persistent apps can be running at the same time without causing any issues as they live in isolated environment and each of them has their very own disk space. Note that non persistent apps can still write data on disk, things on temporary cache and etc, but that data will get deleted once the container restarts due to a crash, deploy, configuration update or host restart. Examples include:

- A image processor which takes a photo and runs some logic to figure out what is in the picture. This is a good example of an app that can benefit from getting spawned as multiple instances as it's CPU heavy.
- A TODO web app. Note that this app will definitely uses some sort of database which is persistent. But the webapp itself does not store anything directly on disk.
- An image upload app that uses Amazon S3 as the storage engine rather than storing images locally on disk

---

## CLI Commands

You can use this CLI tool to deploy your apps. Before anything, install the CLI tool using npm:

```bash
npm install -g caprover
```

### Server Setup

The very first thing you need to do is to setup your Captain server. You can either do this by visiting `HTTP://IP_ADDRESS_OF_SERVER:3000` in your browser, or the recommended way which is the command line tool. Simple run

```bash
caprover serversetup
```

Follow the steps as instructed, enter IP address of server. Enter the root domain to be used with this Captain instance. If you don't know what Captain root domain is, please visit [www.caprover.com](http://www.caprover.com/) for documentation. This is a very crucial step. After that, you'll be asked to enter your email address. This should be a valid email address as it will be used in your SSL certificate. After HTTPS is enabled, you'll be asked to change your password. And... Your are done! Go to Deploy section below to read more about app deployment.

### Login

_If you've done the "Server Setup" process through the command line. You can skip "Login" step because "server setup" automatically logs you in as the last step of setup._

The very first thing you need to do is to login to your Captain server. It is recommended that at this point you have already set up your HTTPS. Login over insecure, plain HTTP is not recommended.

To log in to server, simply run the following line and answer the questions.

```bash
caprover login
```

If operation finishes successfully, you will be prompted with a success message.

NOTE: You can be logged in to several Captain servers at the same time. This is particularly useful if you have separate staging and production servers.

### Deploy

In order to deploy your application, you first need to create a captain-definition file and place it in the root of your project folder. In case of a nodejs application, this would sit in the same folder as your package.json.

A simple captain-definition file for a nodejs application is:

```json
{
  "schemaVersion": 2,
  "templateId": "node/8.7.0"
}
```

See [Captain Definition File](https://caprover.com/docs/captain-definition-file.html) for more details on the Captain Definition file.

After making sure that this file exists, run the following command and answer the questions given:

```bash
caprover deploy
```

You will then see your application being uploaded and, after that, your application getting built. Note that the build process can take several minutes, so please be patient!

To use the previously-entered values for the current directory, without being asked again, use the `-d` option:

```bash
caprover deploy -d
```

Alternatively, you can use the stateless mode and supply the CapRover server information inline:

```bash
caprover deploy -h https://captain.root.domain.com -p password -b branchName -a app-name
```

This can be useful if you want to integrate CI/CD pipeline.

#### Options:

Those params are available:

- `-d, --default`: Uses previously entered values for the current directory. Other options are not considered.
- `-c, --configFile <file>`: Specifies a configuration file to use for deployment settings.
- `-u, --caproverUrl <url>`: Sets the CapRover machine URL to which the deployment will be made. This URL is typically in the format \[http\[s\]://\]\[captain.\].your-captain-root.domain.
- `-p, --caproverPassword <password>`: The password for the CapRover machine. This option is prompted when a URL is provided and an app token is not used.
- `-n, --caproverName <name>`: The name of the CapRover machine you wish to deploy to. This can be selected from a list of logged-in machines.
- `-a, --caproverApp <app>`: Specifies the application name on the CapRover machine to which you are deploying. This is selected from a list of available applications on the machine.
- `-b, --branch <branch>`: Specifies the Git branch to be deployed. Note that uncommitted and git-ignored files will not be included.
- `-t, --tarFile <tarFile>`: Specifies the path to a tar file which must include a captain-definition file for deployment.
- `-i, --imageName <image>`: Specifies a Docker image to be deployed. The image must exist on the server or be accessible through public or private repositories that CapRover can access.
- `--appToken <token>`: An optional token for app-level authentication, if required.

### List logged in servers

To see a list of servers you are currently logged in to, run the following line:

```bash
caprover list
```

### Logout

Run the following command:

```bash
caprover logout
```

---

## One-Click Apps

CapRover has built-in support for several popular apps that can be deployed as is. These include WordPress, MySQL, MongoDB and many more.

There is a repository of [One Click Apps on GitHub](https://github.com/caprover/one-click-apps) and it's continuously growing.

#### Databases and Database GUI

- MongoDB
- MongoExpress
- MsSQL
- MySQL
- Redis
- PhpMyAdmin
- PostgreSQL
- Adminer
- Apache CouchDB
- Gitea
- ElasticSearch
- And many more...

#### Blogging and Content

- WordPress
- Ghost
- Prisma 1
- Strapi
- Minio
- And many more...

#### Dev Tools

- Jenkins
- Drone.io
- Hasura
- Nexus3
- Many more...

#### Other Apps

- Parse
- NextCloud
- Rainloop
- Thumbor
- OhMyForm
- And many more...

Thanks to [@8byr0](https://github.com/8byr0), we have a **community maintained** [directory of apps](https://wizardly-ptolemy-8fcac8.netlify.app/). You can view the source code [here](https://github.com/8byr0/caprover-sampleapps-browser).

### What about other apps?

Just because an app or database is not available as a one click app, it doesn't mean that you can't deploy it. All you need to do is to search for the Docker image of the app that you're looking for. For example, before NextCloud was available as a one click app, you could still deploy it manually like this

With CapRover v1, it's even easier than the method explained above. Since `captain-definition` now supports `imageName`. You can copy and past this into the deploy section of an app that you create. No more `tar` file creation is needed when all you need is `imageName`:

```json
{
  "schemaVersion": 2,
  "imageName": "nextcloud:12-rc"
}
```

All the environment variables that you can set are listed on their DockerHub page: [https://hub.docker.com/_/nextcloud/](https://hub.docker.com/_/nextcloud/)

### Configuration Settings

They all come with pre-configured settings, however, you'll be have the option to customize the settings. For example, MySQL database uses port 3306, but you can change this port to another port if it suits your needs.

It is important to mention that some of these configuration parameters, might show up as environment variables in your app settings after you deploy the app, however, their values only being used in the installing phase. i.e., changing password of MySQL through changing the PASSWORD environment variable will not work. Instead, you should use MySQL commands to change the password. The PASSWORD environment variable is being used to set up the original password during the installation phase.

### Upgrading One Click apps

So you deployed your one click app and sometime later a new version comes out and you want to update your app. The process is different for different apps:

#### Simple Image Update

Most good quality apps allow you to simply update the underlying image and that's it! This usually the case for most application. For example, if you have a MySQL 5.5 and you want to upgrade to 5.7, you can simply go to the "Deployment" tab, navigate to the bottom and under **Method 6: Deploy via ImageName** just type in mysql:5.7 and click deploy!

The image names are usually in `imagename:version` format or `account/image:version` format. You can look at the image that have been deployed by CapRover under the deployment history. Also you can look at the new versions at DockerHub. For example,

- `mysql` versions can be found from here: [https://hub.docker.com/_/mysql?tab=tags](https://hub.docker.com/_/mysql?tab=tags)
- `portainer/portainer` versions can be found from here: [https://hub.docker.com/r/portainer/portainer/tags](https://hub.docker.com/r/portainer/portainer/tags)

Note that there are other use cases where CapRover modifies the original image to provide more functionality. For example, redis container is modified to provide [authentication option](https://github.com/caprover/one-click-apps/blob/af172b6680583487bdeacf230d7abaf9b57f4811/public/v4/apps/redis.yml#L10-L12). In this case, it's easier to simply delete your app and recreate it. If your app has persistent data, make sure NOT TO REMOVE the volume when deleting the app and make sure to recreate the app with the exact same name so that the exact same volume will be attached to the app.

#### Other cases

Some apps have a different way of upgrading, specifically if they have persistent code data. WordPress is a good example. To upgrade WordPress, all you need to do is to perform upgrade from within the wordpress website panel itself. Sometimes on top of that, you need to upgrade the underlying image, in that case, just follow the guide above.

### Connecting to Databases

#### Connecting Within CapRover Cluster

Note that since all these applications are Docker containers, you can have multiple MySQL databases on running on port 3306 without having any conflict. If you want to connect to two different MySQL databases, from a PHP app, where both PHP and MySQLs are under the same instance of CapRover, you can use `srv-captain--mysqlappname1:3306` and `srv-captain--mysqlappname2:3306`.

#### Connecting Remotely

However, if you want to connect to your database from a remote machine (e.g. your laptop) you need to map a container port to a server port. In that case, you have to map two different ports on the server, for example:

- Port 1001 of the server goes to mysql-1 port 3306
- Port 1002 of the server goes to mysql-2 port 3306

Port mapping is needed if you want to connect to a database from a remote machine. You can read more about it [Captain Configuration - Port Mapping](https://caprover.com/docs/app-configuration.html#port-mapping).

After port mapping, you can enter these values for your Database Client:

- Host: IP-ADDRESS-OF-SERVER
- Port: MAPPED-PORT-ON-HOST

For example, in the example explained above, `MAPPED-PORT-ON-HOST` is `1001` for `mysql-1` and `1002` for `mysql-2`.

Assuming your server ip is `123.123.123.123` and your mapped port is `9999`:

- For Mongo DB, you would use `mongodb://dbuser:dbpassword@123.123.123.123:9999/dbname`
- For MySQL, you would use `HOST: 123.123.123.123`, `PORT: 9999`
- and etc...

**IMPORTANT:** After port mapping is done make sure to open the server port. For example, if you mapped port 4444 of your host (server) to port 3306 of your container, you need to run the following command:

```bash
ufw allow 4444
```

---

## Best Practices

CapRover is designed to be easy to use and intuitive. Having said that, there are a few tips and tricks that can help you get the most out of CapRover.

### Hidden Root Domain

It's always a good practice to hide your tech stack from the potential attacker. To be extra secure, you can hide your root domain two levels deeper than your wildcard DNS settings. For example on your DNS panel you set

```
A RECORD:

*.server.domain.com   >>>>   123.123.123.123
```

Then when setting up CapRover, instead of entering `server.domain.com`, enter `something.server.domain.com`. This way, you can access the dashboard via `captain.something.server.domain.com` and not `captain.server.domain.com`. You can then set your app's domain to `myapp.server.domain.com` under the app's HTTP settings to hide your root domain.

Keep in mind this is not a shield that protects you from everything. It's just a security measure that makes it harder, and nearly impractical for some brute force attackers to attack your CapRover infrastructure.

### Custom Default Password

CapRover uses `captain42` as its default password. This is usually safe as you can change your password by running `caprover serversetup` from your local machine right after the server installation is finished. However, this leaves a small window of 30 seconds or so for the attacker to change your password before you. This is very unlikely, but it's possible for this attack to happen. The attacker needs to know the exact attack window on a particular machine. Anyways, to mitigate this risk, simply choose a custom initial password when installing CapRover by adding `DEFAULT_PASSWORD` env var to the installation script. For example, the script below changes the default password from `captain42` to `myinitialpassword`

```bash
docker run -e ACCEPTED_TERMS=true -e DEFAULT_PASSWORD='myinitialpassword' -p 80:80 -p 443:443 -p 3000:3000 -v /var/run/docker.sock:/var/run/docker.sock -v /captain:/captain caprover/caprover
```

### Enforce HTTPS

It is highly recommended that one of the first things you do is to enable HTTPS and enable "Enforce HTTPS" for your CapRover dashboard. After you've done all these, you should change your password. Note that if you are using `caprover serversetup` wizard, you will be doing this process automatically, no need to change your password after the setup.

### Use Service Accounts for Git

One of the most popular features of CapRover is the automatic deployment from source control (GitHub, BitBucket, GitLab and etc...). For this approach to work with a private repository, you have to enter your username/password and they will be kept as encrypted content on your server. It is always a good practice to create a service account (bot account) on GitHub and etc, and give that account specific permission (read only) to certain repositories only. Such that if that account was compromised, your main owner account remains intact and you can remove the compromised account from the repo.

### Out of Memory when Building

When you build on a paid service such as Heroku, your build process happens on a machine with high CPU and RAM. When you use CapRover, your build is done on the same machine that serves your app. This is not a problem until your app gets too big and the build process requires too much RAM. In that case, your build process might crash! See [**this**](https://github.com/caprover/caprover/issues/315) for example. There are multiple solutions:

1- Add swap space to the web server, explained [**here**](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-16-04).

2- Build on your local machine. For example, this process is explained in detail [**here**](https://caprover.com/docs/recipe-deploy-create-react-app.html) for Create React App.

3- However, **the best solution** is to use a separate build system. You can see the guide [**here**](https://caprover.com/docs/ci-cd-integration.html)

### Customize the NGINX Config for new apps

Moved to [https://caprover.com/docs/nginx-customization.html#customize-and-override-the-nginx-config-for-all-apps](https://caprover.com/docs/nginx-customization.html#customize-and-override-the-nginx-config-for-all-apps)

This section is kept here to avoid link breaking.

---

## Firewall & Port Forwarding

Captain uses:

- 80 TCP for regular HTTP connections
- 443 TCP/UDP for secure HTTPS and HTTP/3 connections
- 3000 TCP for initial Captain Installation (can be blocked once Captain is attached to a domain)
- 7946 TCP/UDP for Container Network Discovery
- 4789 TCP/UDP for Container Overlay Network
- 2377 TCP/UDP for Docker swarm API
- 996 TCP for secure HTTPS connections specific to Docker Registry

In case of an ubuntu server, run

```bash
ufw allow 80,443,3000,996,7946,4789,2377/tcp; ufw allow 7946,4789,2377,443/udp;
```

Note that for a more secure installation you can only expose 80/443/3000 to the world, the rest of the ports are only used in a cluster, and it would suffice to make them open to the other nodes in the cluster.
If you have a single instance, just run:

```bash
ufw allow 80,443,3000
```

Also, if you are using Port Mapping to allow external connections, for example from your laptop to a MySQL instance on Captain, you will have to add the corresponding port to the exclusion as well.

NOTE:
Docker bypasses ufw for mapped ports. If you have manually added a mapped port for any of your apps deployed under CapRover, ufw does not necessarily block the ports. See the [relevant information here](https://askubuntu.com/questions/652556/uncomplicated-firewall-ufw-is-not-blocking-anything-when-using-docker)

---

## Troubleshooting

This section covers most frequent issues that users may encounter.

### Cannot connect <ip_server>:3000?

There is a whole set of reasons for this.

#### First)

You need to make sure that CapRover is running on your server. To check this, ssh to your server and run

```bash
docker service ps captain-captain --no-trunc
```

You might see Captain is getting restarted constantly due to an error. Fix the issue and retry. For example, see [error creating vxlan interface](https://github.com/caprover/caprover/issues/14#issuecomment-345447689), or [error while creating mount source path](https://github.com/caprover/caprover/issues/352). Linode, for example, has many problems, such as [subnet sandbox join failed](https://github.com/docker/machine/issues/2753#issuecomment-171822791) and [vxlan interface](https://github.com/docker/machine/issues/2753#issuecomment-188353704). Search [CapRover Github issues](https://github.com/caprover/caprover/issues) for your problem and if you can't find a solution, create a new issue on Github.

#### Second)

If you don't see any errors when your ran `docker service ps captain-captain --no-trunc`, then try

```bash
docker service logs captain-captain --since 60m

## you should also get the logs from nginx

docker service logs captain-nginx --since 60m
```

You might see that CapRover is getting restarted constantly due to an error. Search [CapRover Github issues](https://github.com/caprover/caprover/issues) for your problem and if you can't find a solution, create a new issue on Github.

#### Third)

If both "First" and "Second" debugging steps explained above worked fine and there is no error seen in the logs, run this command on your server:

```bash
curl localhost:3000 -v
```

If successful, it's probably your firewall that's blocking the connection. See [Firewall Docs](https://caprover.com/docs/firewall.html).

### Successful Deploy but 502 bad gateway error!

This applies to you if:

- You have been able to setup your server and access it via `captain.rootdomain.example.com`.
- You have been able to deploy one of the samples apps (see [here](https://github.com/caprover/caprover/tree/master/captain-sample-apps)) successfully and it worked.
- You tried to deploy your own application and it deployed successfully, but when you try to access it via `yourappname.root.example.com` you get a 502 error.

If all above points are correct, this is how to troubleshoot:

- SSH to your server and view your application logs. Make sure it hasn't crashed and it's running. To view logs, please see the section at the end of this page " [How to view my application's log](https://caprover.com/docs/troubleshooting.html#how-to-view-my-applications-log)"
- If you application logs show that your application is running, the most common case is that your application is binding to a custom port, not port 80. For example, CouchDB runs at port 5984. In this case, go to app's settings on CapRover, go to HTTP Settings, then select 5984 as the "Container Port".
- If your app defines the binding IP address as 127.0.0.1, change it to `0.0.0.0`, see [this issue](https://github.com/caprover/caprover/issues/76#issuecomment-481053496) for more details.

### Domain Verification Failed - Error 1107!

This happens when CapRover cannot verify that yourcustomdomain.com points to the IP address of CapRover. This can be caused by several factors:

- DNS changes take up to 24 hrs to propagate, specially if your server had cached them before. So wait for 24hrs and retry again. If it doesn't work, proceed to the next step:
- To confirm, go to [https://mxtoolbox.com/DNSLookup.aspx](https://mxtoolbox.com/DNSLookup.aspx) and enter `yourcustomdomain.com`. Make sure it points to the server IP. If you're using a proxy service like CloudFlare, this may cause a problem. Disable their proxy in your DNS on CloudFlare and have A record directly point to the IP address of your CapRover server.
- If you tested all above, and when you visit `something.domain.com` you see the CapRover page, then you can say your domain is working fine, but CapRover is unable to verify it because the loopback test doesn't work. In this case, you can choose to skip domain verification done by CapRover:

```bash
echo  "{\"skipVerifyingDomains\":\"true\"}" >  /captain/data/config-override.json
docker service update captain-captain --force
```

- If none of the above works, please open an issue on Github.
- **AWS EC2 Users** - Check to make sure the CIDR Block of your VPC is above 172.0.0.0/16 (NOT 0.0.0.0/16, which is common).

### Connection Timeouts

Sometimes when you have an inactive database connection pool, Docker drops the connection after some time. To fix, you can do either of these:

- Implement an automatic retry strategy
- Implement a automatic ping every few minutes to ensure that the connection doesn't become inactive
- Changing Keepalive config in your app (see [here](https://github.com/caprover/caprover/issues/873#issuecomment-715328966) for an example on knex)
- Make changes to your Docker configs (more advanced)

The [root cause](https://github.com/moby/moby/issues/31208) is not related to CapRover, it's an underlying Docker issue.

### Something bad happened

When you see this error in the UI, it means something "unexpected" went wrong such as connection lost, server crashing (due to out of memory), etc. The best way to see what's happening is to get the server logs:

```bash
docker service logs captain-captain --since 5m --follow
```

### How to view my application's log?

Your application is deployed as a Docker service. For example, if your app name in captain is `my-app` you can view your logs by connecting to your server via SSH and run the following command:

```bash
docker service logs srv-captain--my-app --since 60m --follow
```

Note that Docker service name is prefixed with `srv-captain--`. Also, you can replace 60m with 10m to view last 10 minutes.

### How to restart my application?

If your application is not behaving well, you can try force restarting it by going to the web dashboard and select your app, then click on "Save Configuration & Update" button. It will forcefully restarts your application.

### How to run shell inside my application (inside container)

Simply run the following command:

```bash
docker exec -it $(docker ps --filter name=srv-captain--myappname -q) /bin/sh
```

Of course, you need to replace `myappname` with your own app name.

### I've made a change to the Nginx config that broke the admin UI!

In this case restart is not going to help. [Do this](https://github.com/caprover/caprover/issues/412#issuecomment-484077130):

Run the nginx fixer to revert **all nginx changes that you've manually made**:

```bash
docker service scale captain-captain=0 && \
docker run -it --rm -v /captain:/captain  caprover/caprover /bin/sh -c "wget https://raw.githubusercontent.com/caprover/caprover/master/dev-scripts/clear-custom-nginx.js ; node clear-custom-nginx.js ;" && \
docker service scale captain-captain=1 && \
echo "OKAY"
```

Hopefully your problem should be resolved and you can be happy.

### How to restart CapRover

If your CapRover is not behaving well, you can try force restarting CapRover using:

```bash
docker service update captain-captain --force
```

### How to use the Edge version

Edge version gets automatically built with every push on master. If your version has a particular bug that is just fixed on master branch, you can temporarily update your CapRover to use the Edge version. Note that once you switch to edge, you won't receive updates. With the next release of CapRover, you have to manually switch back to CapRover. Note that this is an advance operations. Also, as a rule of thumb, once you switch to Edge, do not switch back to the regular version until a new version is released.

To switch to edge

```bash
docker pull caprover/caprover-edge:latest
docker service update captain-captain --image caprover/caprover-edge:latest
```

To switch back to the main image

```bash
docker service update captain-captain --image caprover/caprover:latest
```

### Customize Config Settings

You can customize any constant defined in [CaptainConstants](https://github.com/caprover/caprover/blob/master/src/utils/CaptainConstants.ts) under configs by adding a JSON file at `/captain/data/config-override.json`. For example, to change `defaultMaxLogSize`, the content of `/captain/data/config-override.json` will be:

```json
{
 "defaultMaxLogSize":"128m"
}
```

After editing this file, [restart CapRover](https://caprover.com/docs/troubleshooting.html#how-to-restart-caprover) (if the change affects CapRover, nginx or certbot) or turn NetData off and on again from the UI.

### Use existing swarm

When you first install CapRover, it tries to automatically set up a swarm cluster for you. But in rare cases, you may already have a swarm cluster, and you want to use that cluster. In this case, you can simply just override it by setting `useExistingSwarm` to true. Run the following script before attempting to install CapRover.

```bash
mkdir -p  /captain/data
echo  "{\"useExistingSwarm\":\"true\"}" >  /captain/data/config-override.json
```

### AWS setup

AWS has its own customization with regards to port handling and etc. It make require some custom setup, see [this blog post for example](https://fuzzyblog.io/blog/caprover/2019/11/10/using-caprover-on-aws.html).

### CloudFlare SSL setup

When using CloudFlare free plan, keep in mind its [Universal SSL only supports SSL up to 1st level subdomains](https://developers.cloudflare.com/ssl/edge-certificates/universal-ssl/limitations/#full-setup). So, if you enable CloudFlare's Universal SSL and set up a 1st level subdomain as a root domain for CapRover, you'll get the following error when trying to access the apps deployed by CapRover:

```
This site can't provide a secure connection
app.root.example.com uses an unsupported protocol.
ERR_SSL_VERSION_OR_CIPHER_MISMATCH
```

If you want to use CapRover with the CloudFlare's Universal SSL, avoid using a subdomain as a root domain.

### ARM processor

As of 1.8.1, CapRover works on arm processors like "raspberry pi" and such. Note that some one click apps may not work on rasberry pi. One click apps are external apps that are not maintained by CapRover.

### Reset Password

If you forgot your password but you have access to your server via SSH:

- SSH to your server
- Run

```bash
docker service scale captain-captain=0

# backup config
cp /captain/data/config-captain.json /captain/data/config-captain.json.backup

# delete old password
jq 'del(.hashedPassword)' /captain/data/config-captain.json > /captain/data/config-captain.json.new
cat /captain/data/config-captain.json.new > /captain/data/config-captain.json
rm /captain/data/config-captain.json.new

# set a temporary password
docker service update --env-add DEFAULT_PASSWORD=mytemppassword captain-captain
docker service scale captain-captain=1
```

- Login to CapRover with your temporary password and change your password from settings.

### How to stop and remove Captain?

CapRover uses docker swarm to support clustering and restarting containers if they stop. In order to fully uninstall CapRover from your system, run this:

```bash
docker service rm $(docker service ls -q)
## remove CapRover settings directory
rm -rf /captain
## leave swarm if you don't want it
docker swarm leave --force
## full cleanup of docker
docker system prune --all --force
```

### I got an email from Let's Encrypt saying my domain's SSL certificate is expiring, and it shouldn't be.

This can happen when you've used the same domain name for a previous project, which you then deleted.
Let's Encrypt keeps track of the old certificate and notifies you when it is due to expire, but this doesn't affect the new certificate.
To confirm, simply just check your SSL expiry date using an online tool like this:
[https://www.sslshopper.com/ssl-checker.html#hostname=captain.server.demo.caprover.com](https://www.sslshopper.com/ssl-checker.html#hostname=captain.server.demo.caprover.com)

---