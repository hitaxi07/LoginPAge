# LoginPAge


<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.thecodecity.myloginapp">

    <uses-permission android:name="android.permission.INTERNET" />
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">

        <meta-data
            android:name="com.facebook.sdk.ApplicationId"
            android:value="@string/facebook_app_id" />

        <activity
            android:name="com.facebook.FacebookActivity"
            android:configChanges="keyboard|keyboardHidden|screenLayout|screenSize|orientation"
            android:label="@string/app_name" />
    </application>

</manifest>




<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center">

    <com.facebook.login.widget.LoginButton
        android:id="@+id/login_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Connect with FB"
        android:layout_marginBottom="30dp"
        android:layout_marginTop="30dp" />

</LinearLayout>

public class MainActivity extends AppCompatActivity {

    LoginButton loginButton;
    CallbackManager callbackManager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        loginButton = (LoginButton) findViewById(R.id.login_button);
        loginButton.setReadPermissions("email");
        callbackManager = CallbackManager.Factory.create();
        // Callback registration
        loginButton.registerCallback(callbackManager, new FacebookCallback<LoginResult>() {
            @Override
            public void onSuccess(LoginResult loginResult) {
                // App code
                Log.d("mylog", "Successful: " + loginResult.getAccessToken());
                Log.d("mylog", "User ID: " + Profile.getCurrentProfile().getId());

  Log.d("mylog", "User Profile Pic Link: " + Profile.getCurrentProfile().getProfilePictureUri(500, 500));
            }

            @Override
            public void onCancel() {
                // App code
            }

            @Override
            public void onError(FacebookException exception) {
                // App code
                Log.d("mylog", "Successful: " + exception.toString());
            }
        });
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

 

        callbackManager.onActivityResult(requestCode, resultCode, data);
    }
}


private void performGraphRequest(LoginResult loginResult) {
        GraphRequest request = GraphRequest.newMeRequest(loginResult.getAccessToken(), new GraphRequest.GraphJSONObjectCallback() {
            @Override
            public void onCompleted(JSONObject json, GraphResponse response) {
                // Application code
                if (response.getError() != null) {
                    System.out.println("ERROR");
                } else {
                    System.out.println("Success");
                    String jsonresult = String.valueOf(json);
                    Log.d("mylog", "JSON Result" + jsonresult);

                    String fbUserFirstName = json.optString("name");
                    String fbUserEmail = json.optString("email");
                    String fbBirthday = json.optString("birthday");
                }
                Log.v("FaceBook Response :", response.toString());
            }
        });
        Bundle parameters = new Bundle();
        parameters.putString("fields", "id,name,email,gender, birthday");
        request.setParameters(parameters);
        request.executeAsync();
    }

