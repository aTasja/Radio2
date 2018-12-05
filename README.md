Radio2 - link to GooglePlay - https://play.google.com/store/apps/details?id=ashatova.radio2
=====================================
App will allow to listen to radio stations. For this work device should be connected to the Internet. 
Initially the application is loaded with three radio stations. User will be bale to add, edit and delete radio station at will. 
Strams of all radio stations will be stored in SQLite database. User will see the list of all radio stations on the main screen of the app.
During installation, the application requests permission to access calls to monitor the status of the phone and turn off the radio during a phone conversation.
App have two languages English and Russian.

<img src="https://github.com/aTasja/Radio2/blob/master/splash.png"  height="330" width="170"> <img src="https://github.com/aTasja/Radio2/blob/master/radio_connecting.png"  height="330" width="170"> <img src="https://github.com/aTasja/Radio2/blob/master/settings.png" height="330"  width="170"> <img src="https://github.com/aTasja/Radio2/blob/master/edit.png" height="330"  width="170"> <img src="https://github.com/aTasja/Radio2/blob/master/add.png" height="330"  width="170"> 

App will have three screens:
---------------------------

I. After launching, user will have seen splash screen.

II. After splash screen user will have seen main screen with three loaded initially radios and buttons panel in the bottom of screen. Some of buttons are invisible in the current stage.

<img src="https://github.com/aTasja/Radio2/blob/master/panel_connecting.png"  width="250" height="40">
<img src="https://github.com/aTasja/Radio2/blob/master/panel_no_wifi.png"  width="250" height="40">

- Progress bar /is visible only while radio is connecting/.
- Play/stop 
- WiFi lost icon /is visible only while internet connection lost/.
- Settings 
- Exit 

III. Add/Edit screen is visible after pressing add or edit buttons on main screen. It allows to user enter title and stream of radio and save them for the App.

Behaviour:
----------
1. Pushing on radio and play button starts progress bar nad change play button to stop button.
2. Some time after user will see info message abowe buttons that radio is connected and hear radio.
	Progress bar is hidden. 
3. If the device loses connection with the network, the user will see WiFi lost icon.
	When the connection is restored the appropriate toast message will be showen, WiFi lost icon will became invisible and the radio will be reconnected.
4. If user will have pushed on stop button - the radio will switch off.
5. Pushing on setting button opens following options menu:
	- Add - allows to add new radio station to the app
	- Edit - allows to edit existing radio station
	- Delete - allows to delete existing radio station from the app
	- Share - allows to share radio title and stream of choosen radio station
6. If user select Add or Edit in settings menu, the latest screen will be shown. Here user can enter title ind stream of new radio. 
   After pushing on Save button App will save new radio station or changes of existing radio in the app.
   If URL will be invalid or entered title or stream exists already in the database, an appropliate toast will be shown.
7. Pushing on Exit button stops radio and close app.
8. While the radio is playing, the application responds to incoming calls. 
   At the moment of an incoming call, the radio stops. After the end of the call or conversation, the radio reconnects.
9. While the radio is playing a push notification is been showing on the top bar of device. 
   The notification  includes name of app and name of playing radio station. 
   If user push the notification, main screen of app will be shown. 
  
App have the following structure:
---------------------------------

### ---SplashActivity class extends Activity--- <br/>
- After installation, the application requests permission to access calls to monitor the status of the phone and turn off the radio during a phone conversation.
- Launches the app and send intent to RadioActivity class to start.<br/>

### ---MainActivity class extends Activity---  <br/>
- Launches main screen of app.<br/>
- Handle all button mode changes, including progress bar.
- Send intent to MediaPlayerService class to connect radio.
- Have two BroadcastReceivers (dynamically registered)
	a) to receive messages from Service about Radio connecting.
	b) to receive messages about Internet connectivity of device.
- Have one static BroadcastReceiver to receive messages about Phone status (IDLE, RINGING). 
- Stops service.<br/>

### ---MediaPlayerService class extends IntentService---  <br/>
- From starting intent gets radio stream and starts MediaPlayer.
- After connecting to radio station send local broadcast message to RadioActivity.
- Create Notification for top bar.

### ---EditActivity class---  <br/>
- Launches edit screen.<br/>
- Allows to enter new or change existing title and stram as new or in plase of selected radio station.
- If stream is valid and title and stream are unique for all app, radio station will be saved in database and MainActivity will starts again.
- Otherwice or if title or URL is empty user will see appropriate toast message and stay in the same screen.

### ---RadioRecord class---  <br/>
- Have setting and getting methods for radio station.

### ---RadioContract class - with inner class - RadioEntry class extends BaseColumns---  <br/>
- This contract defines the metadata for the App, including the provider's access URIs and its "database" constants.

### ---RadioDatabaseHelper class extends SQLiteOpenHelper---  <br/>
- The database helper used by the Radio Provider to create and manage its underlying SQLite database.

### ---RadioProvider class extends ContentProvider---  <br/>
- Implements a Content Provider used to manage Radio Stations in database.

### ---RadioOps class--- <br/>
- Support class that consolidates and simplifies operations on the RadioContentProvider.

### ---SettingsSpinnerAdapter class extends BaseAdapter--- <br/>
- Adapter class used to customize settings menu items to contain images.

### ---CallReceiver class extends BroadcastReceiver--- <br/>
- BroadcastReceiver to receive messages about Phone status (IDLE, RINGING). Send local broadcasts to MainActivity.




