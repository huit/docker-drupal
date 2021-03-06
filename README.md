docker-drupal
=============

*This is still a work in progress, and does not yet work correctly.*

This repo contains a recipe for making a set of [Docker](http://docker.io) containers for Drupal, using Linux, Apache and MySQL, with [Vagrant](http://vagrantup.com) orchestrating the container setup. It aims to demontrate the use of the [Twelve-Factor App](http://12factor.net) in a simple fashion, using docker containers.

# Getting started

To build with Vagrant, make sure you have at least version [1.6](http://www.vagrantup.com/downloads.html) installed, with the requisute support for Docker. 

This will try to go in line with [Drupal automated-testing](https://drupal.org/automated-testing).

To run, simply clone this repository 
```
git clone https://github.com/huit/docker-drupal.git
cd docker-drupal
```

and run vagrant up with the docker provider:
```
vagrant up mysql --provider=docker
vagrant up drupal --provider=docker
vagrant up varnish --provider=docker
```

This will download and build, then run the container.  Note: we start up each individually in order to avoid some funky race conditions when docker tried to link the containers. Hopefully this will be resolved in a later version of Vagrant.


(BELOW IS UNMODIFIED TEXT, WHICH MAY NOT BE RELEVANT. REVIEW NEEDED.)

this can take a while but should eventually return a command prompt. It's done when it says "Successfully built {hash}"

## And run the container, connecting port 80:
```
sudo docker run -d -t -p 80:80 <yourname>/drupal
```
That's it!
Visit http://localhost/ in your webrowser. 

Note: you cannot have port 80 already used or the container will not start.
In that case you can start by setting: `-p 8080:80`

build directly from github is broken at the moment:
```
sudo docker build -t <yourname>/drupal git://github.com/ricardoamaro/docker-drupal.git
```


### Credentials

* ROOT   MYSQL_PASSWORD will be on /mysql-root-pw.txt
* DRUPAL MYSQL PASSWORD will be on /drupal-db-pw.txt
* Drupal account-name=admin & account-pass=admin


## More docker awesomeness

This will create an ID that you can start/stop/commit changes:
```
# sudo docker ps
ID                  IMAGE                   COMMAND               CREATED             STATUS              PORTS
538114c20d36        <yourname>/drupal:latest   /bin/bash /start.sh   3 minutes ago       Up 6 seconds        80->80  
```

Start/Stop
```
sudo docker stop 538114c20d36
sudo docker start 538114c20d36
```

Commit the actual state to the image
```
sudo docker commit 538114c20d36 <yourname>/drupal
```

Starting again with the commited changes
```
sudo docker run -d -t -p 80:80 <yourname>/drupal /start.sh
```

Shipping the container image elsewhere 
```
sudo docker push  <yourname>/drupal
```

You can find more images using the [Docker Index][docker_index].

ricardoamaro/drupal-lamp

### Clean up
While i am developing i use this to rm all old instances
```
sudo docker ps -a | awk '{print $1}' | grep -v CONTAINER | xargs -n1 -I {} docker rm {}
``` 

### Known Issues
* Upstart on Docker is broken due to [this issue][docker_upstart_issue], and that's one of the reasons the image is puppetized using vagrant.
* Warning: This is still in development and ports shouldn't be open to the outside world.


## Contributing
Feel free to fork and contribute to this code. :)

1. Fork the repo
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## Authors

Created and maintained by [Ricardo Amaro][author] (<mail_at_ricardoamaro.com>)

## License
GPL v3

[author]:                 https://github.com/ricardoamaro
[docker_upstart_issue]:   https://github.com/dotcloud/docker/issues/223
[docker_index]:           https://index.docker.io/

