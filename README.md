# TLNewsSpider - Public Opinion Information Acquisition

# I. Project Introduction

TLNewsSpider is an opinion information acquisition and visualization platform based on GNE (General News Extractor) module to extract information such as body content, title, author, release time, image address and source code of the tag where the body is located from 400+ opinion sites. The main technologies of the project cover python, scrapy, scrapyd, scrapydweb (open source management platform), mysql, redis, and support any number of crawlers running in one click, timed tasks, batch deletion, one-click deployment, and can realize crawler monitoring visualization, configuration of cluster crawler allocation policy, ready-made docker one-click deployment, etc. The features. The most recent fully liberalized conditions can catch 500W+ valid data.


![image](https://user-images.githubusercontent.com/37069873/177952594-0aabe51f-aaeb-44cf-a65d-f6b5cc391a71.png)
![image](https://user-images.githubusercontent.com/37069873/177952680-8ef1dcd5-123e-4e37-adae-afe555233af1.png)
![image](https://user-images.githubusercontent.com/37069873/177952747-775d4329-08e8-465d-9cb0-d099ddcdc43f.png)


# II. Technical components

main technologies: python, scrapy, scrapyd, scrapydweb (open source management platform), mysql, redis

crawler configurable middleware.

- redis de-duplication middleware
- request retry middleware
- proxy middleware
- crawler details information is pushed to third-party applets (WeChat public, Fishu, etc.-to be developed) at regular intervals

Configurable data pipelines for crawlers:

- Data pre-processing
- Data filtering and cleaning
- Data storage in the library
- data into redis (same as redis de-duplication)

Other functions.

- Data push to Kafaka
- Crawler optional restrictions (time range, number of page turns)

Other scrapy advanced features

# III. Installation and deployment

> You need to upload the project to the server fixed directory: /home/spider_workplace/

## 1、Install Docker, Docker-Compose

### 1.1. Install docker online (centos system)

**under sudo privileges** 

```bash
yum update 
yum install -y yum-utils
yum install docker
# Use the domestic mirror repository
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s https://registry.docker-cn.com
# Reboot
systemctl restart docker
# Start the test
docker run hello-world
```

### 1.2. Install docker offline 

Reference: https://blog.csdn.net/xinle0320/article/details/124205608

### 1.3, install docker-compose online

```bash
curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# Add permissions
chmod +x /usr/local/bin/docker-compose
```

### 1.4. Offline installation

1. github Offline download the corresponding version of docker-compose

> https://github.com/docker/compose/releases/download/{version}/docker-compose-linux-x86_64

2. upload to service directory: /usr/local/bin/docker-compose
3. Modify permissions: chmod +x /usr/local/bin/docker-compose

## 2. Install the database

```bash
1. Add the database sql table structure file that needs to be initialized > Rename init.sql Upload
## Database storage directory
mv . /docker_yml/database /data/ && cd /data/database
# Install and start
docker-compose up -d
# Check if it started successfully
docker-compose ps
```

## 3. Install scrapyd, scrapydweb, logparser

```bash
cd . /docker_yml/news_crawl 
## Install and start
docker-compose up -d
# Check if it started successfully
docker-compose ps
# Failed to start reference: 4. Modify scrapydweb configuration, add cluster, etc.
```

## 4. Modify scrapydweb configuration, add clusters, etc.

```bash
# Modify the scrapydweb_settings_v10.py configuration file according to the error report or other requirements
vim /home/spider_workplace/TLNewsCrawl/TLNewsSpider/scrapyd_web_manager/scrapydweb_settings_v10.py
# Rebuild the container
docker-compose down
docker-compose up -d
# View logs.
docker-compose logs
```

# IV. Task scheduling

### 1. Introduction to use

Based on scrapydweb's public interface, we manage the scheduling of crawler tasks in batch, eliminating the need for a single click on the front-end page Note: 1. Divide the available nodes and hosts before operation
There are two strategies for dividing the number of crawler tasks in cluster mode**<u>multiple nodes</u>**: 

- def: group_spiders_by_chars Crawlers are divided equally by initials 
- def: group_spiders_by_length equal division of crawlers by number of crawlers **(the one currently used)**

```bash
>> python auto_manage_spiders.py -h

usage: auto_manage_spiders.py [-h] [-host HOST] [-n {1,2}] [-t {0,1,2}] [-dp]
                              [-bsd] [-bst] [-bdt] [-bdj] [-ls]
Based on scrapydweb's public interface, manage the scheduling of crawler tasks in batches, eliminating the need for single-click operations on front-end pages Note: 1. divide the available nodes and hosts before operation

optional arguments:
  -h, --help show this help message and exit
  -host HOST, --host HOST
                        host: current cluster host node
  -n {1,2}, --node {1,2}
                        node: index of currently available cluster nodes
  -t {0,1,2}, --time {0,1,2}
                        Timed task type selection (hours) 0: untimed
  -dp, --deploy deploy: Deploy the most popular version of the crawler
  -bsd, --batch_schedule
                        batch_schedule: schedule all crawlers with one click
  -bst, --batch_stop_job
                        batch_stop_job: Stop all crawlers with one click
  -bdt, --batch_delete_task
                        batch_delete_task: Delete all cron timed tasks with one click
  -bdj, --batch_delete_job
                        batch_delete_job: Delete all tasks with one click
  -ls, --listspiders listspiders: View all crawler names
```

### 2. Usage examples

**NOTE: Tips**

1. execute the script anywhere under network interoperability; otherwise execute it inside the container.

2. --host parameter default value set to current master node **(required)**

3. host_list: list plus all current node hosts **(required)**

4. cron_list: set cron parameters for timed tasks on demand -- currently set to hourly, modify if needed **(not necessary)**

   

```python
# Batch start crawler for node 1
python auto_manage_spiders.py -n 1

# Batch start the crawler for node 2, and add it to a timed task
python auto_manage_spiders.py -n 2 -t 1

# Deploy a new version of the crawler
python auto_manage_spiders.py -dp
```

![image](https://user-images.githubusercontent.com/37069873/178644546-21589244-2ef6-4fa4-b12c-ce1139b4f7aa.png)

ps: the core of this project is the overall large framework of the deployment application and some advanced module applications

# V. Wall recommendation
- If this project is helpful to your work, learning, please move your little finger to get rich
- Give the author the power to follow up more open source ** ↓ ↓ ↓ **



Translated with www.DeepL.com/Translator (free version)