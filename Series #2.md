Android App Development - Web Requests and Parsing JSON Objects  

#### Repository
https://github.com/aosp-mirror/platform_build

#### What Will I Learn?

- You will learn how to make http/https web requests to a  server.
- You will learn how to parse JSON objects, as well as to display the parsed data.

#### Requirements

- Basic knowledge of Java, and XML.
- Android Studio installed on your operating system of choice.
- A device to run the application within, can be a physical device, or a VM like the one included in Android Studio's 'Device Manager'.

#### Difficulty

- Basic

#### Tutorial Contents

## Introduction
In this tutorial, we will be learning to make requests to a website, and retrieve and parse JSON Object Data. We will be carrying on with our previous project, and for the purposes of our application, we will be extracting user data from Steemit.com. Finally, this data will be displayed within a 3rd activity. 

We should have two activities already existing in our project from before which will make this much simpler, though this can be done in a new project as well, should you prefer. If you are carrying on from the previous tutorial, simply create a new empty activity now. I have named mine 'activity_followers.xml'. 

In this activity, we will need several different text views, essentially in regards to how many bits of data you would like to display to the user. I have used 6 Textviews, as well as 2 Image views on my 'followers.java' XML file.

## Setting the Layout: activity_followers.xml

The following code is some XML that can be used to create both an ImageView, as well as a TextView, when entered into your activity_followers.xml Layout File.

    <ImageView
        android:id="@+id/imageView5"
        android:layout_width="259dp"
        android:layout_height="72dp"
        android:layout_alignParentBottom="true"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="200dp"
        app:srcCompat="@drawable/cooltext295204046785396" />
            
    <TextView
        android:id="@+id/followersTVname"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true"
        android:layout_alignStart="@+id/followersTVid"
        android:layout_marginTop="124dp"
        android:background="#ebfd4d"
        android:maxLength="9000"
        android:maxLines="300"
        android:text="name"
        android:textColor="@android:color/black"
        android:textSize="18sp" />
        
As I mentioned above, you will want 6 TextViews, and 2 ImageViews to carry along with this tutorial. The full code can be found in the link below to the entire project hosted on github. For now, lets carry on with the steps. Next we will need to create another activity. 

## aSyncTask: Running Tasks in Background

This will be slightly different from what we have done in the past, and this is because we will not be using this 'activity' to display data to the screen. We will instead be extending the aSyncTask class, so that we can have some code run in the background. This is where we will fetch, and parse our JSON data. First, create a new empty activity, and call it something like 'fetchdata.java'.

We will not need to implement anything in this layout file, as we will not be setting any 'Views'. This activity is strictly to be run in the background of our application, and set this up now.

Once we have created our class, we now need to override a couple of its functions. We will be using the doInBackground(), and the onPostExecute methods, so you can override these now. First we will deal with the doInBackground() function.

## Making Web Requests:

Now, in this application and series, I really want to try and test the different things that we are able to do with the steem blockchain, as I'm writing to it, you're reading from it, and we should all be doing what we can to add value to this platform. I hope that these tutorials, help all of you readers get comfortable and start testing your own things with steem, especially on Mobile, as there are very few android applications out as of yet. :)

For the reasons listed above, we will be using Steemit as our website of choice, which we will be requesting information from. The first thing we are going to need are some variables. I am just going to give them all to you right now, though they will not all be used immediately. First we will need to create 9 variables like so.

    String data = "";
    String dataParsed = "";
    String singleParsed = "";

    String id;
    String name;
    String memo;

    String balanceSteem;
    String balanceSP;
    String balanceSBD;
    
After this has been done, we will move to our doInBackground() function. This is of course, everything that we want to be done 'in the background' of our application. Yours should look much like mine below,though you can change the variables as you like.

    @Override
    protected Void doInBackground(Void... voids) {

        {
            try {
                URL url = new URL("https://steemit.com/@ceruleanblue.json");
                
                HttpsURLConnection httpsURLConnection = (HttpsURLConnection) url.openConnection();
                InputStream inputStream = httpsURLConnection.getInputStream();
                BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));

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

Now to briefly explain this previous segment, surrounded in a try/catch clause, we first set our URL variable, then create an HttpsURLConnection, in order to make the connection, as well as an InputStream for accessing the data byte by byte, and a BufferedReader daisy-chained through the InputStream. Finally we have all of our catch clauses, in case anything goes wrong during runtime.

## Parsing JSON Object Data:

Now we will need to add just a few more lines of code, directly above our catch clauses, and directly below the 'BufferedReader bufferedReader' line in our fetchdata.java file. (Complete code linked below)


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
                        
Again, lets go through what just happened in the most previous code segment. First we create a String, and then feed the data from our BufferedReader, into our String. After this, we create a new JSON object out of said String.

Unfortunately, the way the data is recieved, we will need to extract yet another JSON Object, and thats exatcly what we do when we create the 'JSONObject user'.  Now that we have queried our second object, we can extract any particular strings we like, using their name in JSON format, and that is what the last 6 lines of this are from.

## Setting up our Activity: followers.java

Lets leave that last page alone for now, as that is the end of our doInBackground() function anyway. Lets move back to our followers.java file, and initialize the widgets that we need to use to display the data. As I have mentioned, I will be using 6 TextViews, displaying 'id, name, memo_key, balance, vesting_shares, and sbd_balance'. First lets initialize our widgets, and make them public for other activities to be able to use.

    public class followers extends AppCompatActivity {
        public static TextView dataTV;
        public static TextView nameTV;
        public static TextView memoTV;

        public static TextView balanceSteemTV;
        public static TextView balanceSPTV;
        public static TextView balanceSBDTV;

Now lets carry on with our onCreate() function. This is where we set our widgets, and execute our aSyncTask activity. We execute this code here, so that no data is transfered between activities.

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



        fetchdata process = new fetchdata();
        process.execute();


    }
    }

We are getting fairly close to the end result of this tutorial, but that is it for our followers.java file, though there are many things that we can do with this data now that we have it. Lets head back over to our followers.java file, and finish up our onPostExecute() function.

## Finishing up, Displaying the Data:

In this function, we will simply be taking the data that we have retrieved from steemit.com, and displaying it to the UI of our followers activity. We will be using several iterations of the 'setText()' method to do so.

    @Override
    protected void onPostExecute(Void aVoid) {
        super.onPostExecute(aVoid);

        followers.dataTV.setText("ID: "+this.id);
        followers.nameTV.setText("Username: "+this.name);
        followers.memoTV.setText("Memo Key: "+this.memo);

        followers.balanceSteemTV.setText("STEEM Balance: "+this.balanceSteem);
        followers.balanceSPTV.setText("Vesting Balance: "+ this.balanceSP);
        followers.balanceSBDTV.setText("SBD Balance: "+this.balanceSBD);

    }
    
    
## Finishing Up, Implementing the onClick Listener:

I almost forgot! We must implement an onClickListener in order to launch our followers activity from within the home screen. quickly do that now in Main_Activity.java.

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button buttonBrowse = (Button)findViewById(R.id.buttonBrowser);
        Button buttonFollowers = (Button)findViewById(R.id.buttonFollow);


        buttonBrowse.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent LaunchBrowser = new Intent (MainActivity.this, browser.class);
                startActivity(LaunchBrowser);
            }
        });

        buttonFollowers.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent LaunchFollowers = new Intent (MainActivity.this, followers.class);
                startActivity(LaunchFollowers);
            }
        });
    }


    


## Conclusion

That is pretty much it guys! You can go ahead and mess with colors, size, image and more, but the code at this pont, is solid and ready to run. Lets give it a run together now. When in the home screen, selecting our followers button will now display some quite useful information to us.

And this is really just the beginning of what we can do with this type of data. I have quite a few plans for the future, but feel free to play around with this on your own, see what kind of things you can make out of this. Theres alot of data there already, and you can change the url easily to your account or anything else. I hope this has been a good read for you all, and that you got some good ideas from this project. 

Happy Hunting,
Cerulean



### FULL CODE BELOW ----- ------ ------ FULL CODE BELOW ------- ------  -------- FULL CODE BELOW --------------------------

## MainActivity.java

    package com.example.smiey.steemie;

    import android.content.Intent;
    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import android.view.View;
    import android.widget.Button;

    public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button buttonBrowse = (Button)findViewById(R.id.buttonBrowser);
        Button buttonFollowers = (Button)findViewById(R.id.buttonFollow);


        buttonBrowse.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent LaunchBrowser = new Intent (MainActivity.this, browser.class);
                startActivity(LaunchBrowser);
            }
        });

        buttonFollowers.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent LaunchFollowers = new Intent (MainActivity.this, followers.class);
                startActivity(LaunchFollowers);
            }
        });
    }


}


## fetchdata.java

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
    String dataParsed = "";
    String singleParsed = "";

    String id;
    String name;
    String memo;

    String balanceSteem;
    String balanceSP;
    String balanceSBD;

    @Override
    protected Void doInBackground(Void... voids) {


        {
            try {
                URL url = new URL("https://steemit.com/@ceruleanblue.json");

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

        followers.dataTV.setText("ID: "+this.id);
        followers.nameTV.setText("Username: "+this.name);
        followers.memoTV.setText("Memo Key: "+this.memo);

        followers.balanceSteemTV.setText("STEEM Balance: "+this.balanceSteem);
        followers.balanceSPTV.setText("Vesting Balance: "+ this.balanceSP);
        followers.balanceSBDTV.setText("SBD Balance: "+this.balanceSBD);

    }
    
    
## followers.java

    package com.example.smiey.steemie;

    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import android.widget.ListView;
    import android.widget.TextView;

    public class followers extends AppCompatActivity {
    public static TextView dataTV;
    public static TextView nameTV;
    public static TextView memoTV;

    public static TextView balanceSteemTV;
    public static TextView balanceSPTV;
    public static TextView balanceSBDTV;




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



        fetchdata process = new fetchdata();
        process.execute();


    }
    }






