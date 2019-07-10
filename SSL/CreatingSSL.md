# Steps: (for creating the SSL)


	1. Pull the justops/nginx image and run as the conatainer, Run this docker command in terminal [ docker run --name nginx -p 80:80 -p 443:443 --link <container name>:<image name> -t -d justops/nginx ]
	2. docker exec -it <container ID> bash
	3. cd conf.d
	4. Create a file as www.devops.example.com in conf.d
	5. Put below content in www.devops.example.com file
			server {
    listen 80 default_server;
    listen [::]:80 default_server;
    root /etc/nginx;
    server_name devops.cccsuk.co.uk www.devops.example.com;
	}
    
    6. cd ..
    7. Rename or delete the existing nginx.conf file in the root directory (/etc/nginx)
    8. Create a new file (nginx.conf) under /etc/nginx directory
    9. Put below content in nginx.conf
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
    
    10. cd sites-available
    11. vim iamcj
    12. Erase the content in iamcj and paste below content in it
    		server {
    listen          80;
    server_name     devops.cccsuk.co.uk www.devops.example.com;
    location / {
        proxy_pass  http://206.189.21.169:3000/;
    }
    }
    
    13. Execute the elow commands in terminal
    14. nginx -t && nginx -s reload
    15. add-apt-repository ppa:certbot/certbot
    16. apt-get update -y
    17. apt-get install python-certbot-nginx -y
    18. certbot --nginx -d devops.example.com -d www.devops.example.com
    19. Fill the email id and re-derect all the URL’s.
