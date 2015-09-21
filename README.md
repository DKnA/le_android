Logging support for Android devices
===================================

[![License](https://img.shields.io/badge/license-MIT-blue.svg?style=flat)](https://github.com/mdp/rotp/blob/master/LICENSE)

Build requirements: Android SDK 2.3+
Runtime requirements: Android OS 2.3+

Features:

- Send Logs via HTTP POST

	Option of changing from Token TCP to using HTTP POST sending to the endpoint 'https://js.logentries.com/v1/logs/LOG-TOKEN'

- Send Logs via SSL

	The library can send logs via Token TCP over TLS/SSL over port 443 by default

- Storing logs offline and sending when connected.  

	While sending logs, if the device looses connection, logs are stored locally until a connection is reestablished then sent to Logentries. 10mb queue limit

- TraceID

	Each log sent contains the device TraceID which is a unique 35 character ID.


Setup
-----

Set up an account with Logentries at <https://logentries.com/>, and create a logfile, by clicking + Add New button and selecting the Manual Configuration Option at the bottom. Select Token TCP as the source type and copy the Token UUID printed in green.

Next, download the library jar file [here](https://github.com/logentries/le_android/raw/master/lib/logentries-android-2.1.4.jar) and place it in the /yourProject/libs folder of your Android project.

Add the android.permission.INTERNET <uses-permission> to the project manifest file.

Use
---

In the desired Activity class, ``import com.logentries.android.AndroidLogger``

To create an instance of the Logger object in an Activity class:
	AndroidLogger logger = AndroidLogger.getLogger(Context context, boolean useHttpPost, boolean useSsl, boolean isUsingDataHub, String dataHubAddr, int dataHubPort,
                          String token, boolean logHostName);
Where

 - context: for example, if in an Activity class, use ``getApplicationContext()``, or if in an Application class, use ``getBaseContext()``.

 - token: is the Token UUID we copied earlier which represents the logfile on Logentries



Log events are created using the following methods of the AndroidLogger class, which differ only in terms of the severity level they report:

 - severe, debug, info, config, fine, finer, finest, verbose

Eg: ``logger.error("Log Event Contents");`` creates the log ``Sat Jul 30 16:04:36 GMT+00:00 2011, severity=ERROR, Log Event Contents``.

Each method corresponds to those used in android.util.Log and java.util.logging.Logger.


## DataHub Support


Additional constructors for AndroidLogger have been created for use with Logentries DataHub.
There are two additional constructors added, one with a customID and one without customID.  They are respectively:

	  AndroidLogger logger = AndroidLogger.getLogger(Context context, String datahub_address, int datahub_port);
and

 	 AndroidLogger logger = AndroidLogger.getLogger(Context context, String datahub_address, int datahub_port, String customID);

 - context: for example, if in an Activity class, use ``getApplicationContext()``, or if in an Application class, use ``getBaseContext()``.

 - datahub_address: is a String of the IP Address of your DataHub machine.

 - datahub_port: is an int of the port number of your incoming connection on your DataHub machine.  The default is port 10000, but this can be changed
to any port by altering the /etc/leproxy/leproxyLocal.config file on your DataHub machine and restarting the leproxy daemon using "sudo service leproxy restart".

- customID - is a String of the customID you may wish to add to your log events.  


NOTE: You cannot use both Token-based and DataHub-based AndroidLogger constructors in your application.  You can only use one or the other.
