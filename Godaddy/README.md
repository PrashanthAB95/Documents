Godaddy API Calls:


1. Create a Test API key with OTE and PROD environment:
		OTE Key Name: cctest
		key: 3mM44UZC2tRE5h_WbtCku2wPTjqiSMfoVRWoq
		secret: WbtKXCmFxoR6GxdMxGTLsH

		Prod key Name: ccprod
		key: e42ndEgx5BPX_ushGJAT6p5mhyLWL8DQwL
		secret: uspmuywEmfzoB9rXqinHY

	Domain: cccseu.com

    https://developer.godaddy.com/keys 

2. For testing we need to create a API key to OTE servers and if we are testing the API call to the production then we need to create a API key to the production server.

	OTE server URL: https://api.ote-godaddy.com 
	Production server URL : https://api.godaddy.com

https://developer.godaddy.com/getstarted 



3. Creating A record:

Example:

	curl -X PATCH https://api.godaddy.com/v1/domains/{domain}/records \
	  -H 'Authorization: sso-key xyz:xyz' \
	  -H 'Content-Type: application/json' \
	  --data '[{"type": "A","name": "blnk1","data": "192.1.2.3","ttl": 3600}]'

Prod test:

i. PATCH ( to create a New record)

      curl -X PATCH https://api.godaddy.com/v1/domains/cccseu.com/records -H 'Authorization: sso-key e42ndEgx5BPX_ushGJAT6p5mhyLWL8DQwL:uspmuywEmfzoB9rXqinHY' -H 'Content-Type: application/json' --data '[{"type": "A","name": "example","data": "178.128.35.107","ttl": 3600}]'
			

Respons: 
	For success ( no response, the cmd executes with blank) 

Attributes:
type: 	 type of record ( A record or Cname etc)
	name:   name of the subdomain to be created
	data: 	 IP it wants to be pointed to.
	ttl: 	 Also note that the optional ttl field needs to be at least 600.

ii. If trying to create same record twice:

Error Respons:
	   {"code":"DUPLICATE_RECORD","fields":[{"code":"DUPLICATE_RECORD","message":"CNAME record name [example] conflicts with another record.","path":"records"}],"message":"Another record with the same attributes already exists","errors":["CNAME record name [optit.devops] conflicts with another record."]}


ii. Get the list of Records:
  	
  	curl -X GET https://api.godaddy.com/v1/domains/cccseu.com/records -H 'Authorization: sso-key e42ndEgx5BPX_ushGJAT6p5mhyLWL8DQwL:uspmuywEmfzoB9rXqinHY' -H 'Content-Type: application/json'


iii. Delete a record: ( not working )
		
		We cannot delete a record using the API calls in Godaddy.
		https://in.godaddy.com/community/Managing-Domains/Deleting-a-single-DNS-record-through-the-API/td-p/108003  


Wild card:

	Wildcard subdomain allows you to point all non-existing subdomains to a specific folder or a Webpage in your account. It means that if you enter different subdomains (which are not created in your cPanel (Dashboard)) in your browser, they all will show the same content that you uploaded to the folder set for the wildcard subdomain.


	When you do so, if the subdomain queried does not exist, the server will respond with the IP address specified in the Zone File Editor as the wildcard.

	Example: *.cccseu.com

Sample example:  
DNS: optit.devops.cccseu.com

If we enter the DNS “optit.devops.cccseu.com” which isn’t in the Record list then it will redirect us to a static Webpage or a Redirecting URL which ever we have configured.
