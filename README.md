# kaha
quick relief lookup for nepal earthquake

# Run the app locally 
## Install dependencies

    npm install 

## Run the app in dev mode

    npm build-dev-watch

This should build a bundle.js file for your app to run in dev mode, plus starts up 
the server as well. The bundle.js file is automatically rebuild on any change made 
to your js files.

## Run the app

    node bin/www

Access the app at http://localhost:3000/

**NOTE**:-
When you do `npm run start` or `node bin/www` then by default it'll run on staging db.
###To run on prod db do:-

    npm run prod

but you'll need to set a db environment (DBPWD) for db passwd.
Contact me or contributors in this repo for the db pass.

## Using docker

To ensure uniform environments across all of dev, staging and prod (and quickly get the app up and running), you can run kaha from docker.

First install docker following the instructions  [here](https://docs.docker.com/installation/).

### Build the kaha docker image

    sudo docker build -t kaha .

You can see it now from `sudo docker images`

### Dev use (against staging)

To run kaha node app against the remote staging db, do this from the repo root directory:

    sudo docker run --name kaha_stage -v $(pwd):/kaha -p 3000:3000 kaha

This creates a docker container called `kaha_stage` based on the kaha image, with the repo dir used as a shared volume inside the container (so that code changes are picked up).

### Dev use (local db)

To run kaha against a local redis installation:

    sudo pip install docker-compose
    sudo docker-compose up

Based on docker-compose.yml, this creates and runs two containers: `kaha_kaha_1` and `kaha_redis_1` (one each for the node app and redis), and links the two.
Using docker compose thus simplifies the dev setup. But if you prefer to do it directly with docker:

    sudo docker run -d --name redis -p 6379:6379 redis && \
    sudo docker run --name kaha_dev -v $(pwd):/kaha -p 3000:3000 --link redis:db kaha

### Prod use

```
sudo docker run --name kaha_prod -v $(pwd):/kaha -p 3000:3000 kaha npm run build-dist && NODE_ENV=prod DBPWD=<passwd> node ./bin/www
```

This creates container `kaha_prod` and uses remote prod redis instance hosted on redislabs. For ease in deployments, a separate dockerfile can be setup with prod details and kaha image can be added to the public docker registry.

### Managing docker containers

    # list the running containers
    sudo docker ps
    # stop and start the container (kaha_stage, kaha_redis_1 etc.)
    sudo docker stop <container_name>
    sudo docker start <container_name>
    # remove container
    sudo docker rm <container_name>

For more details, read the docker docs.

```
The MIT License (MIT)

Copyright (c) 2015 kaha.co

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
