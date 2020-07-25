## UFANISI COURIER SERVICES  API REFERENCE
API reference for Ufanisi Courier Services that enables vendors integrate courier tracking services in their websites &amp; or apps powered by ufanisi.
 
The purpose of this API is to help you as a merchant and our customer  in offering & automating shipment tracking service to your customers  in your website.
Once you contract or partner with  us as your courier service provider, we give you access to our API that enables you to register shipments with us as well as  request all your shipment details from our service to your website or mobile app. 
### The API can be used  by
- E-commerce services e.g Online Shops & Marketplace,
- Other Courier Service Providers To Forward shipment to Ufanisi.


## FOR DEVELOPERS
- Before using the Ufanisi API, you need to register on the Ufanisi website to get an API key to use in your API calls. 
- Please register at:-  https://ufanisicourier.co.ke/developer/ .  After registering, you will be able to add your website's domain and get an APIkey. 
  The APIkey  is a unique code generated when you register your domain for our API usage. 
- The Ufanisi API is built around REST principles. Use POST requests to create objects requests, GET requests to retrieve objects;

## API ENDPOINT
Endpoint for all API  calls will be. https://ufanisicourier.co.ke/api/v1/api/

## API REQUESTS 
Request data is passed to the API by POSTing JSON objects with the appropriate key/value-pairs to the endpoint i.e https://ufanisicourier.co.ke/api/v1/api/ (as we will be showing you below).

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
- Quick JS implementation to get all shipments; replace   with your apikey.
```js 
   $(function(){ 
            let params = JSON.stringify({
                counts:10,
                orderby:'date'
            }); 
            $.ajax({
                url: 'https://liniacle.com/ufanisi/api/api/getshipments?apikey=ece58bc26c1577ad71411afbad9fa6d2',
                data: params,
                dataType: "JSON",
                processData: false,
                contentType: false,
                type: "POST",
            success: function(res) { 
                $("#response").text(JSON.stringify(res));
                console.log(res);
            }
        });   
   }); 

```

- Other examples here use php / cURL, you can use your preferred programming language as long as you use the correct endpoint and api key for your request .
- if you're using php and cURL, below is cURL function to handle all requests
  
```php 
    function ProcessRequest($data, $url)
    {
            $mainData=json_encode($data); 
            $ch = curl_init($url);
            curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
            curl_setopt($ch, CURLOPT_POSTFIELDS,$mainData);
            curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
            curl_setopt($ch, CURLOPT_HTTPHEADER, array(
                 'Content-Type: application/json',
                 'Content-Length: ' . strlen($mainData))
            ); 
            $error = curl_error($ch);
            $result = curl_exec($ch);
            curl_close($ch);
            if($error){
                return   $error;
            }
            return   $result;
    }
```
## Register Shipment 
- Below is an example showing how to register a shipment request, put ZERO (0) if value not available.

```php
     function  createShipment() 
      {
             $data= array();
             // SENDER DETAILS
             $data['sendername']="testsender";
             $data['senderemail']="test@gmail.com";
             $data['sendertel']="0704904401";
             $data['sendercountry']="Kenya";
             $data['sendertown']="Nairobi";
             $data['senderstreet']="0";
             $data['senderbuilding']="0";
             $data['senderroom']="0";
             // RECEIVER DETAILS 
             $data['receivername']="testreceiver";// full names
             $data['receiveremail']="testreceiver@gmail.com"; // 
             $data['receivertel']="254704904401"; // phone number
             $data['receivercountry']="Kenya";// *
             $data['receivertown']="Mombasa";// must 
             $data['receiverstreet']="0"; // *
             $data['receiverbuilding']="0";// *
             $data['receiverroom']="0"; // * optional
 
             // DELIVERY DETAILS
             $data['deliverydate']=date("Y-m-d");// Y-m-d 
             $data['recipientname']="testreceiver";// can be receivername 
             $data['idnumber']="";// id number of sender optional
             // SHIPMENT DETAILS
             $data['orderid'] = "1001";// your order id for reference
             $data['origin']="Nairobi";
             $data['destination']="Mombasa";
             $data['service']="sameday";// options are overnight;sameday;international;ecommerce
             $data['weight']="0.5";// in kg;put 0 if none;
             $data['length']="100";// in cm;put 0 if none;
             $data['width']="200"; // in cm;put 0 if none;
             $data['height']="200";// in cm;put 0 if none;
             $data['numofpackages']="2";// number
             $data['isdangerous']="no";// yes/no;
             $data['description']="Deliver to Bamburi Mwembeni Stage. Call 257282278 and will pickup. Call before delivering";// extra request    
          // make request using cURL. For other languages make request to  url  in using POST method , 
              $url = 'https://liniacle.com/ufanisi/api/api/createshipment?apikey=ece58bc26c1577ad71411afbad9fa6d2';   
             $response  = ProcessRequest($data, $url);
             return $response; 
         }
 ```

 ## Register Shipment Response
 - Receive a response with shipmentID and status. Status might be pending or confirmed. You can save this in your database if necessary.

```json 
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
- You can cancel shipment using this method by sending your request with  shipmentid,orderid

```php
    function cancelShipment(){
            $data= array();
            $data['shipmentid']="";  // also known as waybillnumber
            $data['orderid']=""; // * 
            // make request using cURL. For other languages make request to  url  in using POST method , 
            // 
            
            $url = 'https://ufanisicourier.co.ke/api/v1/api/cancelshipment?apikey={{yourapikey}}';   
            $response  = ProcessRequest($data, $url);
            return $response; 
    }
```

 ## Cancel Shipment Response
 - This is the response you get when you cancel a shipment;

```json 
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
- You can use this method in your customer tracking interface to query directly from the api. 

```php
    function trackShipment(){
            $data= array();
            $data['shipmentid']="";  // also known as waybillnumber
            // make request using cURL. For other languages make request to  url  in using POST method , 
            
            $url = 'https://ufanisicourier.co.ke/api/v1/api/trackshipment?apikey={{yourapikey}}';   
            $response  = ProcessRequest($data, $url);
            return $response; 
    }
```

 ## Track Shipment Response
 - Here is the response you get when you send a tracking request for a shipment.

```json 
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

```php
    function getShipments(){
            $data= array();
            $data['counts']="10"; //  set limit, max 100  default 10
            $data['orderby']=""; //  order by  dateadded, delierydate,destination,status, origin,  from latest ,  default = dateadded
            // make request using cURL. For other languages make request to  url  in using POST method , 
            
            $url = 'https://ufanisicourier.co.ke/api/v1/api/getshipments?apikey={{yourapikey}}';   
            $response  = ProcessRequest($data, $url);
            return $response; 
    }
```

 ## Get All Shipments Response
 - the response you get when you request all shipments as an array.
 - return al list of shipments associated with your API key / Domain.

```json 
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

```php
    function getPricelist(){
            $data= array();
            $data['origin']=""; // required 
            $data['destination']=""; 
            // make request using cURL. For other languages make request to  url  in using POST method , 
            
            $url = 'https://ufanisicourier.co.ke/api/v1/api/getpricelist?apikey={{yourapikey}}';   
            $response  = ProcessRequest($data, $url);
            return $response; 
    }
```

 ## Get  priceList  Response
 - price list response 

```json 
        {
        "status":1,// 1 for success 0 for failed;
        "data": {
                "origin":"",// 
                "destination":"",  
                "pricelist":[{
                    "currency":"KSH",
                    "amount":"1000",
                    "description":"Delivery within 1, 2, or 3 days based on where your package started and where it’s being sent.",
                }]
            }
        }
```

## Get Destinations Request
- Request all destinations that ufanisi courier services serve.

```php
    function getDestinations(){
            $data= array();
            $data['orderby']=""; // default name; 
             // make request using cURL. For other languages make request to  url  in using POST method , 
            $url = 'https://ufanisicourier.co.ke/api/v1/api/getdestinations?apikey={{yourapikey}}';   
            $response  = ProcessRequest($data, $url);
            return $response; 
    }
```

 ## Get  Destinations  Response
 - Destinations Response 

```json 
        {
        "status":1,// 1 for success 0 for failed;
        "data": [{
                    "name":"", 
                 }] 
        }
```

## WEBHOOKS
- Register a tracking webhook.
- Webhooks typically are used to connect two different applications. When an event happens on the trigger application, it serializes data about that   event and sends it to a webhook URL from the action application—the one you want to do something based on the data from the first application. 

- You can register your webhook(s) for a Shipments updates, this way, UFANISI  will send HTTP notifications to your provided url  whenever the status changes.

- Your webhook url should be able to receive the data and update your database at anytime or even perform other function like sending SMS & Email notifictions with your own implementation.
- Login to your account in https://developer.ufanisicourier.co.ke and add your callback url under webhook tab.

 ## Webhook  Response
 - You can save this callback response in your database 
 - This response is only sent when ufanisi make a change in your submitted shipments eg Shipment status update eg pending, approved, packaging , transist, delivered etc. 

```json 
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
- Check our demo  on how the api can be used to forward and track your shipments at https://ufanisicourier.co.ke/apidemo/

 

