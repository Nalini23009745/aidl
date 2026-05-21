# Ex.No:2 Develop an android application to implement the AIDL server and client app. The server app hosts a Bound Service and contains the logic to return random colours to its client.The client app calls the service and changes the button's colour within the Main activity.



## AIM:

To Develop an android application to implement the AIDL server and client app. The server app hosts a Bound Service and contains the logic to return random colours to its client.
The client app calls the service and changes the button's colour within the Main activity using AIDL interface in Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min.required Griaffe )

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as CSAIDL and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Display message give in MainActivity file(client/server).

Step 7: Save and run the application.

## PROGRAM:
```
/*
Program to print the client/server services using AIDL”.
Developed by:Nalini P
Registeration Number :2122232200663
*/
```

### activity_main.xml
```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical">

    <Button
        android:id="@+id/btnColor"
        android:layout_width="200dp"
        android:layout_height="100dp"
        android:text="CHANGE COLOR"/>
</LinearLayout>
```


### MainActivity.java
```
package com.example.pmdex2;

import android.content.ComponentName;
import android.content.Intent;
import android.content.ServiceConnection;
import android.os.Bundle;
import android.os.IBinder;
import android.os.RemoteException;
import android.view.View;
import android.widget.Button;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    IColorService colorService;
    Button btnColor;

    ServiceConnection connection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            colorService = IColorService.Stub.asInterface(service);
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {
            colorService = null;
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        btnColor = findViewById(R.id.btnColor);

        Intent intent = new Intent();
        intent.setAction("com.example.pmdex2.COLOR_SERVICE");
        intent.setPackage("com.example.pmdex2");

        bindService(intent, connection, BIND_AUTO_CREATE);

        btnColor.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (colorService != null) {
                    try {
                        int color = colorService.getRandomColor();
                        btnColor.setBackgroundColor(color);
                    } catch (RemoteException e) {
                        e.printStackTrace();
                    }
                }
            }
        });
    }
}
```
### ColorService.java
```
package com.example.pmdex2;

import android.app.Service;
import android.content.Intent;
import android.os.IBinder;
import android.os.RemoteException;

import java.util.Random;

public class ColorService extends Service {

    private final IColorService.Stub binder = new com.example.pmdex2.IColorService.Stub() {
        @Override
        public int getRandomColor() throws RemoteException {
            Random random = new Random();
            return 0xff000000 | random.nextInt(0x00ffffff);
        }
    };

    @Override
    public IBinder onBind(Intent intent) {
        return binder;
    }
}
```
### Android Manifest.xml
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="pmd ex2"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <service
            android:name=".ColorService"
            android:exported="true">
            <intent-filter>
                <action android:name="com.example.pmdex2.COLOR_SERVICE" />
            </intent-filter>
        </service>
    </application>

</manifest>
```


### lColorService.aidl
```
package com.example.pmdex2;

interface IColorService {
    int getRandomColor();
}

```
## OUTPUT

<img width="1920" height="1200" alt="Screenshot (78)" src="https://github.com/user-attachments/assets/9ea56a8c-e5b2-4e4d-96fe-10144565444e" />
<img width="1920" height="1200" alt="Screenshot (79)" src="https://github.com/user-attachments/assets/eb306d3e-d892-45e6-ac69-4df02abf4eab" />

<img width="1920" height="1200" alt="Screenshot (80)" src="https://github.com/user-attachments/assets/0f1a45e1-9375-4d2e-a380-0193827f36dc" />

<img width="1920" height="1200" alt="Screenshot (81)" src="https://github.com/user-attachments/assets/5d32128e-a404-4b6c-8d0f-3919643c83a4" />
<img width="1920" height="1200" alt="Screenshot (82)" src="https://github.com/user-attachments/assets/3e9f878b-b152-41bd-b45a-20104dfc6473" />


## RESULT
Thus a Simple Android Application to create a AIDL interface and communicate the process between client and server using AIDL interface in Android Studio is developed and executed successfully.
