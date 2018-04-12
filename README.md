![logo](https://i.imgur.com/NHC1sJX.png)

# Adyo JS 

The Adyo JS SDK makes it easy to integrate Adyo ads into your website. Using our script tags, your ads will be showing in no time.

## Basic Usage

To get started, two components are required in the HTML of your web page:

### Zone Div Tag

Place a `div` tag in the `body` of your HTML where you would like the ad to be displayed. The `div` requires a unique ID which will be used by the JS SDK.

``` html
<div id="UNIQUE-ID-OF-DIV" style="width:300px; height:250px"></div>
```
**Note**: The `div` tag represents the actual zone as defined in the Adyo API. Please make sure that the `div` is the exact same size as the zone.

### Javascript

There are two ways to implement the Javascript tags:

* Async (Recommended)
* Synchronous (for earlier browsers without Async support)

#### Method 1: Async

Copy the script below and place it just before the closing `</body>` tag:

```js
<script type="text/javascript">
  (function(){
    var s = document.createElement('script'); 
    s.type = 'text/javascript';
    var protocol = (window.location.protocol === 'https:' ? 'https:' : 'http:');
    s.src = protocol.concat('//cdn.adyo.co.za/adyo.min.js');
    s.type = 'text/javascript';
    s.src = 'adyo.min.js';
    s.async = true;
    s.onload = s.onreadystatechange = function () {
      var rs = this.readyState; 
      if (rs) if (rs != 'complete') if (rs != 'loaded') return;
  
      try {
        Adyo = window.Adyo || {}; Adyo.ads = Adyo.ads || [];
        Adyo.ads.push({
          handler: function(params) { 
            Adyo.init(params); 
          },
          params: { 
            dom_id: 'UNIQUE-ID-OF-DIV',
            network_id: 13, // YOUR NETWORK ID GOES HERE
            zone_id: 1, // YOUR ZONE ID GOES HERE
          }
        });
        
      } catch (e) {};
    }
  
    var n = document.getElementsByTagName('script')[0]; 
    n.parentNode.insertBefore(s, n);
  }());
</script>
```



#### Method 2: Synchronous

Include the Adyo JS in your HTML head:

``` html
<head>
  ...
  <script type="text/javascript" src="https://cdn.adyo.co.za/adyo.min.js"></script>
</head>
```


Copy the script below and place it just before the closing `</body>` tag:

```javascript
<script type="text/javascript">
  (function(){
    Adyo = window.Adyo || {}; Adyo.ads = Adyo.ads || [];
    Adyo.ads.push({
      handler: function(params) { 
        Adyo.init(params); 
      },
      params: { 
        dom_id: 'UNIQUE-ID-OF-DIV',
        network_id: 13, // YOUR NETWORK ID GOES HERE
        zone_id: 1, // YOUR ZONE ID GOES HERE
      }
    });
  }());
</script>	
```




### Request Parameters

Apart from providing the `network_id` and `zone_id` in your script, other *optional* parameters are also available:

* `user_id`: A unique identifier for a user. Used for frequency capping. If no `user_id` is provided, the SDK automatically creates a unique identifier in local storage for subsequent requests.
* `keywords`: A comma separated string of keywords to include in the request, e.g: `bmw,audi,volvo`. Used for keyword targeting.
* `width` and `height`: Used to manually override the dimensions of the zone as defined in the API (Explained below)



**Example Parameters**:

```javascript
..
params: { 
	dom_id: 'UNIQUE-ID-OF-DIV', // Required
	network_id: 13, // Required
	zone_id: 1, // Required
        user_id: "user-123abc", // Optional string
  	keywords: "bmw,audit,volvo", // Optional comma seperated string
      
        // Optional dimensions override (E.g zone_id 1 has a different height and width to our current situation). Used to fetch the closest sized ad.
  	width: 500,
  	height: 500,
}
..
```



#### Manual Dimension Overriding

A situation might arise that the dimensions of your zone don't equal the dimenions of your `div` tag. Especially when your tag uses CSS rules to automatically size itself (e.g `width: 100%`).

The Adyo serving engine always tries to pick the closest sized creative (image/html) for the placement (ad) request using the dimensions of the zone specified by the `zone_id` parameter in your request.

If the example zone has a dimension of `1000x1000` then any ad request will return creatives closest to that dimension. If your `div` is only `500x350` then the creatives won't fit properly within the `div`. The SDK will automatically try and scale and position the creative as best as possible.

To solve this situation, we can provide the `height` and `width` parameters in the request to ignore the zone's `1000x1000` dimension, and rather use `500x350` when searching for the closest sized creatives.

**Note:** This assumes the zone indeed has attached creatives of a closer size to `500x350`. Else the `1000x1000` creatives will still get chosen as there are no other choices.



## Feedback

For any feedback, please contact us at: devops@unitx.co.za or create an issue. You are also more than welcome to send a pull request for any changes or bug fixes.



## Changelog

- v1.0.0 - Initial Release.
- v1.0.1 - Added functionality to show popup depending on the placement targets. Performance enhancements and bug fixes.
- v1.0.2 - Changed ES6 style code to vanilla to support older platforms.
