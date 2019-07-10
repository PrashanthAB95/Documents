# Steps For Minio Setup With SSL:

	1. Run the Minio as a Container.
			To run the Minio as a container, command is [ docker run -p 9000:9000 --name minio -v /var/minio/data:/data -v /var/minio/config:/root/.minio -e MINIO_ACCESS_KEY=admin -e MINIO_SECRET_KEY=admin123 -d minio/minio server /data ]
    2. Then for SSL purpose Run the Nginx in container.
    		To run the nginx as a container, command is [ docker run --name nginx --network keycloak --link keycloak:jboss/keycloak -p 80:80 -p 443:443 -t -d justops/nginx ]
    3. Then Execute the below command.
    4. docker exec -it <Container ID of Nginx> bash
    5. cd conf.d
    6. Create a file as www.devops.example.com in conf.d
    7. Put below content in www.devops.example.com file
    		server {
    listen 80 default_server;
    listen [::]:80 default_server;
    root /etc/nginx;
    server_name devops.cccsuk.co.uk www.devops.example.com;
        }
        
    8. cd ..
    9. Rename or delete the existing nginx.conf file in the root directory (/etc/nginx)
    10. Create a new file (nginx.conf) under /etc/nginx directory
    11. Put below content in nginx.conf
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
        
    12. cd sites-available
    13. vim iamcj
    14. Erase the content in iamcj and paste below content in it
    		server {
    listen          80;
    server_name     devops.cccsuk.co.uk www.devops.example.com;
    location / {
        proxy_pass  http://206.189.21.169:3000/;
    }
    }
    
    15. Execute the elow commands in terminal
    16. nginx -t && nginx -s reload
    17. add-apt-repository ppa:certbot/certbot
    18. apt-get update -y
    19. apt-get install python-certbot-nginx -y
    20. certbot --nginx -d devops.example.com -d www.devops.example.com
    21. Fill the email id and re-derect all the URL’s.

	
	
