PhotoFrame deployment package (Linux)
=====================================

Requirements: Java 11 or newer.

Default JDK: /volume1/java/current/bin/java (edit java.env if needed)

1. Extract:
     tar xzf photoframe-deploy-*.tar.gz
     cd photoframe

2. Configure Google Photos:
     cp google-photos-credentials.properties.template google-photos-credentials.properties
     # edit credentials

3. Start (survives SSH disconnect):
     ./start-background.sh
     tail -f photoframe.log

   Foreground (stops when SSH session ends):
     ./run.sh

4. Browser (use host IP, e.g. 192.168.18.64):
     http://192.168.18.64:8082/
     http://192.168.18.64:8082/myphotoframe.html

Stop: ./stop.sh

If slideshow freezes: check ps | grep photoframe.jar and tail photoframe.log
Use start-background.sh, not bare java -jar in SSH.
