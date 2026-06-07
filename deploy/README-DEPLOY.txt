PhotoFrame deployment package (Linux) — Java 21 / main branch
===========================================================

Requirements: Java 21. Copy java.env.template to java.env and set JAVA_BIN to your JDK 21 java binary.

For Synology NAS (Java 11), build and deploy from git branch java11
(archive photoframe-deploy-1.0.0-java11.tar.gz), not this package.

Logging (same system as BookForge):
  logs/backend/photoframe.log   — main log
  logs/backend/error.log        — errors only
  logs/backend/api.log          — HTTP requests
  logs/backend/debug.log        — debug detail
  logs/archive/                 — rotated files
  logs/nohup.stdout.log         — console capture when using start-background.sh

1. Extract (IMPORTANT — parent directory, not inside photoframe):

     cd /volume1/apps
     tar xzf photoframe-deploy-2.0.0.tar.gz
     cd photoframe
     ls -la start-background.sh run.sh stop.sh

   Wrong:  cd /volume1/apps/photoframe && tar xzf ...
           (creates photoframe/photoframe/ — scripts hidden one level down)

   Updating in place: extract to a temp folder, then copy files over, or:
     cd /volume1/apps && tar xzf photoframe-deploy-1.0.0.tar.gz
     cp -f photoframe/start-background.sh photoframe/stop.sh photoframe/photoframe.jar /volume1/apps/photoframe/
2. cp google-photos-credentials.properties.template google-photos-credentials.properties
3. cp java.env.template java.env
   # edit java.env — set JAVA_BIN for your host
4. ./start-background.sh

Watch logs:
  tail -f logs/backend/photoframe.log
  tail -f logs/backend/error.log

If the app stopped unexpectedly:
  grep -E "ERROR|shutdown|Exception" logs/backend/photoframe.log | tail -50

Stop: ./stop.sh

Browser: http://192.168.18.64:8082/

Do NOT use bare "java -jar ... &" in SSH without nohup — use ./start-background.sh

Default log level: TRACE (if LOG_LEVEL is unset). Optional in java.env:
  LOG_LEVEL=DEBUG   — less verbose
  LOG_LEVEL=INFO    — production

Google photo JPEG cache (storage-lib) — optional in java.env:
  PHOTOFRAME_STORAGE_PROVIDER=filesystem   (default)
  PHOTOFRAME_STORAGE_ROOT=.                (project root; relative paths resolved at startup)
  PHOTOFRAME_STORAGE_GOOGLE_PREFIX=google-photos-cache
  For Azure Blob: PHOTOFRAME_STORAGE_PROVIDER=azure, AZURE_STORAGE_CONNECTION_STRING=...,
  and set storage.azure.container-name in application.properties or equivalent env.
