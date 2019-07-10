# Steps For Keycloak Server Setup:

    1. Create a Docker Network
			To create the Docker Network, command is [ docker network create keycloak ]
    2. Run the Postgres as Container.
    		To run the postgres as a container, command is [ docker run -d --name postgres --net keycloak -e POSTGRES_DB=keycloak -e POSTGRES_USER=keycloak -e POSTGRES_PASSWORD=password -p 5432:5432 -v /var/lib/postgresql/data/pgdata:/var/lib/postgresql/data -d postgres ]
    3.Now Run the Keycloak as a Container.
    		To run the keycloak as a container, command is [ docker run --name keycloak --network keycloak --link postgres:postgres -p 8080:8080 -e KEYCLOAK_USER=admin -e KEYCLOAK_PASSWORD=admin -e PROXY_ADDRESS_FORWARDING=true -t -d jboss/keycloak ]
    4. Then for SSL purpose Run the Nginx in container.
    		To run the nginx as a container, command is [ docker run --name nginx --network keycloak --link keycloak:jboss/keycloak -p 80:80 -p 443:443 -t -d justops/nginx ]
    5. Then Execute the below command.
    6. docker exec -it <Container ID of Nginx> bash
    7. cd conf.d
    8. Create a file as www.devops.example.com in conf.d
    9. Put below content in www.devops.example.com file
    		server {
    listen 80 default_server;
    listen [::]:80 default_server;
    root /etc/nginx;
    server_name devops.cccsuk.co.uk www.devops.example.com;
        }
    
    10. cd ..
    11. Rename or delete the existing nginx.conf file in the root directory (/etc/nginx)
    12. Create a new file (nginx.conf) under /etc/nginx directory
    13. Put below content in nginx.conf
    		events {
        worker_connections 768;
        # multi_accept on;
        }
        http {
        upstream frotend {
    server 206.189.21.169:3000;
        }

        server {
        server_name devops.eample.com www.devops.eample.com;

        location / {
            proxy_pass         http://206.189.21.169:3000/;
            proxy_redirect     off;
            proxy_set_header   Host $host;
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header   X-Forwarded-Host $server_name;

        }
        }
        }
        
    14. cd sites-available
    15. vim iamcj
    16. Erase the content in iamcj and paste below content in it
    		server {
    listen          80;
    server_name     devops.cccsuk.co.uk www.devops.example.com;
    location / {
        proxy_pass  http://206.189.21.169:3000/;
    }
    }
    
    17. Execute the elow commands in terminal
    18. nginx -t && nginx -s reload
    19. add-apt-repository ppa:certbot/certbot
    20. apt-get update -y
    21. apt-get install python-certbot-nginx -y
    22. certbot --nginx -d devops.example.com -d www.devops.example.com
    23. Fill the email id and re-derect all the URL’s.
	
