# Simple Retrofit Tutorial
This project is a simple step-by-step tutorial to create an Android application that uses the Retrofit library to retrieve and upload data (JSON or any other structured data) via a REST based web service.

## What is Retrofit?
Retrofit is a REST type-safe HTTP Client for Java and Android. Created by Square inc under Apache 2.0 license it turns your HTTP API into a Java interface. 
Retrofit makes relatively easy to retrieve and upload JSON (or other structured data) via a REST based webservice. In Retrofit you configure which converter is used for the data serialization.

## Getting Started

Let's get started by first creating a new project.

**1.Create a new project:**

Go to File ⇒ New Project and create a new project named RetrofitTutorial. You have to select the default activity ⇒  select Empty Activity and proceed.


**2.Add dependencies:** 

Open the Gradle Script file in the app module and add these libraries in your build.gradle.

```
compile ‘com.squareup.retrofit2:retrofit:2.3.0’
compile "com.android.support:recyclerview-v7:28.0.1"
```


![Add Dependencies](https://github.com/karykt/SimpleRetrofit/blob/master/Retrofit%20images/dependencies.JPG)


**3.Add Internet Permission in the AndroidManifest.xml:** 

Define Internet permission in your Manifest file as showed bellow

```
<uses-permission android:name="android.permission.INTERNET" />
```

![Add Internet Permission](https://github.com/karykt/SimpleRetrofit/blob/master/Retrofit%20images/internet_permission.JPG)


**4.Create API Interface:** 

Following, we will create an Interface to define our different methods that will be used for network transactions. Let's name it ApiInterface.

```
import retrofit.Callback;
import retrofit.http.Field;
import retrofit.http.FormUrlEncoded;
import retrofit.http.POST;

public interface ApiInterface {

    @FormUrlEncoded // annotation used in POST type requests
    @POST("/retrofit/register.php")     // API's endpoints
    public void registration(@Field("name") String name,
                             @Field("email") String email,
                             @Field("password") String password,
                             @Field("logintype") String logintype,
                             Callback<SignUpResponse> callback);
// In registration method @Field used to set the keys and String data type is representing a string
// type value and callback is used to get the response from api and it will set it in our POJO class
}
```

![Create API Interface]()


**5.Define the RestAdapter: **

Now we need to define the RestAdapter to implement the API’s. This class will have a method that create the connection and then returns the API Interface object.

```
import retrofit.RestAdapter;


public class Api {

    public static ApiInterface getClient() {

        // change your base URL
        RestAdapter adapter = new RestAdapter.Builder()
                .setEndpoint("http://mobileappdatabase.in/") //Set the Root URL
                .build(); //Finally building the adapter

        //Creating object for our interface
        ApiInterface api = adapter.create(ApiInterface.class);
        return api; // return the APIInterface object
    }
}
```

![Define the RestAdapter]()


**6.Create a RecyclerView in our XML file: **


