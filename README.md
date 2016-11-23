Sample: Android-MQA-BlueList
===

The Android-MQA-BlueList sample builds upon the bluelist-base-android sample to integrate with the Mobile Quality Assurance service on BlueMix

After you run the sample and add your application key in the code, you can use Mobile Quality Assurance in the Android-MQA-BlueList sample to send bug reports, provide feedback, and write log messages to the Mobile Quality Assurance service. 

---
Downloading the Mobile Quality Assurance Android SDK
---

You can download the Latest Mobile Quality Assurance Android SDK from [Android SDK for download](http://www-01.ibm.com/support/knowledgecenter/SSJML5_6.0.0/com.ibm.mqa.uau.saas.doc/topics/c_AndroidSDKsForDownload.html) in IBM Knowledge Center.


Importing this sample into Android Studio
---

1. In Android Studio, click **File** > **Import Project....**
2. In the **Select Eclipse or Gradle Project to Import** wizard, select the bluelist-mqa-android project.
3. Click **OK**.

Adding the Mobile Quality Assurance Android libraries to the bluelist-mqa-android project
---

1. In Android Studio, click **File** > **New Module**.
2. In the **Create New Module** wizard, under **More Modules**, select **Import .JAR or .AAR Package**, and then click **Next**.
3. In the **File name** field, enter the name of the Mobile Quality Assurance SDK `.aar` file that you downloaded.

	*Tip: To browse to the file, click ... * 
4. Click **Finish**.

Add Mobile Quality Assurance to your mobile app
---

1. In Android Studio, click **File** > **Project Structure**.
2. In the Project Structure window, select the module for your app, and then select the **Dependencies** tab.
3. Click **+**, and then click **Module Dependency**.
4. In the Choose Modules window, select the Mobile Quality Assurance Android module that you imported, and then click **OK**. The Choose Modules window closes.
5. Click **OK** to close the Project Structure window.

Logging in or signing up in Bluemix 
---
Log in or sign up in [Bluemix](https://console.ng.bluemix.net/).

Creating a Mobile Quality Assurance service instance in Bluemix
---
Create an instance from the Bluemix dashboard.

1. From the Bluemix dashboard, click **ADD A SERVICE OR API** in the Service section.
2. In the Mobile section, click **Mobile Quality Assurance**.
3. Select the space that you want to add the Mobile Quality Assurance service to.
4. Change the default service name to a name that is specific to your service instance. Assigning a custom name to your service instance makes it easier to locate your service in Bluemix.
5. Select a plan for your service and review the terms of the plan, such as the financial details.
6. Click **CREATE**.

For more information about creating a Mobile Quality Assurance service instance, see [Getting started with mobile quality services for apps](http://www-01.ibm.com/support/knowledgecenter/SSJML5_6.0.0/com.ibm.mqa.uau.saas.doc/topics/t_setup_Mobile Quality Assurance_services_saas.html) in IBM Knowledge Center.

Adding a mobile app to the Mobile Quality Assurance service
---

1. Click the Mobile Quality Assurance service. 
2. In the tile that opens, click **New MQA App**.
3. Specify a name for the new Mobile Quality Assurance mobile app.

Adding an Android platform to the Mobile Quality Assurance mobile app
---

After you add the mobile app to the Mobile Quality Assurance service instance, you must prepare the app for reporting.
1. Click **Add Platforms**. 
2. Select the **Android platform** for your new mobile app, and then click **Submit**.

Obtaining the Mobile Quality Assurance App Key
---

Each mobile app platform has its own unique key.

1. In the Android platform tile, click **Show App Key**.

	*Tip: You can also retrieve the App Key by clicking the gear icon and selecting **Show App Key**.*
2. Copy the App Key. To copy the key, click the App Key text to select it, right-click the selected text, and then select **Copy**.
   	
Adding the Mobile Quality Assurance App Key to the Mobile Quality Assurance Android sample project
---

1. In Android Studio, open the `MainActivity.java` file of the bluelist-mqa-android project. 
2. Navigate to the following line, and replace the `Your-Application-Key-Goes-Here` variable with your Mobile Quality Assurance App Key.
		 public static final String APP_KEY = "Your-Application-Key-Goes-Here";

Getting to know the Mobile Quality Assurance libraries
---

1. To connect your mobile application to the Mobile Quality Assurance service, you must set up a Mobile Quality Assurance configuration and start a Mobile Quality Assurance session by using the configuration object in your mobile project.
	For example in the bluelist-mqa-android project, you can review the code in the `onCreate` event within the `MainActivity.java` class.

	a. Define a Mobile Quality Assurance configuration object. 
   		
   		//Your application key for Mobile Quality Assurance.
   		public static final String APP_KEY = "Your-Application-Key-Goes-Here"; 
   		
   		protected void onCreate(Bundle savedInstanceState) {
   		 	
	   	  Configuration configuration = new Configuration.Builder(this)
   		 	.withAPIKey(APP_KEY) //Provides the quality assurance application APP_KEY
   		 	.withMode(Mode.QA) //Selects the quality assurance application mode
	   	 	.withReportOnShakeEnabled(true) //Enables shake report trigger
	   	 	.withDefaultUser("default_user@email.com") //Sets a default user and user selection
	   	 	.build();   				
   		}	
   		
   **API key**

	A string that contains your unique application key for Mobile Quality Assurance.    
   		.withAPIKey(String APP_KEY) 
   **Preproduction mode or production mode** 

	Use this method to switch between preproduction mode (`Mode.QA`) and production mode (`Mode.MARKET`). 
   		.withMode(Mode.mode)
   **In-app bug reports**
	
	You can enable or disable bug reporting when users shake the device. 
    		.withReportOnShakeEnabled(true)
    		
   **Default user information**

	You can override the login process and have all users automatically connect with a default login.
   		.withDefaultUser("default_user@email.com") 
 
	b. Start a Mobile Quality Assurance session.
   		//Your application key for Mobile Quality Assurance.
   		public static final String APP_KEY = "Your-Application-Key-Goes-Here";	
   		
   		protected void onCreate(Bundle savedInstanceState) {
   			
	   		Configuration configuration = new Configuration.Builder(this)
   				.withAPIKey(APP_KEY) //Provides the quality assurance application APP_KEY
   				.withMode(Mode.QA) //Selects the quality assurance application mode
	   			.withReportOnShakeEnabled(true) //Enables shake report trigger
   				.withDefaultUser("default_user@email.com") //Sets a default user and user selection	
   				.build();
   			
   			MQA.startNewSession(MainActivity.this, configuration);  				
   		}	
	
	Call the `MQA.startNewSession()` method to start a new Mobile Quality Assurance session.
	
			MQA.startNewSession(final Context context, final Configuration configuration)	
	For more information about configuring a Mobile Quality Assurance session, see [Starting an Android session](http://www-01.ibm.com/support/knowledgecenter/SSJML5_6.0.0/com.ibm.Mobile Quality Assurance.uau.saas.doc/topics/t_StartingAnAndroidSession.html) in IBM Knowledge Center.

2. You can log to the Mobile Quality Assurance service instance session by using the Mobile Quality Assurance library. 

	Mobile Quality Assurance provides its own logging library, `com.ibm.mqa.Log`, which you can use instead of the standard Android logging library. This class mimics the standard `android.util.Log` library and has the same interfaces, including the `Log.v`, `Log.d`, `Log.i`, `Log.w`, and `Log.e` methods. The Mobile Quality Assurance logging methods send your data to the Logcat tool. 
	
	To send all logs through Mobile Quality Assurance, replace the `import android.util.Log;` import statement with this statement: `import com.ibm.mqa.Log;`.
	For example, complete the following steps in the bluelist-mqa-android project:
	
	a. Import the Mobile Quality Assurance Log library into the `MainActivity.java` class.
			import com.ibm.mqa.Log;
	b. Create a warning log message.
			Log.w("myTag", "This log message will be sent to Mobile Quality Assurance.");

3.  You can open the bug report screen from inside your own application code. 
	
	Opening the bug and feedback report screen is useful when your application uses the full screen and relies on shaking for its own input. By using this method, you can add your own user interface mechanism for starting a bug report.
	
	For example, in the bluelist-mqa-android project, the following line of code triggers Mobile Quality Assurance reports programmatically.
			MQA.showReportScreen();

For more information about the Mobile Quality Assurance Android features, see [IBM Mobile Quality Assurance for Bluemix](http://www-01.ibm.com/support/knowledgecenter/SSJML5_6.0.0/com.ibm.mqa.uau.saas.doc/Mobile Quality Assurance600saas_welcome.html) in IBM Knowledge Center.

Running the app
---

1. In Android Studio, click **Run** > **Run 'bluelist-mqa-android'**.
2. Choose a target from **Choose Device** wizard, and then click **OK**. The target can be an emulator or a physical device.
3. After the bluelist-mqa-android project is launched successfully, click the **Send Report** button to send a report to Mobile Quality Assurance. 

![BlueList Android App](https://github.com/VidyasagarMSC/Android-MQA-BlueList/blob/master/images/Screenshot_1479894637.png =100x20)
	<br>Mobile Quality Assurance opens the Send Report screen. You can report a bug or submit feedback to Mobile Quality Assurance from this screen.<br>
	
	
![Report a bug or Send feedback](https://github.com/VidyasagarMSC/Android-MQA-BlueList/blob/master/images/Screenshot_1479894641.png)
	
a. Click **REPORT A BUG** to create a bug report in Mobile Quality Assurance. You can complete the following actions in the Report a bug screen:
	- Describe the problem.
	- Edit the screenshot.
	- Click **Send** to submit the bug report to Mobile Quality Assurance.
	
![BlueList Android App](https://github.com/VidyasagarMSC/Android-MQA-BlueList/blob/master/images/Screenshot_1479894637.png)
	
 b. Click **GIVE FEEDBACK** to submit feedback to Mobile Quality Assurance. You can complete the following actions in the Give feedback screen:
	- Enter your feedback.
	- Rate the application.
	- Choose to include a screenshot.
	- Click **Send** to submit the feedback to Mobile Quality Assurance.

 *Tip: You can shake the physical device to open the Send report screen or use Command ⌘ + M when running in an Android emulator.*

Viewing the Mobile Quality Assurance session information in Bluemix
---

1. Log in to [Bluemix](https://console.ng.bluemix.net/).
2. From the **DASHBOARD** view, click your Mobile Quality Assurance service.
3. Expand your Mobile Quality Assurance application. Data is displayed in the Android Pre-production tile that represents the number of sessions, bugs, and feedback that you reported.
4. Click any of the following entries to view detailed information:
	- Click **Sessions** to open the Mobile Quality Assurance service page.
	- Click the most recent session to open the session page.
	- Scroll down the session page to view the log messages, bugs and feedback that you reported from the bluelist-mqa-android sample app.
