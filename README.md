## Start the project

```
docker-compose up -d
```

Docker images will be pulled (if needed) and containers will be started, but you still need to set them up with these 2 procedures :

### Install the toolkit

Download the latest release of the toolkit and place it at the root of this folder and name it `informea.war`

```
wget -O informea.war https://github.com/InforMEA/odata.provider/releases/download/v.2.2.2/informea-2.2.2.war
```

Prepare the files for Tomcat.

```
rm -rf webapp || true
unzip informea.war -d webapp
rm informea.war
```

Configure the DB parameters

```
cp webapp/WEB-INF/classes/META-INF/persistence.template.xml webapp/WEB-INF/classes/META-INF/persistence.xml
sed -i 's/mysql:\/\/[^\?]*/mysql:\/\/db:3306\/web/g' webapp/WEB-INF/classes/META-INF/persistence.xml
sed -i 's/jdbc\.user" value="[^"]*"/jdbc.user" value="root"/g' webapp/WEB-INF/classes/META-INF/persistence.xml
sed -i 's/jdbc\.password" value="[^"]*"/jdbc.password" value="root"/g' webapp/WEB-INF/classes/META-INF/persistence.xml
```

And finally, restart Tomcat

```
docker-compose restart informea
```

## Database

Download the DB dump and import it using the following command

```
mysql -h 127.0.0.1 -P 9933 -u root -proot web < my_dump_file.sql
```

If you don't have the `mysql` command available in your CLI environment, you may use any MySQL client :
- Host : `127.0.0.1` (or `localhost`)
- Port : `9933`
- User : `root`
- Password : `root`

## Access from your browser

Everything should be ready by now, you may brose the toolkit at the following URL :

http://127.0.0.1:9980/informea

## Upgrade the toolkit

Just repeat the exact same steps as during toolkit installation !
