## geos, proj, gdal

```bash
#!/bin/bash
wget http://download.osgeo.org/geos/geos-3.5.0.tar.bz2
tar xvjf geos-3.5.0.tar.bz2
cd geos-3.5.0
./configure --prefix $HOME
make && make install
cd ..
wget http://download.osgeo.org/proj/proj-4.8.0.tar.gz
tar xvzf proj-4.8.0.tar.gz
cd proj-4.8.0
./configure --prefix $HOME
make && make install
cd ..

wget http://download.osgeo.org/gdal/1.11.1/gdal-1.11.1.tar.gz
tar xvzf gdal-1.11.1.tar.gz
cd gdal-1.11.1
./configure --prefix $HOME --with-geosconfig=$HOME/bin/geos-config --with-pg=/p$
make && make install

```

## PostGIS

```bash
$ wget http://download.osgeo.org/postgis/source/postgis-2.1.4.tar.gz
$ tar xvzf postgis-2.1.4.tar.gz
$ cd postgis-2.1.4
$ ./configure --with-projdir=$HOME \
  --with-geosconfig=$HOME/bin/geos-config \
  --with-gdalconfig=$HOME/bin/gdal-config
$ mkdir $HOME/postgis
$ make && make install DESTDIR=$HOME/postgis REGRESS=1
$ cd ..
$ createdb template_postgis
$ sed -i 's/\$libdir\/postgis-2.1/\$HOME\/postgis\/lib\/postgis-2.1.so/g' \
  $HOME/postgis/share/contrib/postgis/postgis.sql
```

Instead of fiddling with the sql files, it would be better to set `$libdir` in the `service/postgresql/run` file

1. Create a PostGIS template

    ```bash
    $ psql -d template_postgis -f $HOME/postgis/share/contrib/postgis/postgis.sql
    ```

    This will fail. ---> ERROR with libgeos

2. Add lib path to `$HOME/service/postgresql/run`

    ```bash
    $ export LD_LIBRARY_PATH=$HOME/lib/:...
    ```

3. Restart Postgres

4. Try to create a PostGIS template a 2nd time

    ```bash
    $ psql -d template_postgis -f $HOME/postgis/share/contrib/postgis/postgis.sql
    ```

    This time it should work. --> Success


## For remote access via client

### Make certs

http://www.postgresql.org/docs/9.1/static/ssl-tcp.html

### Adapt `pg_hba.conf` and postgresql as needed
