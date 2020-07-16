## UFANISI COURIER SERVICES  API REFERENCE
-API reference for Ufanisi Courier Services that enables vendors integrate courier tracking services in their website &amp; app powered by ufanisi.
 
The purpose of this API is to help you as a merchant and our customer  in offering & automating shipment tracking service to your customers  in your website.
Once you contract/ partner with  us as your courier service provider, we give you access to our API that enables you to register shipments with us as well as  request all your shipment details from our service to your website or mobile app. 
The API can be used  by:-
    1) E-commerce services e.g Online Shops & Marketplace,
    2) Other Courier Service Providers To Forward shipment to Ufanisi.
 

## FOR DEVELOPERS
- Before using the Ufanisi API, you need to register on the Ufanisi website to get an API key to use in your API calls. 
- Please register at:-  https://api.ufanisicourier.co.ke/ .  After registering, you will be able to add your website's domain and get an APIkey. 
  The APIkey  is a unique code generated when you register your domain for our API usage. 
- The Ufanisi API is built around REST principles. Use POST requests to create objects requests, GET requests to retrieve objects;

## API ENDPOINT
Endpoint for all API  calls will be. https://api.ufanisicourier.co.ke/live/ replace {live} with sandbox if you’re in testing mode.

## API REQUESTS 
Request data is passed to the API by POSTing JSON objects with the appropriate key/value-pairs to the endpoint i.e https://api.ufanisicourier.co.ke/live/  (as we will be showing you below).

## API RESPONSE 
The API will return responses in JSON format for any success or failed requests.

## API RESOURCES.
Here is  list of what our API can do.
   - Register / forward Shipments
   - Get Shipments
   - Cancel Shipment*
   - Edit Shipment*
   - Track Shipment
   - Get Shipment Rates
   - Get Destinations
   - Webhooks 


## THE CODE

- Examples here use php / cURL, you can use your preferred programming language as long as you use the correct endpoint and api key for your request .
- if you're using php and cURL below is cURL function to handle all requests

```php
  - call this function to process any request to our endpoint 
  - for javascript & other languages, just pass the apikey and required parameters   using POST method.
  - apikey is provided when you register for this service,

    function ProcessRequest($data, $headers, $url)
    {
        $curl = curl_init();
        curl_setopt($curl, CURLOPT_URL, $url);
        curl_setopt($curl, CURLOPT_HTTPHEADER, $headers);
        $data = json_encode($data);
        curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
        curl_setopt($curl, CURLOPT_POST, true);
        curl_setopt($curl, CURLOPT_POSTFIELDS, $data);
        curl_setopt($curl, CURLOPT_HEADER, false);
        curl_setopt($curl, CURLOPT_SSL_VERIFYPEER, false);
        $response = curl_exec($curl);
        return $response;  
    }
```

## Register Shipment 
-Below is an example showing how to register a shipment request, put ZERO (0) if value not available.

```php
     function  createShipment() 
     {
            $data= array();
            // SENDER DETAILS
            $data['sendername']="", 
            $data['senderemail']="", 
            $data['sendertel']="", 
            $data['sendercountry']="", 
            $data['sendertown']="", 
            $data['senderstreet']="", 
            $data['senderbuilding']="", 
            $data['senderroom']="", 
            // RECEIVER DETAILS 
            $data['receivername']="", // full names
            $data['receiveremail']="",  // 
            $data['receivertel']="",  // phone number
            $data['receivercountry']="", // *
            $data['receivertown']="", // must 
            $data['receiverstreet']="",  // *
            $data['receiverbuilding']="", // *
            $data['receiverroom']="",  // * optional

            // DELIVERY DETAILS
            $data['deliverydate']="", // Y-m-d 
            $data['receipientname']="",// can be receivername 
            // SHIPMENT DETAILS
            $data['orderid'] = "", // your order id for reference
            $data['origin']="", 
            $data['destination']="", 
            $data['service']="", // options are overnight, sameday, international, ecommerce
            $data['weight']="", // in cm, put 0 if none;
            $data['length']="", // in cm, put 0 if none;
            $data['width']="",  // in cm, put 0 if none;
            $data['height']="", // in cm, put 0 if none;
            $data['numofpackages']="", // number
            $data['description']="", // extra request    
         // make request using cURL. For other languages make request to  url  in using POST method , 
            $headers = ['Content-Type:application/json; charset=utf8'];
            $url = 'https://api.ufanisicourier.co.ke/createshipment?apikey={{yourapikey}}';   
            $response  = $this->ProcessRequest($data, $headers, $url);
            return $response; 
        }
 ```
 ## Register Shipment Response
 - Receive a response with shipmentID and status. Status might be pending or confirmed. You can save this in your database if necessary.
``` json 
        {
        "status":1,// 1 for success 0 for failed;
        "data": {
                "shipmentid":"", // also known as waybillnumber
                "status":"pending", // pending approval; 
                "message":"Registered Successfully"//
            }
        }
```

## Cancel Shipment
- You can cancel shipment using this method sending your request with  shipmentid,orderid
``` php
    function cancelShipment(){
            $data= array();
            $data['shipmentid']="";  // also known as waybillnumber
            $data['orderid']=""; // * 
            // make request using cURL. For other languages make request to  url  in using POST method , 
            // 
            $headers = ['Content-Type:application/json; charset=utf8'];
            $url = 'https://api.ufanisicourier.co.ke/cancelshipment?apikey={{yourapikey}}';   
            $response  = $this->ProcessRequest($data, $headers, $url);
            return $response; 
    }
```
 ## Cancel Shipment Response
 - This is the response you get when you cancel a shipment;
``` json 
        {
        "status":1,// 1 for success 0 for failed;
        "data": {
                "shipmentid":"", // also known as waybillnumber
                "status":"canceled", // pending approval;
                "message":"Cancelled Successfully"
            }
        }
```

## Track Shipment Request
- Track a shipment using this method.  Pass shipment id to each request;
``` php
    function trackShipment(){
            $data= array();
            $data['shipmentid']="";  // also known as waybillnumber
            // make request using cURL. For other languages make request to  url  in using POST method , 
            $headers = ['Content-Type:application/json; charset=utf8'];
            $url = 'https://api.ufanisicourier.co.ke/trackshipment?apikey={{yourapikey}}';   
            $response  = $this->ProcessRequest($data, $headers, $url);
            return $response; 
    }
```

 ## Track Shipment Response
 - Here is the response you get when you send a tracking request for a shipment.
 - You can use this method in your customer tracking interface to query directly from the api. 
``` json 
        {
        "status":1,// 1 for success 0 for failed;
        "data": { 
                "shipmentid":"", // also known as waybillnumber
                "deliverystatus":"", // eg pending approval, packaging, transit, delivered
                // sender details
                "sendername":"",  
                "senderemail":"",  
                "sendertel":"",  
                "sendercountry":"",  
                "sendertown":"",  
                "senderstreet":"",  
                "senderbuilding":"",  
                "senderroomnumber":"",
                // receiver details
                "receivername":"",  
                "receiveremail":"",  
                "receivertel":"",  
                "receivercountry":"",  
                "receivertown":"",  
                "receiverstreet":"",  
                "receiverbuilding":"",  
                "receiverroomnumber":"",  
                // shipment details
                "shippedvia":"",
                "receipientname":"",
                "idnumber":"",
                "deliverydate":"", 
                "origin":"",  
                "destination":"",  
                "service":"",   
                "actualweight":"",  
                "volumetricweight":"",
                "length":"",  
                "width":"",  
                "height":"",   
                "numofpackages":"",  
                "declaredvalue":"",  
                "date":"",  
                "issuedstatus":"",  
                "description":"",
                "dangerous":"", // yes or no
                "amountreceived":"",
                "receivedas":"",  
                "deliverynote":"",
            }
        }
```

## Get All Shipments Request
- Request for all shipments. You can set maximum number of responses and order by given values.
``` php
    function getShipments(){
            $data= array();
            $data['counts']="10"; //  set limit, max 100  default 10
            $data['orderby']=""; //  order by  dateadded, delierydate,destination,status, origin,  from latest ,  default = dateadded
            // make request using cURL. For other languages make request to  url  in using POST method , 
            $headers = ['Content-Type:application/json; charset=utf8'];
            $url = 'https://api.ufanisicourier.co.ke/getshipments?apikey={{yourapikey}}';   
            $response  = $this->ProcessRequest($data, $headers, $url);
            return $response; 
    }
```

 ## Get All Shipments Response
 - the response you get when you request all shipments as an array.
``` json 
//  return al list of shipments associated with your API key / Domain.
        {
        "status":1,// 1 for success 0 for failed;
        "data": [{  
                "shipmentid":"", // also known as waybillnumber
                "deliverystatus":"", // eg pending approval, packaging, transit, delivered
                // sender details
                "sendername":"",  
                "senderemail":"",  
                "sendertel":"",  
                "sendercountry":"",  
                "sendertown":"",  
                "senderstreet":"",  
                "senderbuilding":"",  
                "senderroomnumber":"",
                // receiver details
                "receivername":"",  
                "receiveremail":"",  
                "receivertel":"",  
                "receivercountry":"",  
                "receivertown":"",  
                "receiverstreet":"",  
                "receiverbuilding":"",  
                "receiverroomnumber":"",  
                // shipment details
                "shippedvia":"",
                "receipientname":"",
                "idnumber":"",
                "deliverydate":"", 
                "origin":"",  
                "destination":"",  
                "service":"",   
                "actualweight":"",  
                "volumetricweight":"",
                "length":"",  
                "width":"",  
                "height":"",   
                "numofpackages":"",  
                "declaredvalue":"",  
                "date":"",  
                "issuedstatus":"",  
                "description":"",
                "dangerous":"", // yes or no
                "amountreceived":"",
                "receivedas":"",  
                "deliverynote":"",
            }]
        }
```



## Get Prices List Shipments Request
- Get ufanisi courier services price list based on orgin and destination.
``` php
    function getPricelist(){
            $data= array();
            $data['origin']=""; // required 
            $data['destination']=""; 
            // make request using cURL. For other languages make request to  url  in using POST method , 
            $headers = ['Content-Type:application/json; charset=utf8'];
            $url = 'https://api.ufanisicourier.co.ke/getpricelist?apikey={{yourapikey}}';   
            $response  = $this->ProcessRequest($data, $headers, $url);
            return $response; 
    }
```

 ## Get  priceList  Response
 - price list response 
``` json 
        {
        "status":1,// 1 for success 0 for failed;
        "data": {
                "origin":"",// 
                "destination":"",  
                "pricelist":[{
                    "currency":"KSH",
                    "amount":"1000",
                    "duration_terms":"Delivery within 1, 2, or 3 days based on where your package started and where it’s being sent.",
                }]
            }
        }
```

## Get Destinations Request
- Request all destinations that ufanisi courier services serve.
``` php
    function getPricelist(){
            $data= array();
            $data['orderby']=""; // default name; 
             // make request using cURL. For other languages make request to  url  in using POST method , 
            $headers = ['Content-Type:application/json; charset=utf8'];
            $url = 'https://api.ufanisicourier.co.ke/getpricelist?apikey={{yourapikey}}';   
            $response  = $this->ProcessRequest($data, $headers, $url);
            return $response; 
    }
```

 ## Get  Destinations  Response
 - Destinations Response 
``` json 
        {
        "status":1,// 1 for success 0 for failed;
        "data": [{
                    "name":"",
                    "country":"",
                 }] 
        }
```
## WEBHOOKS
- Register a tracking webhook.
- Webhooks typically are used to connect two different applications. When an event happens on the trigger application, it serializes data about that   event and sends it to a webhook URL from the action application—the one you want to do something based on the data from the first application. 

- You can register your webhook(s) for a Shipments updates, this way, UFANISI  will send HTTP notifications to your provided url  whenever the status changes.

- Your webhook url should be able to receive the data and update your database at anytime or even perform other function like sending SMS & Email notifictions with your own implementation.
- Login to your account in https://api.ufanis.co.ke and add your callback url under webhook tab.

 ## Webhook  Response
 - You can save this callback response in your database 
 - This response is only sent when ufanisi make a change in your submitted shipments eg Shipment status update eg pending, approved, packaging , transist, delivered etc. 
``` json 
        {
        "status":1,// 1 for success 0 for failed;
        "data": {
                "shipmentid":"", // also known as waybillnumber
                "deliverystatus":"", // eg pending approval, packaging, transit, delivered
                // sender details
                "sendername":"",  
                "senderemail":"",  
                "sendertel":"",  
                "sendercountry":"",  
                "sendertown":"",  
                "senderstreet":"",  
                "senderbuilding":"",  
                "senderroomnumber":"",
                // receiver details
                "receivername":"",  
                "receiveremail":"",  
                "receivertel":"",  
                "receivercountry":"",  
                "receivertown":"",  
                "receiverstreet":"",  
                "receiverbuilding":"",  
                "receiverroomnumber":"",  
                // shipment details
                "shippedvia":"",
                "receipientname":"",
                "idnumber":"",
                "deliverydate":"", 
                "origin":"",  
                "destination":"",  
                "service":"",   
                "actualweight":"",  
                "volumetricweight":"",
                "length":"",  
                "width":"",  
                "height":"",   
                "numofpackages":"",  
                "declaredvalue":"",  
                "date":"",  
                "issuedstatus":"",  
                "description":"",
                "dangerous":"", // yes or no
                "amountreceived":"",
                "receivedas":"",  
                "deliverynote":"",
            } 
        }
```


# API USAGE DEMO.
- Check our demo online shop on how the api can be used at https://apidemo.ufanisicourier.co.ke/

 

