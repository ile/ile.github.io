---
layout: post
published: true
---

Facebook's Graph API is such a big mess^H complicated process that after having tackled it down I'm compelled to write this post, and maybe other ones later.

Here I'm showing how to use the real time updates feature. Meaning, you will get a notification whenever a user, who has authorized your app, writes for example a status message (or does something else that the app is instructed to track). 

I'm going to show the steps and explain along the way.

### 1. Create a new App

![image](https://cloud.githubusercontent.com/assets/433707/6323320/7c59397e-bb30-11e4-977f-dd31ec095b05.png)

Of type "www".

### 2. Do some basic setup

![image](https://cloud.githubusercontent.com/assets/433707/6323339/db33a678-bb30-11e4-8e10-4841124c7e70.png)

Site URL and App Domains. Not sure if these are mandatory but they don't seem to hurt.

![image](https://cloud.githubusercontent.com/assets/433707/6323490/d92533bc-bb33-11e4-92b9-5a34f39bf50e.png)

Also set the deauthorize url. This will be called whenever the user deauthorizes your app. Knowing this will probably be useful.

### 3. Go to Graph API Explorer

![image](https://cloud.githubusercontent.com/assets/433707/6323376/9619e2b8-bb31-11e4-9505-b2d65bed8b51.png)

This is the important stuff. Here you will setup the subscriptions that the App is tracking. First, you can see that the sunscriptions are empty by doing a GET request. Fill your own App ID. Click the `Get App Token` to automatically authorize the request.

### 4. In your server code, set up the callback for the upcoming subscript setup

When you use the Graph API Explorer there will be a request to your server to verify the callback url you will be using for the real subscriptions.

Facebook will issue a GET request to this url with some parameters. The parameters are `hub.challenge` and `hub.verify_token`. You will need to send the `hub.challenge` back in the request handler. `hub.verify_token` is for you to check that it is the same which you were expecting (you will set it yourself in a bit).

For example, here is the JavaScript code for this request step:

{% highlight js lineos %}
router.get('/cb', function(req, res, next) {
	console.log('get /cb', req.query);
	if (req.query['hub.verify_token'] === 'moi') {
		res.send(req.query['hub.challenge']);
	}
});
{% endhighlight %}

### 5. Set the subscription in Graph API Explorer

![image](https://cloud.githubusercontent.com/assets/433707/6323445/dbe20112-bb32-11e4-809a-cad12d42ac69.png)

Here we are doing a POST request to setup the callback url and the fields that this App will be tracking. Here we track the user's statuses (hence object `user` and fields `statuses`).

If your callback url is working correctly you should get a success message back.

Now, if you go back to step 4 and issue a GET request, you should see the subscription:

![image](https://cloud.githubusercontent.com/assets/433707/6323566/52ace418-bb35-11e4-8b32-65b2bd7e1f80.png)


### 6. Setup the Login button and permissions

With the login button the user is authorizing your app to track his/her status messages.

The code to set this up can be taken from Facebook's docs, for example [here](https://developers.facebook.com/docs/facebook-login/login-flow-for-web/v2.2). This code will be using the Facebook JS SDK which will be loaded "on the fly" with the script snippet. You will need to add your app id into the correct place.

The permissions are setup in the JavaScript code for now. Whenever your Facebook App goes public (now it's in the test mode) you will need to setup the permissions in the App as well.

So, edit the Html/JS we got from Facebook's docs:

```html
<fb:login-button scope="public_profile,email,user_status" onlogin="checkLoginState();"></fb:login-button>
```

Here we are issuing the permissions `public_profile,email,user_status`. In the test mode, we don't need to bother Facebook with reviewing the app, but if we were to publish the app, we would submit our app for reviewing because we of the permission `user_status`.

### 7. Start your server

![image](https://cloud.githubusercontent.com/assets/433707/6323500/236661a8-bb34-11e4-9508-4f15b7c60d5d.png)

We should see the login button if we are lucky. Click it.

![image](https://cloud.githubusercontent.com/assets/433707/6323635/390f9cf2-bb36-11e4-813e-7aa4778f4217.png)

Let's authorize the app.

### 8. We should be all set. Write a status message.

Now after writing the status message you should get a callback from Facebook.

If you got this far and got the callback, you will get a user id and 'changed fields' to notify you what has changed. This won't get you the actual status message but you will have to go get it with the Graph API. That is another story. 

The code for this excercise is here (a Node.js app): [https://github.com/ile/fb-real-time-example](https://github.com/ile/fb-real-time-example).

In the server code, there is the app secret that you need to change. This is used for various things. Here is it used to decode the signed request for deauthorization POST messages.

P.S. if you want to save some time and trouble with the dreaded FB API, you can [hire me](http://ilkkah.com/contact).






