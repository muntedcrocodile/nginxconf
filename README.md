# nginxconf


This is an example of what i use for my selfhosted nginx configuration

I have the following services as part of this configuration
 - nginx
 - duckdns
 - certbot

You can easily disable the certbot, or duckdns services by commenting them out.

Enter required tokens
```fish
cp .env.example .env
```
Then edit the .env file and add the required values.

This configuration is configured to work with authelia to act as a auth endpoint to protect server access. You can disable the authelia auth by removing the following 2 lines from the nginx configs
```yml
include /config/nginx/snippets/authelia-location.conf;
include /config/nginx/snippets/authelia-authrequest.conf;
```

Only edit the .conf files in the `data/sites-available` directory and use symlinks in the `data/sites-enabled` directory to enable/disable your configurations as required. 

Run the following line to create said symlink
```fish
ln -s ../sites-available/TARGET ./data/sites-enabled/
```

To serve and endpoint simply add the following to the nginx of your service
```yml
networks:
  proxy:
    external: true
```

and add the following to the service itself
```yml
networks:
    - proxy
```

Note: You will need to create the `proxy` network manually since is an external network references by the docker compose and not created by the docker compose

