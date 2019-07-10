# Steps For Creating the Uptime For CentOS (URL Monitoring)


    1. Run the Uptime as a Container
			To run the Uptime as a container, command is [ docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -it --name elk sebp/elk ]
    2. Now the ELK will run successfully as a container, we can access ELK in the browser.
    3. In the server we need to install the heartbeat to monitor the URL.
    4. Execute and follow the below steps:
    		sudo rpm --import https://packages.elastic.co/GPG-KEY-elasticsearch
            
            sudo vi /etc/yum.repos.d/elastic.repo
            
            Paste the below content in the above mentioned path
            	[elastic-7.x]
                name=Elastic repository for 7.x packages
                baseurl=https://artifacts.elastic.co/packages/7.x/yum
                gpgcheck=1
                gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
                enabled=1
                autorefresh=1
                type=rpm-md
                
            sudo yum install -y heartbeat-elastic
            
            sudo chkconfig --add heartbeat-elastic
            
            sudo systemctl enable heartbeat-elastic
            
            sudo systemctl start heartbeat-elastic
            
    5. Finally, upadte the URL's in the heartbeat.yml in the downloaded heartbeat package.
