# VTV "Alphen" website

Go to the docker directory:

```sh
cd docker
docker compose up -d
```

Bring it back down again:

```sh
docker compose down
```

Go to the root of this repository, fix the permissions and delete the wp_content:

```sh
cd ..
sudo chown -R moni:moni site
sudo rm -rf site/wp-content/
```

Unpack the backup file and move the wp-content into it:

```sh
rm -rf ~/tmp_wp 
mkdir ~/tmp_wp
cp -v /mnt/c/Users/mghao/Downloads/app_beta-vtvalphen-nl_VTV-Alphen--BETA-_2025-08-28_20-22-38.tar.gz ~/tmp_wp
cd ~/tmp_wp
tar -xf app_beta-vtvalphen-nl_VTV-Alphen--BETA-_2025-08-28_20-22-38.tar.gz 
mv ~/tmp_wp/wp-content/ ~/projects/vtvalphen-wp-3/site/
```

Bring it back up

```sh
cd docker
docker compose up -d
```

## Restore database

Bash into the database container:

```sh
docker exec -it vtv-db-1 bash
```

Log into the database:

```sh
mysql --binary-mode -u wp -p wp
```

And within the session:

```
SET NAMES utf8mb4; 
SOURCE /tmp_wp/APP-DATA.SQL
UPDATE wp_options SET option_value = 'http://localhost:8080' WHERE option_name = 'siteurl';
UPDATE wp_options SET option_value = 'http://localhost:8080' WHERE option_name = 'home';
```

Go back out.

Go to wordpress:

* http://localhost:8080/