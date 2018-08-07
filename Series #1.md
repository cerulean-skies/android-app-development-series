#### <center>Android App Development Series #1 - Basic Web Browser</center>

#### Repository
e.g. https://github.com/utopian-io/utopian.io

#### What Will I Learn?

- You will learn how to create 'activities', as well as launch them using 'intents'.
- You will learn some of the basic functions of the WebBrowserView, along with how to interact with the view.
- You will learn how to use 'buttons', 'editTexts', and how to interact with all of these 'widgets' through the use of Java, as well as XML.

#### Requirements
State the requirements the user needs in order to follow this tutorial.

- Basic knowledge of Java, and XML.
- Android Studio installed on your operating system of choice.
- A device to run the application within, can be a physical device, or a VM like the one included in Android Studio's 'Device Manager'.

#### Difficulty
Choose one of the following options:

- Basic

#### Tutorial Contents
In this tutorial, we will be creating our first android application. During this, we will learn about widgets, and views, and how to use them to create a very basic application, consisting of a web browser, an editText, as well as a few buttons. The first thing we will need to do is start up our Android Studio IDEs, and create a new project, with a blank activity.

### The Home Page:
The home page of this application, will be our activity_main.xml file, unless you have chosen to rename this. Usually on a home page for an android application, you will want to have a good background image, as well as a few navigational buttons. Lets create those now. The following code can be used to create an ImageView, as well as 4 Buttons, when placed properly in your xml file.



><ImageView
        android:id="@+id/imageView"
        android:layout_width="match_parent"
        android:layout_height="255dp"
        android:layout_alignParentStart="true"
        android:layout_alignParentTop="true"
        app:srcCompat="@android:drawable/sym_def_app_icon" />
        
The last line of this can be changed/removed, should you want to use a different image or icon. This bit of code here will create one ImageView widget, located in the top of the screen. The following bit of code is used to create a button.

>    <Button
        android:id="@+id/buttonBrowser"
        android:layout_width="match_parent"
        android:layout_height="63dp"
        android:layout_alignParentStart="true"
        android:layout_below="@+id/imageView"
        android:layout_marginStart="0dp"
        android:layout_marginTop="6dp"
        android:text="BROWSER" />
        
It is now up to you, to create as many buttons as you like. I have created four, as I have some decent plans for this app that we are working on right now, but you will only need the one button to complete this tutorial with me. 

### The Browser Activity:
We will now need to create a new blank activity. This will be the activity that contains our web browser, and will be launched from our activity_main.

In this activity, we will be using 2 Buttons, as well as an EditText, and a WebBrowserView. I will show you how to set them up, in the following bits of code.

>   <WebView
        android:id="@+id/browser"
        android:layout_width="match_parent"
        android:layout_height="418dp"
        android:layout_alignParentBottom="true"
        android:layout_alignParentStart="true" />

>    <Button
        android:id="@+id/buttonBack"
        android:layout_width="190dp"
        android:layout_height="wrap_content"
        android:layout_alignParentStart="true"
        android:layout_alignParentTop="true"
        android:text="Back" />
        
So to break things up a bit, the first two sections of code, directly above this paragraph, are responsible for displaying both a WebView, as well as a Button. The following two sections, assemble another Button, and an EditText that we will use as our address bar.

>    <Button
        android:id="@+id/buttonForward"
        android:layout_width="192dp"
        android:layout_height="wrap_content"
        android:layout_alignParentEnd="true"
        android:layout_alignParentTop="true"
        android:text="Forward" />

>    <EditText
        android:id="@+id/adressBar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentStart="true"
        android:layout_below="@+id/buttonBack"
        android:ems="10"
        android:inputType="textPersonName"
        android:text="Address Bar" />
        
### Putting Together the Home Page - activity_main.java:

This .java file,is where we will store our code that will cause the actual interactions, between the user, the main UI, and the hardware. Our Home page is incredibly simple at the current moment, as this tutorial is simply designed to teach launching intents, and using the web browser, as well as a few other basic widgets. For now, all we will need to do on this page, is declare our Button that we will use to launch the activity, and then initialise what is known as an 'onClickListener', which will simply listen for when the button is pressed.

Once pressed, we want the button to create a new 'Intent', which is a way of sending data between activities/applications, and finally launch that intent, bringing us to our browser page. The following code can be used to do just this, when placed in the appropriate position, in the onCreate function of our activity_main.java files.

        Button buttonBrowse = (Button)findViewById(R.id.buttonBrowser);

        buttonBrowse.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent LaunchBrowser = new Intent (MainActivity.this, browser.class);
                startActivity(LaunchBrowser);

Once this code has been added, clicking on our Browser Button, will send us to our Browser activity. Unfortunately that does absolutely nothing at the current moment, so we will need to head over to our browser.java file, and set some things up there.

### Putting Together the Browser Activity - browser.java:
Alright, this is where things get a bit trickier. In our browser, we want it to be able to navigate back and forward, so we will need to set up some simple navigation buttons. We also would like to display the URL of the web page that we are currently visiting, so we will be using an EditText for that, though you could use another few different widgets to accomplish this feature. 

Aside from that, we will also need to have some control over our WebView. To interact with all of these Views/Widgets, we will need to initialize them in our .java file. The following four lines of code does just that, and should be placed at the beginning of the onCreate function - directly below setContentView.

        Button buttonBack = (Button)findViewById(R.id.buttonBack);
        Button buttonForward = (Button)findViewById(R.id.buttonForward);
        final EditText addressbar = (EditText)findViewById(R.id.adressBar);
        final WebView browser = (WebView)findViewById(R.id.browser);
        
In the next few lines of code, we will want to set up how our browser handles data, things like Javascript, Storage, etc. These lines can go directly below the previous four.

        final WebSettings ws = browser.getSettings();
        ws.setJavaScriptEnabled(true);
        ws.setDomStorageEnabled(true);
        browser.setOverScrollMode(WebView.OVER_SCROLL_NEVER);

This last paragraph gives us a settings object to work with, as well as allows us to use javascript, and sets scrolling capabilities, as well as another boolean we need to set true in order to retain javascript functionality across various situations.

After this has been set up, we will need to create a 'WebClient' and we will be using ChromeWebClient, as google=awesome, and I like chrome :P Aside from this, we will need to 'override' some of its main operations. The following bit of code can go below the previous, in your browser.java files.


        browser.setWebChromeClient(new WebChromeClient(){
        
            @Override
            public void onReceivedTitle(WebView view, String title) {
                super.onReceivedTitle(view, title);
                addressbar.setText(browser.getUrl().toString());
            }
        });
        
This last segment of code, creates/enables a 'WebClient', as well as overrides one of its main operations - the onReceivedTitle function. In this override, we set the text of the address bar whenever the function is called, with the new URL of the page we are visiting.

Next we want to ensure that the URL is set to the EditText, from the beginning of the onCreate call, and we can do that with a few simple lines as well.

        browser.loadUrl("https://steemit.com/@ceruleanblue");
        addressbar.setText(browser.getUrl().toString());
        
I figure this browser will only be designed to browse Steemit for now, as I mentioned earlier, I do have several plans for the end game of this application. Finally, we are at the last steps of this 'app' haha if you can call it that. We will need to add to a couple of onClickListeners, to our 'back', and 'forwards' buttons. The next two fragments of code, do exactly that.

        buttonBack.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View view) {
                if (browser.canGoBack()) {
                    browser.goBack();
                    addressbar.setText(browser.getUrl().toString());
                } else {
                    finish();
                }

            }
        });
        
        buttonForward.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View view) {
                if (browser.canGoForward()) {
                    browser.goForward();
                    addressbar.setText(browser.getUrl().toString());
                } else {
                    finish();
                }

            }
        });

When either of these buttons are selected during a run, the browser will now go back or forward, respectively to their associated buttons. 

### Conclusion:

If you have been following along with this tutorial, you have learnt a few different basics in android programming with java, such as the setText function, as well as how to implement onClickListeners, and much more. The premise for the widgets that you have learnt about in this tutorial, can usually be easily applied to other widgets of the same sort.

For now, we have a simple application, with some basic web browsing capabilities, but I have a few different ideas that could make this project into a kinda fun Steemit/Busy tool, depending how things play out from here. I hope this has been educational, and enjoyable for you beginners out there. Android programming is becoming more and more popular each day, so why not start learning now right? :)

Happy Hunting, 
Cerulean





#### Curriculum
This is the first release, in what I hope will be a rather long-running series of android application development.


#### Proof of Work Done
Insert here the full url of the code used in the tutorial, under your GitHub or a relevant gist, e.g. https://github.com/username/projname
© 2018 GitHub, Inc.

### FULL CODE BELOW ------- ------ ------ FULL CODE BELOW ------ ------- -------- FULL CODE BELOW

## activity_main.xml

<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="match_parent"
        android:layout_height="255dp"
        android:layout_alignParentStart="true"
        android:layout_alignParentTop="true"
        app:srcCompat="@android:drawable/sym_def_app_icon" />

    <Button
        android:id="@+id/button2"
        android:layout_width="match_parent"
        android:layout_height="60dp"
        android:layout_alignParentStart="true"
        android:layout_below="@+id/buttonBrowser"
        android:text="NOT YET" />

    <Button
        android:id="@+id/button3"
        android:layout_width="match_parent"
        android:layout_height="61dp"
        android:layout_alignParentStart="true"
        android:layout_below="@+id/button5"
        android:text="NOT YET" />

    <Button
        android:id="@+id/button5"
        android:layout_width="match_parent"
        android:layout_height="61dp"
        android:layout_alignParentStart="true"
        android:layout_below="@+id/button2"
        android:text="NOT YET" />

    <Button
        android:id="@+id/buttonBrowser"
        android:layout_width="match_parent"
        android:layout_height="63dp"
        android:layout_alignParentStart="true"
        android:layout_below="@+id/imageView"
        android:layout_marginStart="0dp"
        android:layout_marginTop="6dp"
        android:text="BROWSER" />
</RelativeLayout>

## activity_browser.xml

<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".browser">

    <WebView
        android:id="@+id/browser"
        android:layout_width="match_parent"
        android:layout_height="418dp"
        android:layout_alignParentBottom="true"
        android:layout_alignParentStart="true" />

    <Button
        android:id="@+id/buttonBack"
        android:layout_width="190dp"
        android:layout_height="wrap_content"
        android:layout_alignParentStart="true"
        android:layout_alignParentTop="true"
        android:text="Back" />

    <Button
        android:id="@+id/buttonForward"
        android:layout_width="192dp"
        android:layout_height="wrap_content"
        android:layout_alignParentEnd="true"
        android:layout_alignParentTop="true"
        android:text="Forward" />

    <EditText
        android:id="@+id/adressBar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentStart="true"
        android:layout_below="@+id/buttonBack"
        android:ems="10"
        android:inputType="textPersonName"
        android:text="Address Bar" />

</RelativeLayout>

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

        buttonBrowse.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent LaunchBrowser = new Intent (MainActivity.this, browser.class);
                startActivity(LaunchBrowser);
            }
        });
    }


        }

## browser.java

        package com.example.smiey.steemie;

        import android.support.v7.app.AppCompatActivity;
        import android.os.Bundle;
        import android.view.MotionEvent;
        import android.view.View;
        import android.webkit.WebChromeClient;
        import android.webkit.WebSettings;
        import android.webkit.WebView;
        import android.widget.Button;
        import android.widget.EditText;

        public class browser extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_browser);

        Button buttonBack = (Button)findViewById(R.id.buttonBack);
        Button buttonForward = (Button)findViewById(R.id.buttonForward);
        final EditText addressbar = (EditText)findViewById(R.id.adressBar);
        final WebView browser = (WebView)findViewById(R.id.browser);

        final WebSettings ws = browser.getSettings();
        ws.setJavaScriptEnabled(true);
        ws.setDomStorageEnabled(true);
        browser.setOverScrollMode(WebView.OVER_SCROLL_NEVER);

        browser.setWebChromeClient(new WebChromeClient(){


            @Override
            public void onReceivedTitle(WebView view, String title) {
                super.onReceivedTitle(view, title);
                addressbar.setText(browser.getUrl().toString());
            }
        });

        browser.loadUrl("https://steemit.com/@ceruleanblue");
        addressbar.setText(browser.getUrl().toString());


        buttonBack.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View view) {
                if (browser.canGoBack()) {
                    browser.goBack();
                    addressbar.setText(browser.getUrl().toString());
                } else {
                    finish();
                }

            }
        });


        buttonForward.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View view) {
                if (browser.canGoForward()) {
                    browser.goForward();
                    addressbar.setText(browser.getUrl().toString());
                } else {
                    finish();
                }

            }
        });

    }

        }



