# Steps For Creating the Elasticsearch inside the Container and Applying the SSL.
    1. Create a Docker Network
			To create the Docker Network, command is [ docker network create docker ]
    2. Run the Elasticsearch as a Container
    		To run the elasticsearch as a conatiner, command is [ docker run -d --name elasticsearch --net docker -p 9200:9200 -v /var/lib/elasticsearch/data:/var/lib/elasticsearch -p 9300:9300 -e "discovery.type=single-node" elasticsearch:6.5.4 ]
    3. Now the elasticsearch will successfully running inside the container
    4. Apply SSL for the Elasticsearch
			To run the Nginx as conatiner, command is [ docker run --name nginx --net docker -p 80:80 -p 443:443 --link elasticsearch -t -d justops/nginx ]
    5. docker exec -it <Container ID of Nginx> bash
    6. cd conf.d
    7. Create a file as www.devops.example.com in conf.d
    8. Put below content in www.devops.example.com file
    		server {
    listen 80 default_server;
    listen [::]:80 default_server;
    root /etc/nginx;
    server_name devops.cccsuk.co.uk www.devops.example.com;
        }
    
    9. cd ..
    10. Rename or delete the existing nginx.conf file in the root directory (/etc/nginx)
    11. Create a new file (nginx.conf) under /etc/nginx directory
    12. Put below content in nginx.conf
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
     
    13. cd sites-available
    14. vim iamcj
    15. Erase the content in iamcj and paste below content in it
     		server {
    listen          80;
    server_name     devops.cccsuk.co.uk www.devops.example.com;
    location / {
        proxy_pass  http://206.189.21.169:3000/;
    }
    }
    
    16. Execute the elow commands in terminal
    17. nginx -t && nginx -s reload
    18. add-apt-repository ppa:certbot/certbot
    19. apt-get update -y
    20. apt-get install python-certbot-nginx -y
    21. certbot --nginx -d devops.example.com -d www.devops.example.com
    22. Fill the email id and re-derect all the URL’s.
