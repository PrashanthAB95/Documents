# Steps For Trudesk Setup With SSL:


	1. For Trudesk Setup we need to do some Prerequisites
	2. Open the terminal, then create Five Directories by executing the below commands.
			mkdir -p /data/db
            mkdir -p /data/configdb
            mkdir -p /data/trudesk/plugins
            mkdir -p /data/trudesk/uploads
            mkdir -p /data/trudesk/backups
    3. Run the Mongodb as Container.
    		To run the Mongodb as a container, command is
            docker run --name mongodb \
            --net docker \
            -v /data/db:/data/db \
            -v /data/configdb:/data/configdb \
            -d mongo:3.6
    4. Then for Trudesk in a container.
    		To run the Trudesk as a container, command is
            docker run --name trudesk --net docker --link mongodb:mongodb \
            -v /data/trudesk/uploads:/usr/src/trudesk/public/uploads \
            -v /data/trudesk/plugins:/usr/src/trudesk/plugins \
            -v /data/trudesk/backups:/usr/src/trudesk/backups \
            -e NODE_ENV=production \
            -e MONGODB_PORT_27017_TCP_ADDR=mongodb -e MONGODB_DATABASE_NAME=trudesk \
            -p 8081:8118 \
            -d polonel/trudesk:1.0.6
            
    5. Then for SSL purpose Run the Nginx in container.
    		To run the nginx as a container, command is [ docker run --name nginx --network keycloak --link keycloak:jboss/keycloak -p 80:80 -p 443:443 -t -d justops/nginx ]
    6. Then Execute the below command.
    7. docker exec -it <Container ID of Nginx> bash
    8. cd conf.d
    9. Create a file as www.devops.example.com in conf.d
    10. Put below content in www.devops.example.com file
    		server {
    listen 80 default_server;
    listen [::]:80 default_server;
    root /etc/nginx;
    server_name devops.cccsuk.co.uk www.devops.example.com;
        }
        
    11. cd ..
    12. Rename or delete the existing nginx.conf file in the root directory (/etc/nginx)
    13. Create a new file (nginx.conf) under /etc/nginx directory
    14. Put below content in nginx.conf
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
        
    15. cd sites-available
    16. vim iamcj
    17. Erase the content in iamcj and paste below content in it
     		server {
    listen          80;
    server_name     devops.cccsuk.co.uk www.devops.example.com;
    location / {
        proxy_pass  http://206.189.21.169:3000/;
    	}
    }
    
    18. Execute the elow commands in terminal
    19. nginx -t && nginx -s reload
    20. add-apt-repository ppa:certbot/certbot
    21. apt-get update -y
    22. apt-get install python-certbot-nginx -y
    23. certbot --nginx -d devops.example.com -d www.devops.example.com
    24. Fill the email id and re-derect all the URL’s.
