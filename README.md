# A geoserver docker image

Based on [terrestris/tomcat](https://github.com/terrestris/docker-tomcat):

* Alpine Linux
* Java Server JRE 8 (unlimited [JCE Policy](http://docs.geoserver.org/latest/en/user/production/java.html#installing-unlimited-strength-jurisdiction-policy-files))
* Tomcat 8.5
* GeoServer
  * Native Java advanced imaging (JAI) is installed
  * (but) [JAI-EXT](http://docs.geoserver.org/stable/en/user/configuration/image_processing/index.html#jai-ext) is enabled by default
  * Marlin renderer
  * Strong cryptography enabled (Hash/Digest)
  * Default logging profile: PRODUCTION
  * Default WFS service level: Basic

## How to build?

`docker build -t {YOUR_TAG} .`

## How to quickstart?

Build the image as described above, then:

`docker run -it -p 80:8080 {YOUR_TAG}`

or if you want to start the container daemonized:

`docker run -d -p 80:8080 {YOUR_TAG}`

Check http://localhost/geoserver to see the geoserver page and login with geoserver defaults `admin:geoserver`

## How to build a specific GeoServer version?

`docker build --build-arg GS_VERSION={YOUR_VERSION} -t {YOUR_TAG} .`

## How to build with custom geoserver data?

`docker build --build-arg GS_DATA_PATH={RELATIVE_PATH_TO_YOUR_GS_DATA} .`

**Note:** The passed path **must not** be absolute! Instead, the path should be within the build context (e.g. next to the Dockerfile) and should be passed as a relative path, e.g. `GS_DATA_PATH=./my_data/`

## Can i build a specific GS version with custom data?

Yes! Just pass the `--build-arg` param twice, e.g.

`... --build-arg GS_VERSION={VERSION} --build-arg GS_DATA_PATH={PATH} ...`

## How to build with additional libs/extensions/plugins?

Put your `*.jar` files (e.g. the WPS extension) in the `additional_libs` folder and build with one of the commands from above! (They will be copied to the GeoServer `WEB-INF/lib` folder during the build.)

**Note:** Similar to the GeoServer data path from above, you can also configure the path to the additional libraries by passing the `ADDITIONAL_LIBS_PATH` argument when building:

`--build-arg ADDITIONAL_LIBS_PATH={RELATIVE_PATH_TO_YOUR_LIBS}`

## How to watch geoserver.log from host?

`docker exec -it {CONTAINER_ID} tail -f /opt/geoserver_data/logs/geoserver.log`
