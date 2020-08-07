# Run cBioPortal using Docker Compose
Download necessary files (seed data, example config and example study from
datahub):
```
./init.sh
```

Start docker containers. This can take a few minutes the first time because the
database needs to import some data.
```
docker-compose up
```
In a different terminal import a study
```
docker-compose run cbioportal metaImport.py -u http://cbioportal:8080 -s study/lgg_ucsf_2014/ -o
```

Restart the cbioportal container after importing:
```
docker-compose restart cbioportal
```

The compose file uses docker volumes which persist data between reboots. To completely remove all data run:

```
docker-compose down -v
```

## Loading other seed databases
### hg38 support
To enable hg38 support. First delete any existing databases and containers:
```
docker-compose -v
```
Then run
```
init_hg38.sh
```
Followed by:
```
docker-compose up
```
When loading hg38 data make sure to set `reference_genome_id: hg38` in [meta_study.txt](https://docs.cbioportal.org/5.1-data-loading/data-loading/file-formats#meta-file-4). The example study in `study/` is `hg19` based. 

## Example Commands
### Connect to the database
```
docker-compose run cbioportal_database \
    sh -c 'mysql -hcbioportal_database -u"$MYSQL_USER" -p"$MYSQL_PASSWORD" "$MYSQL_DATABASE"'
```

## Advanced topics
### Run different cBioPortal version

A different version of cBioPortal can be run using docker-compose by declaring the `DOCKER_IMAGE_CBIOPORTAL`
environmental variable. This variable can point a DockerHub image like so:

```
export DOCKER_IMAGE_CBIOPORTAL=cbioportal/cbioportal:3.1.0
docker-compose up
```

which will start the v3.1.0 portal version rather than the newer default version.
