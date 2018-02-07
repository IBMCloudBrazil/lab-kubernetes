

    Launch a shell and confirm that Docker is installed. The version number is not particularly important.

    $ docker -v
    Docker version 17.06.1-ce, build 874a737

    As with all new computer things, it is obligatory that you start with "hello-world"

    $ docker run hello-world
    Unable to find image 'hello-world:latest' locally
    latest: Pulling from library/hello-world
    b04784fba78d: Pull complete
    Digest: sha256:f3b3b28a45160805bb16542c9531888519430e9e6d6ffc09d72261b0d26ff74f
    Status: Downloaded newer image for hello-world:latest

    Hello from Docker!
    This message shows that your installation appears to be working correctly.

    To generate this message, Docker took the following steps:
     1. The Docker client contacted the Docker daemon.
     2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
     3. The Docker daemon created a new container from that image which runs the
        executable that produces the output you are currently reading.
     4. The Docker daemon streamed that output to the Docker client, which sent it
        to your terminal.

    To try something more ambitious, you can run an Ubuntu container with:
     $ docker run -it ubuntu bash

    Share images, automate workflows, and more with a free Docker ID:
     https://cloud.docker.com/

    For more examples and ideas, visit:
     https://docs.docker.com/engine/userguide

Notice the message Unable to find image 'hello-world:latest' locally First you see that the image was automatically downloaded without any additional commands. Second the version :latest was added to the name of the image. You did not specify a version for this image.

    Rerun "hello-world" Notice that the image is not pulled down again. It already exists locally, so it is run.

    $ docker run hello-world

    Hello from Docker!
    This message shows that your installation appears to be working correctly.

          [output truncated]

    It already exists locally and docker images shows you that image.

    $ docker images
    REPOSITORY        TAG       IMAGE ID       CREATED        SIZE
    hello-world       latest    1815c82652c0   2 months ago   1.84kB

    From where was the hello-world image pulled? Go to https://hub.docker.com/_/hello-world/ and you can read about this image. Docker-hub is a repository that holds docker images for use. Docker-hub is not the only repository.

    This image is atypical; when an image is run it usually continues to run. The running image is called a container. Next, run a more typical image; this image contains the noSQL database "couchDB".

    $ docker run -d couchdb
    Unable to find image 'couchdb:latest' locally
    latest: Pulling from library/couchdb
    ad74af05f5a2: Downloading [==>                       ]  2.702MB/52.61MB
    ffdd0c835430: Download complete
    d922980c187f: Downloading [===>                      ]  2.661MB/43.77MB
    affbf57fdbcf: Verifying Checksum
    0ddcd7e9244b: Download complete
    34473f480310: Downloading [=============>           ]   2.26MB/8.236MB
    78a52d457cb5: Waiting

The output above was captured while the image was still downloading from docker-hub. When the download is down you do not see anything from the container, like with hello-world. Instead you see a long hex id like 2169c6b42e5c590229c5c86f5ed3596b1b56c2366378914b082e5b000752bd34. This is the id of the container.

    Here is how you would see the running container. Notice only the first part of that long hex id is displayed. Typically this is more than enough to uniquely identify that container. docker ps provides information about when the container was created, how long it has been running, then name of the image as well as the name of the container. Note that each container must have a unique name. You can specify a name for each container as long as it is unique.

    $ docker ps
    CONTAINER ID  IMAGE     COMMAND                  CREATED         STATUS        PORTS     NAMES
    2169c6b42e5c  couchdb   "tini -- /docker-e..."   8 minutes ago   Up 8 minutes  5984/tcp  nervous_poincare

    An image can be run multiple times. Launch another container for the couchdb image.

    $ docker run -d couchdb
    f9885aaf0a96742119462208dce611018ab2104737adf3485d6fc4e7642b104b

    Now you have two containers running the couchdb database. Did you notice how quickly the second instance started? There was no need to download the image this time. The id of the container is show after is has started.

$ docker ps
CONTAINER ID  IMAGE     COMMAND                  CREATED         STATUS        PORTS     NAMES
f9885aaf0a96  couchdb   "tini -- /docker-e..."   2 minutes ago   Up 2 minutes  5984/tcp  brave_booth
2169c6b42e5c  couchdb   "tini -- /docker-e..."   22 minutes ago  Up 22 minutes 5984/tcp  nervous_poincare

    The containers look similar, but they have unique names and unique ids. Stop the most recent container and then check to see what's running.

    $ docker stop f9885aaf0a96
    f9885aaf0a96

    $ docker ps
    CONTAINER ID  IMAGE     COMMAND                  CREATED         STATUS        PORTS     NAMES
    2169c6b42e5c  couchdb   "tini -- /docker-e..."   25 minutes ago  Up 25 minutes 5984/tcp  nervous_poincare

    Stop the other container and see what is running.

    $ docker stop 2169c6b42e5c
    2169c6b42e5c

    $ docker ps
    CONTAINER ID  IMAGE     COMMAND                  CREATED         STATUS        PORTS     NAMES

    Notice the image still exists.

$ docker images
REPOSITORY        TAG       IMAGE ID       CREATED        SIZE
couchdb           latest    7f8923b03b7f   5 weeks ago     225MB
hello-world       latest    1815c82652c0   2 months ago   1.84kB

    Did you forget about the hello-world image? Go ahead and delete the couchdb image and double check that it is gone.

docker rmi couchdb
Error response from daemon: conflict: unable to remove repository reference "couchdb" (must force) - container 2169c6b42e5c is using its referenced image 7f8923b03b7f

    You cannot delete that image until you delete the "couchdb" container. Note the docker ps -a shows all the containers, not just the ones that are running.

    $ docker ps -a
    CONTAINER ID  IMAGE     COMMAND                  CREATED           STATUS                   NAMES
    f9885aaf0a96  couchdb     "tini -- /docker-e..." 22 minutes ago    Exited 2 minutes ago     brave_booth
    b44146cfad65  hello-world "/hello"               An hour ago       Exited About an hour ago elated_engelbart
    8d71894c865b  hello-world "/hello"               2 hours ago       Exited 2 hours ago       stoic_lamport

    Delete the couchdb container, delete the couchdb image, and make sure it is gone. You can leave hello-world.

    $ docker rm f9885aaf0a96
    f9885aaf0a96

    $ docker rmi couchdb
    Untagged: couchdb:latest
    Untagged: couchdb@sha256:eb463cca23b9e9370afbd84ae1d21c0274292aabd11b2e5b904d4be2899141ff
    Deleted: sha256:7f8923b03b7f807ffbd51ff902db3b5d2e2bbbc440d72bc81969c6b056317c8a
    Deleted: sha256:d53bc50464e197cbe1358f44ab6d926d4df2b6b3742d64640a2523e4640104c4
    Deleted: sha256:851748835e9443fa6b8d84fbfada336dffd1ba851a7ed51a0152de3e3115b693
    Deleted: sha256:feb87fb4c017e2d01b5d22dee3f23db2b3f06a0111a941dda926139edc027c8e
    Deleted: sha256:e00c9e10766f6a4d24eff37b5a1000a7b41c501f4551a4790348b94ff179ca53
    Deleted: sha256:b64ffebe7ca9cec184d6224d4546ccdfecce6ddf7f5429f8e82693a8372cf599
    Deleted: sha256:f78934f92a8a0c822d4fc9e16a6785dc815486e060f60b876d2c4df1925584d8
    Deleted: sha256:491c6d0078fa4421d05690c79ffa4baf3cdeb5ead60c151ab64af4fb6d4d93dc
    Deleted: sha256:2c40c66f7667aefbb18f7070cf52fae7abbe9b66e49b4e1fd740544e7ceaebdc

    $ docker ps -a
    CONTAINER ID  IMAGE     COMMAND                  CREATED           STATUS                   NAMES
    b44146cfad65  hello-world "/hello"               An hour ago       Exited About an hour ago elated_engelbart
    8d71894c865b  hello-world "/hello"               2 hours ago       Exited 2 hours ago       stoic_lamport

    $ docker images
    REPOSITORY        TAG       IMAGE ID       CREATED        SIZE
    hello-world       latest    1815c82652c0   2 months ago   1.84kB

         Docker images and containers can be referenced by name or by id.
