###geos


`wget wget http://download.osgeo.org/geos/geos-3.4.2.tar.gz`

`tar xvzf geos-3.4.2.tar.gz`

`cd geos-3.4.2`


`./configure --prefix $HOME`

` make &&make install`

`cd ..`
##proj

`wget http://download.osgeo.org/proj/proj-4.8.0.tar.gz`

`tar xvzf proj-4.8.0.tar.gz`

`cd proj-4.8.0`


`./configure --prefix $HOME`

`make && make install`

`cd ..`
###gdal

 wget http://download.osgeo.org/gdal/1.11.1/gdal-1.11.1.tar.gz

tar xvazf  gdal-1.11.1.tar.gz

cd  gdal-1.11.1

`./configure --prefix $HOME --with-geosconfig=$HOME/bin/geos-config  --with-pg=/package/host/localhost/postgresql-9.2.6-1/bin/pg_config`

`make && make install`

`cd ..`
###postgis

`wget http://download.osgeo.org/postgis/source/postgis-2.1.4.tar.gz`

`tar xvzf postgis-2.1.4.tar.gz`

`cd postgis-2.1.4`

`./configure --with-projdir=$HOME --with-geosconfig=$HOME/bin/geos-config  --with-gdalconfig=$HOME/bin/gdal-config`

`mkdir $HOME/postgis`

`make && make install DESTDIR=$HOME/postgis REGRESS=1`

`cd ..`

`createdb template_postgis`

`sed -i 's/\$libdir/$HOME/postgis\/lib\/postgis-2.1.so/g' HOME/postgis/share/contrib/postgis/postgis.sql`
Instead of fiddling with the sql files, it would be better to set $libdir in the service/postgresql/run file

`psql -d template_postgis -f $HOME/postgis/share/contrib/postgis/postgis.sql`

---> ERROR with libgeos

###add lib path to $HOME/service/postgresql/run

export LD_LIBRARY_PATH=$HOME/lib/:...



###restart Postgres

###2nd try:

`psql -d template_postgis -f $HOME/postgis/share/contrib/postgis/postgis.sql`

--> Success

####forremote acces via client

### Make certs

http://www.postgresql.org/docs/9.1/static/ssl-tcp.html

### Adapt pg_hba.conf and postgresql as needed



