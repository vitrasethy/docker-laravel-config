# docker-laravel-config
- copy all files except readme to laravel directory
- create .env file if you don't have
- add database info into .env. The last three can be anything you want
- DB_CONNECTION=pgsql
  DB_HOST=laravel-db
  DB_PORT=5432
  DB_DATABASE=docker_learn
  DB_USERNAME=laravel
  DB_PASSWORD=password
- run "docker-compose build app"
- run "docker-compose up -d"
- run "docker exec -it <container_id> sh"
- run "php artisan migrate"
- end
