RepairWise Plugin
=================
A simple web application that has the RepairWise Angular Plugin embedded.

## Contents

* [Integration](#integration)
* [Styling](#styling)
* [OAuth](#oauth)

# Integration

In your `index.html` add a reference to our plugin.

```html
<head>

  <!-- Any other tags you want -->

  <script src="http://dev-rw-plugin.s3-website-us-west-2.amazonaws.com/repairwise-embed.bundle.js"></script>
</head>
```

Next add a reference to a javascript file that will contain the 2 required functions `getToken` and `saveReportInfo`, ours is called `utils.js`.

```html
<head>

  <!-- Any other tags you want -->

  <script src="utils.js"></script>
  <script src="http://dev-rw-plugin.s3-website-us-west-2.amazonaws.com/repairwise-embed.bundle.js"></script>
</head>
```
To include the plugin into the page add the `<rw-main>` tag.

```html
<rw-main>Loading...</rw-main>
```

Now to actually start and load the plugin do the following:

```html
    <script>
        window.startRepairWise(getToken, saveReportInfo, address, redirectUrl);
    </script>
```

The plugin won't quite work yet, we need to implement the two required methods `getToken` and `saveReportInfo` in our `utils.js`.

First we'll add the `getToken` method.

```javascript
  function getToken() {
    // callback to your OAuth resource
  }
```
For more information on OAuth and getting a token read [OAuth](#oauth).

Lastly we'll add the `saveReportInfo` in our `utils.js`.

```javascript
  function saveReportInfo(addressJson, linkToReport) 
  { 
     console.log(addressJson.propertyNumber); 
     console.log(addressJson.street); 
     console.log(addressJson.city); 
     console.log(addressJson.stateProvince); 
     console.log(addressJson.postalCode); 
     console.log(addressJson.country); 
    
     console.log(linkToReport); 
  } 
```

We are just printing this information to the console, ultimately what you do with the `addressJson` and `linkToReport` is up to you.

`address` and `redirectUrl` are optional parameters.

`address` is a json object that must be formatted in the following way:
```javascript
{
  propertyNumber: string
  street: string
  city: string
  stateProvince: string
  postalCode: string
  country: string
}
```

`redirectUrl` is a url string that will enable the close button feature. This will be a button in RepairWise that the user can click to go to the provided url. For example, clicking the close button when `redirectUrl` is `https://www.google.com/` will take the user to `https://www.google.com/`.

Now the plugin will successfully load in your page!  You will may notice however that there is no styling.

# Styling

It is requires that you add the provided style sheet `repairwise-style.css` to the root of your website.

Ensure that both yours and our style sheets are include in the `index.html`.

```html
    <link rel="stylesheet" type="text/css" href="repairwise-style.css" />
    <link rel="stylesheet" type="text/css" href="styles/main.css" />
```

The colors of the plugin can be customized to fit the style of your website. We have provided a style sheet `repairwise-style.css`.

For example to change the color of the header, open the `repairwise-style.css` file and go to.

```css
.rw-header {
	/*
		Thin header trim 
	*/
	background-color: #032D4C;
}
```

Change the color to `#F2F2F2`, for example

```css
.rw-header {
	/*
		Thin header trim 
	*/
	background-color: #F2F2F2;
}
```

It is critical to mention that none of the `.css' classes can be renamed.

# OAuth

RepairWise will authenticate all network requests using a client credential grant workflow. 

You will need to create your own function that performs a post request to https://identity.verisk.com with the following parameters:

```
"client_id", "repairwise"

"client_secret", <-- Your secret key -->

"grant_type", "client_credentials"

"scope", "rw_api"
```

Your post request must return a `Promise` that resolves to a string. This will be your token that will be appended to all requests in the plugin.