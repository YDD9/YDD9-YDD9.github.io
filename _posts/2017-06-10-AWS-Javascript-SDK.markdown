---
layout: post
title:  "AWS-Javascript-SDK"
date:   2017-06-10 11:49:39 +0100
comments: true
categories: AWS Javascript Node SDK
---

[link](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/loading-the-jssdk.html)
In Web Browsers

The quickest way to get started with the SDK is to load the hosted SDK package directly from Amazon Web Services. To do this, add the following script tag to your HTML pages:

```
<script src="https://sdk.amazonaws.com/js/aws-sdk-2.7.20.min.js"></script>
```  

After the SDK loads in your page, the SDK is available from the global variable AWS (or window.AWS).


Unfortunately above sdk mini doesn't contain all components to work on, such as LexModelBuildingService is too new to be included. So you have to use the aws-sdk differently.
Here's a [blog article](https://aws.amazon.com/blogs/developer/category/javascript/) walks you through downloading complete aws-sdk into your project then use them in Js.  

If you want to create your copy of aws-sdk and include only components you need, here's the [link](https://sdk.amazonaws.com/builder/js/) to generate your own copy of aws-sdk.

Simple approach to make it run:  
```  
npm i -g aws-sdk
# in your project root, the sdk will installed inside node_modules
npm i aws-sdk
# declare global variable AWS in your js
var AWS = require('aws-sdk/dist/aws-sdk-react-native');
```   


