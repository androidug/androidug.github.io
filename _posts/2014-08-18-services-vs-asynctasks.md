---
layout: post
title: Services vs Asynctasks
description: "Summary of presentation on Android services and asynctasks"
category: presentation
tags: [meetup]
---
##### by *Elsabé Ros* #####

This is a summary of a talk on Asynctasks and Services given at the Android usergroup on the 18th of August 2014. For the full example used see [https://github.com/AesSedai101/Android-Asynctasks-and-Services](https://github.com/AesSedai101/Android-Asynctasks-and-Services).

----------

By default, everything in Android runs in the UI thread. For long running tasks, this can lock up the application and cause a Application Not Responding error. To prevent this, the developer needs to execute long running tasks in a separate thread. Android provides two mechanisms to do this: Asynctasks and Services, in addition to the standard Java threading classes.

### Asynctask ###
From the Android documentation:
> "This class allows to perform background operations and publish results on the UI thread without having to manipulate threads and/or handlers."
> 
> `public abstract class AsyncTask<Params, Progress, Result>`

- The Asynctask class is a helper class built around Threads and Handlers. 
- It is used for short tasks (a few seconds at most) 
- A specific instance can only be executed once.

Snippet from example: 

	public class ImageAsyncTask extends AsyncTask<ImageCalculator, Void, Bitmap> {
    	@Override
    	protected Bitmap doInBackground(ImageCalculator... imageCalculators) {
       		....
    	}

    	@Override
    	protected void onPostExecute(Bitmap bitmap) {
        	super.onPostExecute(bitmap);
        	....
    	}
	}

The Asynctask takes in three template parameters: the parameters passed to the Asynctask, the return type of the doInBackground method and the type of the progress units published while the task is busy.

The Asynctask also has two abstract methods that needs to be implemented:

- **`doInBackground`** the long running task will happen in this method.
- **`onPostExecute`** will take in the result returned from `doInBackground` as a parameter and is invoked on the *UI thread*

There is also a few optional methods that can be overridden:

- **`onCancelled`** runs on the *UI thread* after `cancel` was called on the Asynctask
- **`onPreExecute`** runs on the *UI thread* before the main task executes
- **`onProgressUpdate`** runs on *UI thread* after the `doInBackground` invoked the `publishProgress` method

### Service ###
From the Android documentation:
> "A Service is an application component that can perform long-running operations in the background and does not provide a user interface."
> 
> `public abstract class Service`

- Android services are not bound to the lifecycle of their application. - Service will continue running in the background, even if the application gets killed. 
- Android allows applications to share services. 
- An Android services is a Singleton - it is impossible to start multiple instances of the same service. 
- Applications can communicate with service in two ways:
	- Broadcast - message delivery not guaranteed
	- Binding - call methods on service directly

Snippet from example:

    public class ImageService extends Service {
    	@Override
    	public int onStartCommand(Intent intent, int flags, int startId) {
       		....
        	return START_STICKY;
    	}

    	@Override
    	public IBinder onBind(Intent intent) {
        	return new MyBinder(this);
    	}

    	public class MyBinder extends Binder {
    	    ....
    	}
	}

The only method that a Service implementation needs to implement is the `onBind` method, that allows an application to interact directly with the service.