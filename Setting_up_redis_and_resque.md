> ### Note: These instructions only apply for HIEv version 1.9.01 or greater

## Redis

Redis is an open source, BSD licensed, advanced key-value store. It is often referred to as a data structure server since keys can contain strings, hashes, lists, sets and sorted sets. This is used in conjunction with Resque to store queues in Redis.

### Installation
Redis version 2.6.13 is contained within the project directory. Go to the project root and in redis directory.

```
$ cd <project_root_directory>/redis
$ tar zxf redis-2.6.13.tar.gz
$ cd redis-2.6.13
$ make
$ sudo make install
```

Create two directories to store your Redis config files and your data
```
$ sudo mkdir /etc/redis
$ sudo mkdir /var/redis
```

Copy the init script that you'll find in the Redis distribution under the utils directory into /etc/init.d. We use the name of the port of the running instance of Redis.
```
$ sudo cp utils/redis_init_script /etc/init.d/redis_6379
```

Edit the init script.
```
$ sudo vi /etc/init.d/redis_6379
```
Make sure to modify REDIS_PORT accordingly to the port you are using. Both the pid file path and the configuration file name depend on the port number.

Copy the template configuration file you'll find in the root directory of the Redis distribution into /etc/redis/ using the port number as name.
```
$ sudo cp redis.conf /etc/redis/6379.conf
```

Create a directory inside /var/redis that will work as data and working directory for this Redis instance
```
$ sudo mkdir /var/redis/6379
```

Edit the configuration file, making sure to perform the following changes:
```
$ sudo vi /etc/redis/6379.conf
```
* Set **daemonize** to yes (by default it is set to no).
* Set the **pidfile** to /var/run/redis_6379.pid (modify the port if needed).
* Change the **port** accordingly. It is not needed as the default port is already 6379.
* Set your preferred **loglevel**.
* Set the **logfile** to /var/log/redis_6379.log
* Set the **dir** to /var/redis/6379 (very important step!)

### Starting up Redis service
```
$ sudo /etc/init.d/redis_6379 start
```

Check if it works by pinging it
```
$ redis-cli ping
```

## Resque
Resque is a Redis-backed Ruby library for creating background jobs, placing them on multiple queues, and processing them later. This project currently uses it for queueing packages and can be expanded to other modules of the web app.

### Starting a single worker
To start a single worker, go to the project root directory and run the following:
```
$ bundle exec rake daemon:resque:start RAILS_ENV=<env>
```
Where <env> is your target environment, eg. qa, staging, production. Please give it a few minutes for the workers to start up.

### Starting a multiple workers
To start multiple workers:
```
$ bundle exec rake daemon:resque:start WORKERS=2 RAILS_ENV=<env>
```
Where you specify the number of workers you wish to create for WORKERS. In this example we create 2 workers.

### Stopping workers
To stop the worker
```
$ bundle exec rake daemon:resque:stop
```

### Check status of workers
To check the status of the worker
```
$ bundle exec rake daemon:resque:status
```

### Debugging & Known issues

There is an issue when terminating multiple workers does not kill the processes. To terminate them manually, first find the resque processes:
```
$ ps -eF | grep resque
devel    19459     1  0 82343 97456   1 Jun17 ?        00:01:01 resque-1.24.1: Waiting for * 
```
Then run a kill command to each of the listed processes
```
$ kill 19459
```