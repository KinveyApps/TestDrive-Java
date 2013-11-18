## Add an App Backend

In the Kinvey console click "Add an App", enter the name "TestDrive-java" when prompted.


## Set up TestDrive Project

1. Download the [TestDrive](https://github.com/KinveyApps/TestDrive-Java/archive/master.zip) project.
2. In Eclipse, go to **File &rarr; Import…**
3. Click **Java &rarr; Existing Project into Workspace**
4. **Browse…** to set **Root Directory** to the extracted zip from step 1
5. In the **Projects** box, make sure the **TestDrive** project check box is selected. Then click **Finish**.
6. Download the latest Kinvey library (zip) and extract the downloaded zip file, from: http://devcenter.kinvey.com/java/downloads
7. Right click on your project &rarr; **Build Path &rarr; Configure Build Path** and on the **Libraries** tab click **Add External Jars**.  Navigate to where you downloaded the Kinvey Jars and add them all. 
8. In Eclipse, right click the project &rarr; **Close Project** &rarr; right click project (again) &rarr; **Open Project**. 
9. Specify your app key and master secret in `HelloWorld` constant variables
**appKey** and **appSecret**

Take a look at our [Getting Started](http://devcenter.kinvey.com/java/guides/getting-started) guide for more information.

```java
public class HelloWorld {

    public static final String appKey = "my_app_key";   
    public static final String mastersecret = "my_master_secret";  
	
```

## Saving Data
### Creating Persistable Objects

With Kinvey's Java library you model data using any class that extends the `GenericJson` class. The sample project comes with a `HelloEntity` class that has a string `somedata`, as well as a field labelled `id`.  When this class is converted to JSON, `somedata` will be given the label "somedata", while `id` will be given the label `_id` which is accomplished by the attribute @Key("_id")

`HelloEntity.java` looks like this:

```java
public class HelloEntity extends GenericJson {

    @Key("_id")
    private String id;

    @Key
    private String somedata;


    public HelloEntity(){}

    public HelloEntity(String somedata){
        this.somedata = somedata;
    }

    public String getSomedata() {
        return somedata;
    }

    public void setSomedata(String somedata) {
        this.somedata = somedata;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }
```

### Saving

The following method is called in the `main(String[] args)` of the `HelloWorld` class. When the app runs, first it will create a client and ping the backend, then it will login a user and next execute the code snippet below.

After creating an entity, the Client is used to access the AppData API, where a call to `saveBlocking` will persist the entity with Kinvey.  This call is performed syncronously, and a call to `.execute()` returns the result.


```java
    HelloEntity test = new HelloEntity();
    test.setSomedata("hello");
    try{
        HelloEntity saved = myJavaClient.appData("native", HelloEntity.class).saveBlocking(test).execute();
        System.out.println("Client appdata saved -> " + saved.getId());
    }catch (IOException e){
        System.out.println("Couldn't save! -> " + e);
        e.printStackTrace();
    }
```



## Load Data
The `AppData` object has a `getBlocking` method as a well as a save, and this can be used to load the object by id that was just saved. 

```java
    try{
        HelloEntity loaded = myJavaClient.appData("native", HelloEntity.class).getEntityBlocking().execute();
        System.out.println("Client appdata loaded by id -> " + loaded.getId());
    }catch (IOException e){
        System.out.println("Couldn't load! -> " + e);
        e.printStackTrace();
    }
```

This is very similar to Saving data, as the AppData API is accessed through the client.  The call to `getBlocking` supports a query, so this snippet is just sending a new empty query to retrieve all results.


## Run the App
Run the sample project and watch the logs, operations are performed in order and will output either success or failure.

## Next steps


* [Data Store Guide](/java/guides/datastore) - How to query for data, delete entities 
* [Security](/java/guides/security) - How to share data or limit read/write access  
* [User Guide](/java/guides/users) - How to add users to your app and understand authentication


##License


Copyright (c) 2013 Kinvey Inc.

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except
in compliance with the License. You may obtain a copy of the License at

 http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License
is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express
or implied. See the License for the specific language governing permissions and limitations under
the License.
