---
layout: post
title: Encode a URI vs URLEncoder in Java and Javascript
description: "used encodeURI in javascript, and want same in Java but you can't have it"
tags: 
  - tip
  - URL
  - URI
  - java
comments: true
share: true
published: true
---

Encoding URLs in Java is quite trivial. However, too often I see people using the URLEncoder class for this. This is WRONG.

The URLEncoder class is used for encoding form data into xxx-form-urlencoded -format. Even though very poorly named, it does say this in the Javadocs ;)

The proper class for URL encoding is the URI class.

Lets assume we have an URL such as http://www.somedomain.com/shop/Awesome Sauces

If you encode this with the URLEncoder class, you get:

```http%3A%2F%2Fwww.somedomain.com%2FAwesome+Sauces```

Unreadable and incorrectly encoded. Reserved characters such as : and / should not be encoded. Also, the URLEncoder encodes empty spaces as a "+" even though it should be encoded as "%20".

Which makes it really annoying and doesn't end up matching up with what you would expect if you used javascript's ```encodeURI``` and ```encodeURIComponent()``` method for example. 

With the URI class, you get:

```http://www.somedomain.com/Awesome%20Sauces```

which is much better and correct.

To construct as simple URL like the example above with the URI class:

```URI myURI = new URI("http", "www.somedomain.com", "/Awesome Sauces");```

And to get the URL in an encoded format:

```String url = myURI.toASCIIString();```

If you want to do something like encode the functionality found in ```encodeURIComponent()```

Here's a bit of a dodgy work around. 
````
    private String getEncodedFileName(String filename) {
        try {
            def filelink = new URI("file","//", filename)
            return filelink.toASCIIString().replace("file://#",'')
        }catch (Exception e){
            return  filename!=null? filename:"";
        }
    } 
```
