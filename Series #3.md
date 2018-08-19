#### Repository
https://github.com/aosp-mirror/platform_build

#### What Will I Learn?

- You will learn how to create and send Intent Data from one activity to another.
- You will learn how to create and manage ListView Widgets - ListView is a scroll-able view that can handle near any form of list.
- You will learn how to create simple a custom adapter for your ListView, so as to display data in an irregular format.

#### Requirements
State the requirements the user needs in order to follow this tutorial.

- Basic knowledge of Java, and XML.
- Android Studio installed on your operating system of choice.
- A device to run the application within, can be a physical device, or a VM like the one included in Android Studio's 'Device Manager'.

#### Difficulty

- Basic

#### Tutorial Contents

## <center>Introduction</center><br>

In this tutorial, we will again be adding to our 'steemie' application - name still a work in progress :P. The three main things that we will be focusing on, as mention above will be Intent Extras, The ListView Widget, and Custom Listiew Adapters. I will explain each one of these tools/concepts briefly before we get into the coding. I am also aiming to make these tutorials more readable, user friendly, and enjoyable for everyone, so I will be changing the formatting a slight bit, as well as including a few more helpful images and comments.

### Intent Extras: Sending Data Between activities. 

As I may not have explained so well in the past, activities are simply, the user interfaces that are presented to the user, along with the corresponding code required to power the UI. In an application like 'Instagram, the home screen is one activity, and the search screen is another. When a developer wants the user to be able to pass data between these two activities, one must use something called an Intent Extra. You will see a good example of this as the tutorial goes on. 

### The Listview Widget, and Custom Adapters:

The ListView Widget, is rather simple to explain, it is not much different than a TextView, or a Button - They are views that are cast to the UI, and implemented in our programming. The difference between a ListView and the rest, is that we use these widgets to display a scroll-able list of data for the user to browse through leisurely. A ListView can quite easily be implemented with a simple list of strings, but today I want to show you how to set up your own CustomAdapter. CustomAdapters allow the developer to display many different types of lists, rather than a simple one by one.

### Simple Edits:

Since we are doing this tutorial using the same application as before, I will be changing a few different things in the code that you currently have, aside from adding to it. I have not changed much, but this does confuse the subject a slight bit, I really should just be giving you an example.. But I want you to have an awesome application at the end of this series! All of the changed pages of the application will be available for you to simply copy and paste, if I do not give enough detail on everything changed, but it is really not all that much. We have added a few strings for the most part :P

## Changes - fetchdata.java

<center>![listv1.PNG](https://cdn.steemitimages.com/DQmULs4foZh19mvHiKpknsCDbogE8tZgRNCjHVy3Pu15JEA/listv1.PNG)<br>
[Screenshot](https://cdn.steemitimages.com/DQmULs4foZh19mvHiKpknsCDbogE8tZgRNCjHVy3Pu15JEA/listv1.PNG)</center>

As you can see in the image above, I have highlighted the things I have changed from our previous code. In this new version of fetchdata.java, we have created a 'user' String, as well as changed our fixed URL, to include our variable. Above our URL,  we have three new lines. In the first, we create a string, and set its value to the text that is contained within our EditText 'searchBar', from the followers activity.

The next line, is an empty if loop, that says to do nothing if the searchBar is empty, preceding an else statement, which dictates that if the searchBar is anything other than empty, to change our 'user' String to the value of the new text retrieved.

<center>![listv2.PNG](https://cdn.steemitimages.com/DQmVX8wf2HEUMPvQ4rKnvzaxBQVZVyxvu3gRj8Aft1Xeviz/listv2.PNG)
<br>[Screenshot](https://cdn.steemitimages.com/DQmVX8wf2HEUMPvQ4rKnvzaxBQVZVyxvu3gRj8Aft1Xeviz/listv2.PNG)</center>

You will see in the GitHub version of this tut(Linked Below), that I have changed the XML layout file for followers.xml, so as to include a Search Bar, and 2 Buttons. The bar to contain a users name, one button to search for the new user, and another to add this new user to a list of people that you are currently following. The above image depicts some strings I have changed in fetchdata.java, as well as the one below.

<center>![listv3.PNG](https://cdn.steemitimages.com/DQmSZDHKtwLJynuQkkiX7RUs1ws3Q1utiuBwz7nzZ6PKtd8/listv3.PNG)
<br>[Screenshot](https://cdn.steemitimages.com/DQmSZDHKtwLJynuQkkiX7RUs1ws3Q1utiuBwz7nzZ6PKtd8/listv3.PNG)</center>

For the most part, these Strings were simply shortened, to make room for the new widgets, and you may feel free to arrange them in really any way you please, provided it is functional. That is about it that we have done in fetchdata, so lets carry on with the next pages.

## Changes/Additions: followers.java

![list5.PNG](https://cdn.steemitimages.com/DQmZypep8dGPGAX5sd8pkhQaLfWWKX1o9ao4MLk6hmjuHtb/list5.PNG)
<center>[Screenshot](https://cdn.steemitimages.com/DQmZypep8dGPGAX5sd8pkhQaLfWWKX1o9ao4MLk6hmjuHtb/list5.PNG)</center>

In the upper half of the image above, we see that we need to add in 2 new Buttons, as well as a public, static EditText, and String. These are made public and static to be used and reused across the application. Farther down in the onCreate(), we simply assign our variables to our widget addresses. In the next image, we implement two onClickListeners attached to our newly created buttons, like so.

<center>![list6.PNG](https://cdn.steemitimages.com/DQmSK6RGXVFimGCpwhGYHovHnJ8gEe1Kqk8wgpNjK9CMHp4/list6.PNG)
<br>[Screenshot](https://cdn.steemitimages.com/DQmSZDHKtwLJynuQkkiX7RUs1ws3Q1utiuBwz7nzZ6PKtd8/listv3.PNG)</center>

The first listener, is attached to our 'searchButton', and for that, we simply copy and paste the process from above into this listener, as all we really need to do is the same as before, with a new username. Our variable makes this rather simple. Followbutton, is a bit more of a step, where we will actually be launching a new activity. ^_^ This is also where we will be sending some Intent Data. Code Follows.

        followButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                  userName = searchBar.getText().toString();
                Intent i = new Intent(followers.this, FollowersList.class);
                i.putExtra("Username", userName);
                startActivity(i);
            }
        });

In the code snippet above, we first set a listener. Next we set our 'userName' String to the value of the 'searchBar' EditText. After this, we Create a new intent, launching a new blank activity that if you have not yet created, you may now want to do. Once this line is finished, we use the putExtra() function of Intent, to store our userName String, under the Title "Username". Finally, we launch the new activity. This will all happen when this new 'Follow' Button is clicked.

## New Activity: FollowersList.java

<center>![list7.PNG](https://cdn.steemitimages.com/DQmdJTNPwh4F6tvjyfY4UeAMx8733omNvhYFvL1ckSFzuqP/list7.PNG)
<br>[Screenshot](https://cdn.steemitimages.com/DQmdJTNPwh4F6tvjyfY4UeAMx8733omNvhYFvL1ckSFzuqP/list7.PNG)</center>

Now this is a brand new addition to our application, so I'm not really gonna bother with the highlighting this time. Instead, I'm just going to break this down for you, section by section, and give you a clear understanding of whats going on. First off, we initialize our ListView, and our Public String.

>public class FollowersList extends AppCompatActivity {

>&nbsp;&nbsp;&nbsp;&nbsp;ListView followList;
&nbsp;&nbsp;&nbsp;&nbsp;public String[] test;

<center>![listv4.PNG](https://cdn.steemitimages.com/DQmSi3ZcEd725Aqw4QFjgpVstHmybUZ6Y8i4xHRGGh1mbab/listv4.PNG)
<br>[Screenshot](https://cdn.steemitimages.com/DQmSi3ZcEd725Aqw4QFjgpVstHmybUZ6Y8i4xHRGGh1mbab/listv4.PNG)</center>


In the snippet of code below, we first set the address of our 'followlist' to the Listview contained in our FollowersList.java file. In the next line we create a Bundle, which is used to store the data that is sent along with an Intent, and we retrieve any incoming data.

>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;followList = (ListView) findViewById(R.id.FollowersListFOLLOWLIST);
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bundle usernameData = getIntent().getExtras();

Next we will need to ensure that nothing goes wrong, should we not be sending any data with our Intent, and simply launching the activity. You will see how this is handled in the following code snippet.

>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if (usernameData == null) {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;} else {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;String userData = usernameData.getString("Username");
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;String test[] = {userData};

In the code above, we first create an If Statement, stating that if the Bundle is null, to simply return, followed by an Else Statement, saying that if this Bundle is anything other than null, to create a new String 'userData', and set its value to the string, retrieved by our Bundle.getString() Function. finally, we create a String Array, and set the first object to the value of userData. The final two lines in this page, are in relation to our next and final section, our CustomAdapter.

>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ListAdapter adapter = new CustomAdapter(this, test);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;followList.setAdapter(adapter);
}

In the code above, we create a new list adapter, and set its value to a new Custom Adapter, containing the context, as well as our 'test' String array. In the 2nd final line, we attach our adapter, to our Listview.

## A Custom View: followers_row.xml

You will need to implement a new layout file, consisting of a horizontal LinearLayout, as well as a TextView, and a Button. This is rather simple, and the XML code can be found through the github link below. This layout file, will be used  over and over for each row, or piece of data displayed in your ListView. 

<center>![listrow.PNG](https://cdn.steemitimages.com/DQmPHswDfNXVMh6UAokCRXSC8ccUNzTYtR4DbtP5E2jzR4E/listrow.PNG)
<br>[Screenshot](https://cdn.steemitimages.com/DQmPHswDfNXVMh6UAokCRXSC8ccUNzTYtR4DbtP5E2jzR4E/listrow.PNG)</center>

This is a very quick file, and we will not be spending much time discussing it. Just know, that this is the design for each your in your list. You can choose to include more TextViews for more data to be displayed, or an ImageView, whatever pleases you really, but for the means of this tutorial, you will simply need one TextView, and one Button.

## The Custom Adapter: CustomAdapter.java

We need to create a new class file for this. You can go ahead and do that now. This class will also need to extend 'ArrayAdapter<String>', so that we can use all of its functions. You can see in the snippet below how this is done. The next three lines, are simply a constructor, that you must include with the use of this class. Create a variable for the context of the adapter like so, and include a String[] variable for us to pass in a String Array. This is what we do in the first line of the constructor. 

>public class CustomAdapter extends ArrayAdapter<String> {

>&nbsp;&nbsp;&nbsp;&nbsp;public CustomAdapter(@NonNull Context context, String[] items) {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;super(context, R.layout.followers_row, items);
&nbsp;&nbsp;&nbsp;&nbsp;}

Finally, we set the context of our adapter,  the layout file that we will be using for our custom row, and the name of the array that we would like to pass in. The last thing we will need to do in this tutorial, is override one major operation of our ArrayAdapter Class. This method is the getView() function, and is what will be used to get our custom view, and handle the widgets within it. Code is as follows.

 <center>![listv5.PNG](https://cdn.steemitimages.com/DQmakBwEZzcakzk2XJamBCFnXreKZEky6LqK1SWdWb7ff8k/listv5.PNG)
<br>[Screenshot](https://cdn.steemitimages.com/DQmakBwEZzcakzk2XJamBCFnXreKZEky6LqK1SWdWb7ff8k/listv5.PNG)</center>

The getView() Function can be overridden by going to Menu>Code>Generate, or pressing Alt+Insert. As you can see in the following two lines of code, which come immediately at the start of our getView(), our first line creates a LayoutInflater 'inflater', and set it to the current context. The second line, creates a new View, 'customView', and inflates it, using the layout file 'R.layout.followers', or followers.xml. The second bit of data included in this function, is a boolean set to false, representing that this is not the root layout.

>LayoutInflater inflater = LayoutInflater.from(getContext());
View customView = inflater.inflate(R.layout.followers_row, parent, false);

In the next snippet, the first line is responsible for creating a new String, and setting its value to the value of the item at that position in the String Array. The second and third line below, creates a TextView, as well as a Button, and sets its address to the appropriate addresses, directing them to our custom row layout file.

>String singleFollower = getItem(position);
TextView singleTV = (TextView) customView.findViewById(R.id.followNameROWTV);
Button unfollow = (Button)customView.findViewById(R.id.unfollowButtonROW);

The last two lines of this function, are responsible for setting the text, as well as returning the view. There is not much to explain here, other than that since we are using a custom layout file, we must ensure to use the widgets corresponding to that layout, which will be later inflated.

>singleTV.setText(singleFollower);
return customView;

## <center>Conclusion</center>

Well that is it for this tutorial guys! I will carry on just to show you around the new and improved 'steemie 2.0' :P but you have gotten through the bulk of the work. As you will see in the following pictures, we are now able to search for new users, as well as add them to a list of followers. 

![list2.PNG](https://cdn.steemitimages.com/DQmSRUvrDr1xxDUN1SqtK9DAxNRuQRwbv111wc61P21GCPC/list2.PNG)![list1.PNG](https://cdn.steemitimages.com/DQmf1nKA2wBRTntgFbFL8YttUXXpSu6bBTxavrTaV7Bo2Yi/list1.PNG)


There is still much to be done, as you can probably see from the following images, this Custom ListView activity that we have created, does not retain its memory once we have left the activity. At its current state, we are only able to retain user data provided we are within the activity. This can be solved numerous ways, from SavedPreferences(), to an SQLite database, though it is nearing 6 AM here, and I would much like to wrap this up for now. That will be explained in an upcoming tutorial.

I hope this has been an informative read for you all, and that I managed to explain all of my changes in detail, and did not confuse you to much. From a tutorial standpoint, I really should be giving these examples in a fresh application each time. We will start doing that eventually, but I really am enjoying working on this current application, and I hope you have been as well.

Happy Hunting,
Cerulean

![list4.PNG](https://cdn.steemitimages.com/DQmS7KADZzjbbST18ZXm3keGPCiqvXYGKmLDUR7TUQtXtRc/list4.PNG)![list3.PNG](https://cdn.steemitimages.com/DQmPLAkhSPn1mxp5JcVVUVps5EZcshKz4tQaguaQosCHGCx/list3.PNG)

#### Curriculum
This is the 3rd release in this series.

- [Android App Development Series #2 - Web Requests and Parsing JSON Objects](https://steemit.com/utopian-io/@ceruleanblue/android-app-development-web-requests-and-parsing-json-objects)
- [Android App Development Series #1 - Basic Web Browser](https://steemit.com/utopian-io/@ceruleanblue/android-app-development-series-1-basic-web-browser)


#### Proof of Work Done
Insert here the full u

############ CODE FOLLOWS BELOW ############ CODE FOLLOWS BELOW ############ CODE FOLLOWS BELOW ############ CODE FOLLOWS BELOW

# activity_followers.xml 

<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".followers">

    <TextView
        android:id="@+id/followersTVname"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignStart="@+id/followersTVid"
        android:layout_below="@+id/followTVbalanceSP"
        android:background="#ebfd4d"
        android:maxLength="9000"
        android:maxLines="300"
        android:text="name"
        android:textColor="@android:color/black"
        android:textSize="12sp" />

    <TextView
        android:id="@+id/followersTVid"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentStart="true"
        android:layout_alignParentTop="true"
        android:layout_marginStart="29dp"
        android:layout_marginTop="81dp"
        android:background="#ebfd4d"
        android:text="ID "
        android:textColor="@android:color/black"
        android:textSize="12sp" />

    <TextView
        android:id="@+id/followersTVmemo"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true"
        android:layout_alignStart="@+id/followersTVname"
        android:layout_marginTop="144dp"
        android:background="#ebfd4d"
        android:text="TextView"
        android:textColor="@android:color/black"
        android:textSize="12sp"
        android:visibility="invisible" />

    <ImageView
        android:id="@+id/imageView5"
        android:layout_width="169dp"
        android:layout_height="59dp"
        android:layout_alignBottom="@+id/imageView6"
        android:layout_toEndOf="@+id/imageView6"
        app:srcCompat="@drawable/cooltext295204046785396" />

    <ImageView
        android:id="@+id/imageView6"
        android:layout_width="176dp"
        android:layout_height="63dp"
        android:layout_alignParentStart="true"
        android:layout_alignParentTop="true"
        android:layout_marginStart="21dp"
        android:layout_marginTop="11dp"
        app:srcCompat="@drawable/cooltext295204660187728" />

    <TextView
        android:id="@+id/followTVbalanceSTEEM"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentEnd="true"
        android:layout_alignTop="@+id/followersTVid"
        android:layout_marginEnd="83dp"
        android:background="#ebfd4d"
        android:text="TextView"
        android:textColor="@android:color/black"
        android:textSize="12sp" />

    <TextView
        android:id="@+id/followTVbalanceSP"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true"
        android:layout_alignStart="@+id/followTVbalanceSTEEM"
        android:layout_marginTop="136dp"
        android:background="#ebfd4d"
        android:text="TextView"
        android:textColor="@android:color/black"
        android:textSize="12sp" />

    <TextView
        android:id="@+id/followTVbalanceSBD"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true"
        android:layout_alignStart="@+id/followTVbalanceSTEEM"
        android:layout_marginTop="184dp"
        android:background="#ebfd4d"
        android:text="TextView"
        android:textColor="@android:color/black"
        android:textSize="12sp" />

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentStart="true"
        android:layout_centerVertical="true"
        android:layout_marginStart="107dp"
        android:background="#ebfd4d"
        android:text="Search For User"
        android:textColor="@android:color/background_dark"
        android:textSize="24sp" />

    <EditText
        android:id="@+id/SearchBarFOLLOWERS"
        android:layout_width="285dp"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="163dp"
        android:ems="10"
        android:hint="Enter Username Here"
        android:inputType="textPersonName" />

    <Button
        android:id="@+id/SearchButtonFOLLOWERS"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_marginBottom="61dp"
        android:layout_toEndOf="@+id/followersTVid"
        android:text="Search User!" />

    <Button
        android:id="@+id/buttonFollowFOLLOWERS"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignStart="@+id/imageView5"
        android:layout_alignTop="@+id/SearchButtonFOLLOWERS"
        android:text="Follow User!" />

</RelativeLayout>

# FollowersRow.xml

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/followNameROWTV"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="TextView"
        android:textColor="@android:color/background_dark"
        android:textSize="18sp" />

    <Button
        android:id="@+id/unfollowButtonROW"
        android:layout_width="101dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="Remove From Following"
        android:textSize="12sp" />
</LinearLayout>

# activity_followers_list.xml

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/followNameROWTV"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="TextView"
        android:textColor="@android:color/background_dark"
        android:textSize="18sp" />

    <Button
        android:id="@+id/unfollowButtonROW"
        android:layout_width="101dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="Remove From Following"
        android:textSize="12sp" />
</LinearLayout>

# FollowersList.java

package com.example.smiey.steemie;

import android.content.SharedPreferences;
import android.preference.PreferenceManager;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.ArrayAdapter;
import android.widget.ListAdapter;
import android.widget.ListView;

import java.util.ArrayList;
import java.util.List;


public class FollowersList extends AppCompatActivity {

    ListView followList;
    public String[] test;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_followers_list);

        followList = (ListView) findViewById(R.id.FollowersListFOLLOWLIST);
        Bundle usernameData = getIntent().getExtras();
        if (usernameData == null) {
            return;
        } else {
            String userData = usernameData.getString("Username");
            String test[] = {userData};

            ListAdapter adapter = new CustomAdapter(this, test);
            followList.setAdapter(adapter);

        }
    }

}

# Followers.java

package com.example.smiey.steemie;

import android.content.Intent;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.ListView;
import android.widget.TextView;

public class followers extends AppCompatActivity {
    public static TextView dataTV;
    public static TextView nameTV;
    public static TextView memoTV;

    public static TextView balanceSteemTV;
    public static TextView balanceSPTV;
    public static TextView balanceSBDTV;

    Button searchButton;
    Button followButton;

    public static EditText searchBar;

    public static String userName;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_followers);

        nameTV= (TextView)findViewById(R.id.followersTVname);
        dataTV = (TextView)findViewById(R.id.followersTVid);
        memoTV = (TextView)findViewById(R.id.followersTVmemo);

        balanceSteemTV = (TextView)findViewById(R.id.followTVbalanceSTEEM);
        balanceSPTV = (TextView)findViewById(R.id.followTVbalanceSP);
        balanceSBDTV = (TextView)findViewById(R.id.followTVbalanceSBD);

        searchButton = (Button)findViewById(R.id.SearchButtonFOLLOWERS);
        followButton = (Button)findViewById(R.id.buttonFollowFOLLOWERS);

        searchBar = (EditText)findViewById(R.id.SearchBarFOLLOWERS);


        fetchdata process = new fetchdata();
        process.execute();

        searchButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                fetchdata process = new fetchdata();
                process.execute();


            }
        });

        followButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                  userName = searchBar.getText().toString();
                Intent i = new Intent(followers.this, FollowersList.class);
                i.putExtra("Username", userName);
                startActivity(i);

            }
        });

    }
}



//        ListView followList = (ListView)findViewById(R.id.followList);

# fetchdata.java

package com.example.smiey.steemie;

import android.os.AsyncTask;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;

import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.MalformedURLException;
import java.net.URL;

import javax.net.ssl.HttpsURLConnection;

public class fetchdata extends AsyncTask<Void,Void,Void> {

    String data = "";

    String id;
    String name;
    String memo;

    String balanceSteem;
    String balanceSP;
    String balanceSBD;

    String user = "ceruleanblue";

    @Override
    protected Void doInBackground(Void... voids) {

        {
            try {

                String getUser = followers.searchBar.getText().toString();

                if (getUser.isEmpty());
                else user = followers.searchBar.getText().toString();


                URL url = new URL("https://steemit.com/@"+user+".json");

                HttpsURLConnection httpsURLConnection = (HttpsURLConnection) url.openConnection();
                InputStream inputStream = httpsURLConnection.getInputStream();
                BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));

                String lines = "";
                while(lines != null){
                    lines = bufferedReader.readLine();
                    data = data + lines;
                }

                JSONObject jo = new JSONObject(data);
                JSONObject user = jo.getJSONObject("user");

                id = user.getString("id"); //like this so on
                name = user.getString("name");
                memo = user.getString("memo_key");

                balanceSteem = user.getString("balance");
                balanceSP = user.getString("vesting_shares");
                balanceSBD = user.getString("sbd_balance");

            } catch (MalformedURLException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            } catch (JSONException e) {
                e.printStackTrace();
            }
        }
        return null;
    }

    @Override
    protected void onPostExecute(Void aVoid) {
        super.onPostExecute(aVoid);

        followers.dataTV.setText("ID: \n"+this.id);
        followers.nameTV.setText("Username: \n"+this.name);
        followers.memoTV.setText("Memo Key: \n"+this.memo);

        followers.balanceSteemTV.setText("STEEM: \n"+this.balanceSteem);
        followers.balanceSPTV.setText("Vesting: \n"+ this.balanceSP);
        followers.balanceSBDTV.setText("SBD: \n"+this.balanceSBD);


    }

}


# CustomAdapter.java

package com.example.smiey.steemie;

import android.content.Context;
import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.TextView;

public class CustomAdapter extends ArrayAdapter<String> {

    public CustomAdapter(@NonNull Context context, String[] items) {
        super(context, R.layout.followers_row, items);
    }


    @NonNull
    @Override
    public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {
        LayoutInflater inflater = LayoutInflater.from(getContext());
        View customView = inflater.inflate(R.layout.followers_row, parent, false);

        String singleFollower = getItem(position);
        TextView singleTV = (TextView) customView.findViewById(R.id.followNameROWTV);
        Button unfollow = (Button)customView.findViewById(R.id.unfollowButtonROW);

        singleTV.setText(singleFollower);
        return customView;



    }
}

