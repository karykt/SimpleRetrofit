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


**4.Create the API Interface:** 

Following, we will create an Interface to define our different methods that will be used for network transactions. Let's name it *ApiInterface*.

```
import retrofit.Callback;
import retrofit.http.Field;
import retrofit.http.FormUrlEncoded;
import retrofit.http.POST;

public interface ApiInterface {

    @FormUrlEncoded 
    @POST("/retrofit/register.php")     
    public void registration(@Field("name") String name,
                             @Field("email") String email,
                             @Field("password") String password,
                             @Field("logintype") String logintype,
                             Callback<SignUpResponse> callback);
}
```

![Create API Interface](https://github.com/karykt/SimpleRetrofit/blob/master/Retrofit%20images/ApiInterface.JPG)


**5.Define the RestAdapter:**

Now we need to define the RestAdapter to implement the API’s. This class will have a method that creates the connection and then returns the API Interface object.

```
import retrofit.RestAdapter;


public class Api {

    public static ApiInterface getClient() {

      
        RestAdapter adapter = new RestAdapter.Builder()
                .setEndpoint("http://mobileappdatabase.in/") 
                .build(); 

        //Creating object for our interface
        ApiInterface api = adapter.create(ApiInterface.class);
        return api;
    }
}
```

![Define the RestAdapter](https://github.com/karykt/SimpleRetrofit/blob/master/Retrofit%20images/Api.JPG)


**6.Create a RecyclerView in our XML file:**

Open res ⇒ layout ⇒ activity_main.xml (or) main.xml and add the following code:

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">


    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scrollbars="none">


        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:orientation="vertical"
            android:padding="20dp">


            <EditText
                android:id="@+id/username"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:backgroundTint="#000"
                android:hint="Full Name"
                android:imeOptions="actionNext"
                android:inputType="text"
                android:paddingBottom="15dp"
                android:paddingLeft="5dp"
                android:singleLine="true"
                android:textColor="#000"
                android:textColorHint="#000" />


            <EditText
                android:id="@+id/email"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:backgroundTint="#000"
                android:hint="Email Address"
                android:imeOptions="actionNext"
                android:inputType="textEmailAddress"
                android:paddingBottom="15dp"
                android:paddingLeft="5dp"
                android:singleLine="true"
                android:textColor="#000"
                android:textColorHint="#000" />


            <EditText
                android:id="@+id/password"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:backgroundTint="#000"
                android:hint="Password"
                android:imeOptions="actionDone"
                android:inputType="textPassword"
                android:paddingBottom="15dp"
                android:paddingLeft="5dp"
                android:singleLine="true"
                android:textColor="#000"
                android:textColorHint="#000" />


            <Button
                android:id="@+id/signUp"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="10dp"
                android:backgroundTint="@color/colorPrimary"
                android:text="Sign Up"
                android:textColor="#fff"
                android:textSize="17sp" />

        </LinearLayout>
    </ScrollView>
</RelativeLayout>
```

**7.Add code to the MainActivity.java:**

Finally, we are getting reference of the *EditText* and *Button.After* that we implemented the *setOnClickListener* event. The data in *EditText* is validate and then we are implementing signup api to save the data in our database. After getting response from the api we are displaying the message on the screen by using a Toast.

Go to app ⇒ java ⇒ package ⇒ MainActivity.java and add the code below:


```
import android.Manifest;
import android.app.ProgressDialog;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

import java.util.List;

import retrofit.Callback;
import retrofit.RetrofitError;
import retrofit.client.Response;

public class MainActivity extends AppCompatActivity {

    SignUpResponse signUpResponsesData;
    EditText email, password, name;
    Button signUp;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        // init the EditText and Button
        name = (EditText) findViewById(R.id.username);
        email = (EditText) findViewById(R.id.email);
        password = (EditText) findViewById(R.id.password);
        signUp = (Button) findViewById(R.id.signUp);
        // implement setOnClickListener event on sign up Button
        signUp.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                // validate the fields and call sign method to implement the api
                if (validate(name) && validate(email) && validate(password)) {
                    signUp();
                }
            }
        });
    }

    private boolean validate(EditText editText) {
        // check the lenght of the enter data in EditText and give error if its empty
        if (editText.getText().toString().trim().length() > 0) {
            return true; // returs true if field is not empty
        }
        editText.setError("Please Fill This");
        editText.requestFocus();
        return false;
    }

    private void signUp() {
        // display a progress dialog
        final ProgressDialog progressDialog = new ProgressDialog(MainActivity.this);
        progressDialog.setCancelable(false); // set cancelable to false
        progressDialog.setMessage("Please Wait"); // set message
        progressDialog.show(); // show progress dialog

        // Api is a class in which we define a method getClient() that returns the API Interface class object
        // registration is a POST request type method in which we are sending our field's data
        Api.getClient().registration(name.getText().toString().trim(),
                email.getText().toString().trim(),
                password.getText().toString().trim(),
                "email", new Callback<SignUpResponse>() {
                    @Override
                    public void success(SignUpResponse signUpResponse, Response response) {
                        // in this method we will get the response from API
                        progressDialog.dismiss(); //dismiss progress dialog
                        signUpResponsesData = signUpResponse;
                        // display the message getting from web api
                        Toast.makeText(MainActivity.this, signUpResponse.getMessage(), Toast.LENGTH_SHORT).show();
                    }

                    @Override
                    public void failure(RetrofitError error) {
                        // if error occurs in network transaction then we can get the error in this method.
                        Toast.makeText(MainActivity.this, error.toString(), Toast.LENGTH_LONG).show();
                        progressDialog.dismiss(); //dismiss progress dialog

                    }
                });
    }
}
```

