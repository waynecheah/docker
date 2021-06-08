![alt text](https://app.datadoghq.eu/static/images/logos/nginx_small.svg)
### Setup on Server
1. Add a new nginx config file
   ```sh
   nano [PATH]/nginx/conf.d/status.conf
   ````
2. Add the following config code
    ```
    server {
      listen 81;
      server_name localhost;

      access_log off;
      allow 127.0.0.1;
      deny all;

      location /nginx_status {
        # Choose your status module

        # freely available with open source NGINX
        stub_status;

        # for open source NGINX < version 1.7.5
        # stub_status on;

        # available only with NGINX Plus
        # status;

        # ensures the version information can be retrieved
        server_tokens on;
      }
    }
    ```
3. Reload the nginx server
    ```sh
    docker exec -it [CONTAINER_NAME] nginx -s reload
    ```
4. Edit conf.yaml
    ```sh
    nano /etc/datadog-agent/conf.d/nginx.d/conf.yaml
    ```
5. Parse the following config code
    ```
    instances:
      - nginx_status_url: http://localhost:81/nginx_status/

    logs:
      - type: file
        path: /home/administrator/docker-php/nginx/log/lp-access.log
        source: nginx
        tags: landing-page
      - type: file
        path: /home/administrator/docker-php/nginx/log/lp-error.log
        source: nginx
        tags: landing-page
    ```
6. Edit Datadog config
    ```sh
    nano datadog.yaml
    ```
7. Find and edit following line in the config
    ```
    ...
    logs_enabled: true
    ```
8. Restart Datadog service
    ```sh
    service datadog-agent restart
    ```
9. Check if Datadog running well
    ```sh
    datadog-agent status    
    ```
