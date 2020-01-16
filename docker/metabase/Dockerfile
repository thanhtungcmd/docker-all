FROM metabase/metabase:latest

VOLUME /var/www/html/metabase/db /tmp

ENV MB_DB_FILE=/tmp/metabase.db

EXPOSE 3000

ENTRYPOINT ["/app/bin/start"]