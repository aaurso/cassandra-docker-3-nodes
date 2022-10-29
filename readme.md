
## Folder Structure:

   - ./
   - ├── data/
   - │  ├── node1/
   - │  ├── node2/
   - │  └── node3/
2,5k ├── docker-compose-cassandra.yaml
   - ├── etc/
   - │  ├── node1/
   - │  ├── node2/
   - │  └── node3/
   - ├── etc_cassandra/
 13k │  ├── cassandra-env.sh
 148 │  ├── cassandra-jaas.config
2,0k │  ├── cassandra-rackdc.properties
1,4k │  ├── cassandra-topology.properties
 68k │  ├── cassandra.yaml
2,1k │  ├── commitlog_archiving.properties
6,4k │  ├── cqlshrc.sample
3,5k │  ├── hotspot_compiler
 533 │  ├── jvm-clients.options
7,8k │  ├── jvm-server.options
 457 │  ├── jvm8-clients.options
2,6k │  ├── jvm8-server.options
1,3k │  ├── jvm11-clients.options
4,2k │  ├── jvm11-server.options
1,2k │  ├── logback-tools.xml
5,1k │  ├── logback.xml
1,6k │  ├── metrics-reporter-config-sample.yaml
 291 │  ├── README.txt
   - │  └── triggers/
   0 └── readme


## Requirements:

- Installed Docker
- Installed Docker-compose

## Notice:

The following procedure is tested on OS X 13 Ventura on Macbook Pro M1 arm64 with ZSH shell.

If you have different platform or architecture or use a different shell than "zsh" there is no guarantee that the terminal commands will be exaclty the same as far as systax goes but this should work on any sistem


## Instructions:

you SHOULD be able to run the cluster just by:

      docker compose -f docker-compose-cassandra.yaml up

If you are not able to run the cluster using that command it means you have not the required files to start the containet and you should follow the subsequent procedure:

   *first: create a folder named "Cassandra_Cluster" and cd into it*
     
      mkdir etc_cassandra etc
      mkdir etc/node1 etc/node2 etc/node3

      docker run --rm -d --name tmp cassandra:latest docker cp tmp:/etc/cassandra etc_cassandra
      docker stop tmp

      cp -a etc_cassandra etc/node1
      cp -a etc_cassandra etc/node2
      cp -a etc_cassandra etc/node3

      docker compose -f docker-compose-cassandra.yaml up


## Further configuration:
   
- modify "cassandra.yaml" to your liking

- you can change the number of clusters from 3 to how many you need,
   just remember that:

      (for each node) mem_limit: 2g  ← set to a sensible value 

      line23:   CASSANDRA_SEEDS: "node1,node2"    # here you can set the seeds

      (for each node)   ports: "XXXX:1st-node-port" #the second number needs to be the port number of the 1st node
      
      (for each node)  depends_on: # you can set the dependencies 
                         nodeX  ← can be the previous, or another, or the first
                           condition: (service_started | service_healthy | service_completed_successfully)

## If you need help:
    
   git: aaurso
   email:a.urso12@studenti.uniba.it






