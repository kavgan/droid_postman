# droid_postman
An android library to POST data over HTTP to any server very easily and flexibly.

HOW TO ADD THIS LIBRARY TO YOUR PROJECT:

1) Download the library droidpostman.jar (link: https://github.com/rohitpaniker/droid_postman)

2) copy the library to your project's /libs directory (Example: "C:\Users\<your-user-name>\AndroidStudioProject\YourProject\app\libs\droidpostman.jar")

3) Fire up Android Studio and open your project

4) Go to File -> Project Structure

5) Within Project Structure window click on your app name under "Module" section at your bottom left side.

6) Now click on "Dependencies" TAB

7) On your top right side you see a "+" (add) sign, click on the "+" sign. A dropdown will come up.

8) From the tiny dropdown, select "File Dependency", it will open a new window.

9) Double click on "libs" folder and double click the copied "droidpostman.jar"

10) Click "OK" at bottom right side

11) Go to build.gradle(Modile:app) file

12) paste this under dependencies: compile project(':droidpostman'). It should look like the below:

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile project(':droidpostman')
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:24.1.0'
}

13) top right corner: click on "Sync Now" 

Wait till the gradle build and compiles your project and the newly added library.



HOW TO USE THIS LIBRARY AND IT'S METHODS:

1) Go to your app's WhateverMainActivityName.java

2) add this import at top: import com.postman.droid.DroidPostMan;

3) Make your onCreate() look like this: 

    public String URL;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        URL = "http://www.someserver.com/postTest.php";

        sendPostRequest(URL);
    }
    
4) Write an AsyncTask routine outside your onCreate() and make it look exactly like this or write your own as per your need:


    private void sendPostRequest(String url) {

        class SendPostReqAsyncTask extends AsyncTask<String, Void, String> {
            private String serverResponse;
            final ProgressDialog progressDialog = new ProgressDialog(WhateverMainActivityName.this, ProgressDialog.STYLE_SPINNER);
            HashMap<String, String> map = new HashMap<>();

            @Override
            protected void onPreExecute() {
                progressDialog.setIndeterminate(true);
                progressDialog.setTitle("Authenticating");
                progressDialog.setMessage("Please wait while we are authenticating you....");
                progressDialog.show();


                map.put("KEY1", "VALUE1");
                map.put("KEY2", "VALUE2");
                map.put("KEY3", "VALUE3");

            }

            @Override
            protected String doInBackground(String... params) {
                serverResponse = DroidPostMan.sendDataToServer(URL, map); // send POST data to server
                return null;
            }

            @Override
            protected void onPostExecute(String result) {
                // TODO Auto-generated method stub
                progressDialog.dismiss();
                Log.e("Server Response", "--------------------------------" + serverResponse);
                if(serverResponse.equals("")){

                }
            }
        }

        SendPostReqAsyncTask sendPostReqAsyncTask = new SendPostReqAsyncTask();
        sendPostReqAsyncTask.execute(url);
    }


5) In the above AsyncTask you just have to change: 
  a) Parameters of map.put("KEY1", "VALUE1") and add as much KEY VALUE pairs as your PHP/Python/JAVA/Ruby script requires or expecting on server side. Example: 
                  map.put("KEY1", "VALUE1");
                  map.put("KEY2", "VALUE2");
                  map.put("KEY3", "VALUE3");
  
  b) before preExecute(), change this with your activitie's name: 
      ProgressDialog(WhateverMainActivityName.this, ProgressDialog.STYLE_SPINNER);
      
  c) in preExecute : change setTitle and setMessage with your custom message.
  

6) Within onPostExecute() you will get response from your script on the server, Handle the result with approproate actions you want after you get response from your script on server:

            @Override
            protected void onPostExecute(String result) {
                // TODO Auto-generated method stub
                progressDialog.dismiss();
                Log.e("Server Response", "--------------------------------" + serverResponse);
                if(serverResponse.equals("SOME_SERVER_VALUE")){

                }
            }
  
  
  
  
  Hope you guy's would love this work. In time i'll keep updating, improvising and maintaining this library.
  
  Any feedback or criticism is welcome.
