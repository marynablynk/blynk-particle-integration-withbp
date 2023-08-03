# Blynk IoT
Particle has teamed up with Blynk to create a webhook integration with the [Blynk IoT platform](https://blynk.io/).

Blynk is a low-code IoT software platform for connecting devices to the cloud, building mobile apps to remotely control and monitor them, and managing users and IoT devices at any scale. To vizialize data and ineract with devices Blynk offers native mobile apps for iOS and Android and the web dashboard, that can be built with drag-and-drop constructor, eliminating the need to write code for the front end. Blynk also includes built-in functionality for over-the-air firmware updates, device provisioning, advanced device and user management tools, alerts and notifications, automations, and data analytics.  

![alt text](https://raw.githubusercontent.com/marynablynk/blynk-particle-integration/main/images/blynk-particle-logos.png "Particle Blynk Logo")

This integration enables bi-directional communication between any Particle hardware and Blynk IoT. It allows visualization of the data and control of the Particle device remotely via the Blynk web and mobile dashboard.

# Key concepts in Blynk IoT

### Device Template  

It is a collection of configurations shared by devices of a similar type.  

Consider smart home switches as an example. These devices typically perform comparable functions and often have the same data model, GPIO assignments, firmware code, etc. Instead of editing each device individually when changes are required, you can modify a Device Template, and all associated devices will be updated accordingly.  

### AuthToken, Template ID, Template Name

**AuthToken** - every device on the Blynk IoT platform has an AuthToken (OAuth Token), also often called a Device Token. This is a unique identifier of the device and it's used to authenticate, validate, and connect devices to Blynk.Cloud. AuthToken is stated in the firmware code and is flashed to the device before it is delivered to the end customer, or assigned to the device during device activation by the end user in case of dynamic provisioning.

**Template ID** - each Device Template possesses a Template ID, a unique identifier that enables Blynk to recognize the device type and link it to the relevant template elements.

**Template Name** - a unique name that is used during Device check and connection and as a default Device name. 

### Datastreams 
These are channels for time-stamped data transmitted between the device and the cloud. For example, sensor data should pass through a Datastream. 

### Widgets
Pre-designed UI elements, UI elements for visualizing device data and interacting with devices accessible to users.


# Setting up integration with Blynk IoT

## Sign up for a Blynk account

If you don't already have one, you'll need to sign up for a Blynk account to use this integration.

Go to [Blynk Console](https://blynk.cloud/) to sign in or to create a new account. A Free account is available for the platform exploration. To access more features for advanced personal or commercial use, check out the Blynk [pricing page](https://blynk.io/pricing) to learn more about subscription options.

## Configure Blynk Template

### Create a Template
In the Blynk.Console, navigate to **Templates** > **My Templates** and click on the **New Template** button at the upper right of the page (make sure the [Developer Mode](https://docs.blynk.io/en/concepts/developer-mode) is enabled). Give the template a name such as 'ParticleDeviceIntegration', set the **Hardware** field to Particle, and choose the appropriate **Connection type** of GSM for cellular devices, or WiFi and click **Done**.

### Configure Datastreams
Go to the **Datastreams** tab in the Template you created. Click **New Datastream** > **Virtual Pin** and configure five datastreams as shown in the Datastream Settings table that follows. It is important to configure the Pin, Data Type, Is Raw, Min, Max, and Default Value as shown. Click the **Save** button at the upper right of the screen when all of the datastreams have been defined. 

Blynk Datastreams are bi-directional channels assigned a data type and link to data values stored on the Blynk Cloud. You reference them as virtual pins between the range of V0 and V255. 

<table>
  <caption>Datastream Settings</caption>
  <tr>
    <th>Datastream <br/> Virtual Pin</th>
    <th>Data Type</th>
    <th>Values</th>
    <th>Comment</th>
  </tr>
  <tr>
    <td>V6</td>
    <td>String</td>
    <td>last_publish</td>
    <td>The Last date/time in UTC data was published from the Particle hardware. Updated by the Particle webhook.
    </td>
  </tr>
  <tr>
    <td>V14</td>
    <td>integer</td>
    <td>millis()</td>
    <td>Simulated sensor data generated by the firmware (C++ millis() function). Could be adapted to represent actual sensor data.
    </td>
  </tr>
  <tr>
    <td>V15</td>
    <td>double</td>
    <td>3.14159</td>
    <td>Simulated sensor data generated by the firmware. A constant floating point value of 3.14159. Could be adapted to represent actual sensor data.</td>
  </tr>
  <tr>
    <td>V16</td>
    <td>integer</td>
    <td>0/1</td>
    <td>Controlled by switch widget on Blynk web dashboard and mobile app. Changes in the datastream value will be received by the hardware and then the state of the built-in LED will be changed (if available), and the state of the LED widget on the Blynk web dashboard and mobile app will be updated (V17).</td>
  </tr>
  <tr>
    <td>V17</td>
    <td>integer</td>
    <td>0/1</td>
    <td>LED widget on Blynk web dashboard and mobile app that echos changes received by the hardware for V16. 
	  </td>
  </tr>
</table>

The datastreams should appear like on the image below when properly configured (fields Pin, Data Type, Is Raw, Min, Max, and Default Value must match exactly).

![alt text](https://raw.githubusercontent.com/marynablynk/blynk-particle-integration/main/images/datastreams.png "Datastreams")


### Create a Web Dashboard
The [Web Dashboard](https://docs.blynk.io/en/blynk.console/templates/dashboard) allows you to visualize data from a device and control the device using [Widgets](https://docs.blynk.io/en/blynk.console/widgets-console) (GUI elements). 

1. In the **Web Dashboard** tab click the **Edit** button at the upper right of the page. 
2. Drag and drop the following widgets to the dashboard (left to right, top to bottom) using the layout shown in the image that follows: Label, Chart, Label, Chart, Switch, LED, Label.  
3. Configure each widget as shown in the image that follows. 
4. After all widgets are configured, click **Save and Apply**. 

**Dashboard layout**
![alt text](https://raw.githubusercontent.com/marynablynk/blynk-particle-integration/main/images/dashboard.png "Dashboard layout")

**Widgets settings**
![alt text](https://raw.githubusercontent.com/marynablynk/blynk-particle-integration/main/images/widgets-h.png "Widget settings")

### Create a Mobile Dashboard
The Blynk app in [Developer Mode](https://docs.blynk.io/en/blynk.apps/overview#developer-mode) enables you to interactively build a custom app by selecting widgets and then configuring them. When the app is in [End-user Mode](https://docs.blynk.io/en/blynk.apps/overview#end-user-mode) it will present the app with widgets to the user with a behavior just like any other native app.

Using the [Mobile Dashboard Editor](https://docs.blynk.io/en/blynk.apps/constructor) create a UI layout. The exact steps vary between iOS and Android - begin by tapping on the Particle device you activated earlier, tap on the wrench icon, and then the **+** icon to add a widget. Add the following widgets: SuperChart (datastreams V14, V15), Labeled Value (V14), Labeled Value (V15), Button (V16), LED (V17), Labeled Value (V6).

Tap and hold your finger on a widget and drag it to the desired position. Use green handles to resize the widget. 

After the widgets are added to the dashboard canvas, you can tap on it to configure it. Every widget has **Settings** and **Design** options at the bottom of the screen. Configure each widget similarly to the dashboard widgets.

## Firmware 

Any Particle hardware (Tracker One, Tracker SoM, Boron, B Series SoM, Photon 2, P2, Argon, Photon, Electron, E Series, Core) running the provided firmware will be sending two channels of simulated sensor data from the hardware to Blynk.  

One channel will be integer values, and the other will be a floating point value. The data sent will be visualized on the Blynk web dashboard and mobile app in both a chart and a value display. Additionally, a switch widget on the web dashboard and mobile app will send data to the hardware to control it. The switch data is simply an On/Off (1/0) state that will be sent back to Blynk by the firmware to control a Blynk LED widget, and it will toggle the state of the built-in LED on the Particle device if it exists. A UTC-based timestamp will also be displayed on the web dashboard and mobile app so the last time data was published from the Particle device will be known.

```
#include "Particle.h"

const char* firmware_version = "0.0.0";

double v15 = 3.14159;
uint8_t led_state = LOW;
bool particle_fn_called = TRUE;	// causes device to publish data immediately after started/boot and connected to Particle cloud.

// Register the Particle cloud function
int blynkLED(String on_or_off);

/////////////////////////////////////////////////////////////////////////
// Blynk

// Update below with your Blynk auth token for your device (automatically populated by Blueprint)
#define BLYNK_TEMPLATE_ID "13 char template id"
#define BLYNK_TEMPLATE_NAME "ParticleDeviceBlueprint"
#define BLYNK_AUTH_TOKEN "your Blynk 32 char auth token"

/////////////////////////////////////////////////////////////////////////


bool deviceHasLedOnD7() {
  // Returns TRUE if the device has a built-in LED on D7:
  //  Boron, Argon, Photon 2, Photon, Electron, Core
  // 8: P1
  switch (PLATFORM_ID) {
    case PLATFORM_BORON:
    case PLATFORM_ARGON:
    case 0:  // Core
    case 6:  // Photon  (PLATFORM_PHOTON_PRODUCTION)
    case 10: // Electron  (PLATFORM_ELECTRON_PRODUCTION)
      return TRUE;
    default:
      return FALSE;
  }
} // deviceHasLedOnD7()

/////////////////////////////////////////////////////////////////////////
// Timer

const uint32_t TIMER_INTERVAL_MS = 300000L;
uint32_t timer_last = 0;

void pubToParticleBlynk() {
  if (Particle.connected()) {
    char data[90]; // See serial output for the actual size in bytes and adjust accordingly.
    // Note the escaped double quotes around the ""t"" for BLYNK_AUTH_TOKEN.  
    snprintf(data, sizeof(data), "{\"t\":\"%s\",\"v14\":%u,\"v15\":%f,\"v16\":%u,\"v17\":%u}", BLYNK_AUTH_TOKEN, millis(), v15, led_state, led_state);
    Serial.printlnf("Sending to Blynk: '%s' with size of %u bytes", data, strlen(data));
    bool pub_result = Particle.publish("blynk_https_get", data, PRIVATE);
    if (pub_result) {
      timer_last = millis();
    } else {
      Serial.println("ERROR: Particle.publish()");
    }
  }
} // pubToParticleBlynk()


void pubTimer() {
  // A timer for publishing data to Particle Cloud, and then continuing to Blynk.
  if (timer_last > millis())  timer_last = millis();
  if ((millis() - timer_last) > TIMER_INTERVAL_MS && Particle.connected()) {
    pubToParticleBlynk();
    timer_last = millis();
  }
} // pubTimer()


/////////////////////////////////////////////////////////////////////////


void setup() {

  if (deviceHasLedOnD7() == TRUE) {
    pinMode(D7, OUTPUT);
    digitalWrite(D7, LOW);
  }

  Serial.begin(9600);
  waitFor(Serial.isConnected, 30000);
  delay(1000);
  Serial.printlnf("Device OS v%s", System.version().c_str());
  Serial.printlnf("Free RAM %lu bytes", System.freeMemory());
  Serial.printlnf("Firmware version v%s", firmware_version);

  // register the Particle cloud function (funcKey, funcName)
  Particle.function("blynk_led", blynkLED);

  Serial.println("Setup complete");

} // setup()



void loop() {
  
  pubTimer();

  if (particle_fn_called == TRUE) {
    particle_fn_called = FALSE;
    // Publish data to Particle cloud..
    pubToParticleBlynk();
  }
  
  if (deviceHasLedOnD7() == TRUE) {
    digitalWrite(D7, led_state);
  } 

} // loop()


int blynkLED(String on_off) {
  // Custom Particle cloud function that changes the state of the built-in LED
  // on D7 in response to an instruction from Blynk calling this
  // custom cloud function. 
  // Returns the value 1 if the LED has been turned on, and 0 if turned off, 
  // -1 if an unexpected on_off value is received.
  // Cloud functions must return int and take one String argument
  // curl https://api.particle.io/v1/devices/{your 25 char device id}/blynk_led
  // -d access_token={your 40 char access token}
  // -d "args=on/off"
  
  
  if (on_off == "on" || on_off == "1") {
    particle_fn_called = TRUE;
    led_state = HIGH;
    return 1;
  } else if (on_off == "off" || on_off == "0") {
    particle_fn_called = TRUE;
    led_state = LOW;
    return 0;
  } else {
    Serial.print("Unexpected on_off value of: '"); Serial.print(on_off); Serial.println("'");
  }
  return -1;

} // blynkLED()
```

### Hardware authentication on Blynk
Static AuthToken will be used for device authentication. A unique Template ID, Template Name, and AuthToken should be included in the firmware.

1. In the Blynk.Console, navigate to **Search** > **Devices** and click **+ New Device**.
2. Choose the **From template** option.
3. Select your template from the **Template** drop-down list, fill out the **Device name** field, and click **Create**.
4. In the upper right the BLYNK_TEMPLATE_ID, BLYNK_TEMPLATE_NAME and BLYNK_AUTH_TOKEN will be displayed. Copy and update the firmware sketch with your parameters. Each device in Blynk will have a unique set of these 3 parameters. They are always available under the **Device Info** tab. 

```
// Update below with your Blynk auth token for your device (automatically populated by Blueprint)
#define BLYNK_TEMPLATE_ID "13 char template id"
#define BLYNK_TEMPLATE_NAME "ParticleDeviceBlueprint"
#define BLYNK_AUTH_TOKEN "your Blynk 32 char auth token"
```

## Webhook on the Particle Cloud

A Particle integration webhook running on the Particle cloud will accept the data from the Particle.publish() function executing on the device, and transform it into a HTTPs GET that will post data to the Blynk cloud, updating the corresponding Blynk datastream values. 
1. Create an account or log in to the [Particle Console](https://console.particle.io/)
2. Go to **Products** > **New Product** to create a new Product, and then add your device
3. After the device is added, click on the **Integrations** on the left > **Add New Integration** and select the **Webhook** option
4. Switch to **Custom template** and fill it with the following lines:

```
{
    "name": "blynk_particle",
    "event": "blynk_https_get",
    "url": "https://ny3.blynk.cloud/external/api/batch/update",
    "requestType": "GET",
    "noDefaults": true,
    "rejectUnauthorized": true,
    "query": 
    {
        "token": "{{t}}",
        "V6": "{{PARTICLE_PUBLISHED_AT}}",
        "V14": "{{v14}}", 
        "V15": "{{v15}}", 
        "V16": "{{v16}}",
        "V17": "{{v17}}"
     }
} 
```

*The keys on the left (token, V6, V14, V15, V16, V17) refer to Blynk datastreams (virtual pins), and the values on the right reference variables from the firmware that will be passed from the Particle.publish() function. The value ‘PARTICLE_PUBLISHED_AT’ for virtual pin V6 is a Particle pre-defined variable that provides a timestamp for when the webhook is executed.*

5. Update **ny3.blynk.cloud** with your server shown in the Blynk.Console lower right. Find the list of valid server addresses [here](https://docs.blynk.io/en/blynk.cloud/troubleshooting)

6. Click on **Create Webhook**

#### The Webhook should look like this:
![alt text](https://raw.githubusercontent.com/marynablynk/blynk-particle-integration/main/images/particle-webhook.png "Particle webhook")


Particle.publish() call in the firmware:
```
char data[90]; 
// Note the escaped double quotes around the &quot;&quot;t&quot;&quot; for BLYNK_AUTH_TOKEN.  
snprintf(data, sizeof(data), &quot;{\&quot;t\&quot;:\&quot;%s\&quot;,\&quot;v14\&quot;:%u,\&quot;v15\&quot;:%f,\&quot;v16\&quot;:%u,\&quot;v17\&quot;:%u}&quot;, BLYNK_AUTH_TOKEN, millis(), v15, led_state, led_state);
bool pub_result = Particle.publish(&quot;blynk_https_get&quot;, data, PRIVATE);
```

Note that the firmware will pass the unique BLYNK_AUTH_TOKEN defined for each device to the Particle webhook as the variable {{t}}. This allows each device to call the same webhook, at the expense of increasing the cellular payload for each transmission by 32 bytes.  

You can learn more about Particle webhooks by visiting this [documentation link](https://docs.particle.io/reference/cloud-apis/webhooks/). 

### Generate Particle Access Token
The Blynk webhook will need a Particle access token to make a Particle HTTP API call to the Particle cloud function.  

1. Browse to the Particle documentation section [Create a token](https://docs.particle.io/reference/cloud-apis/access-tokens/#create-a-token-browser-based-) (browser based). 
2. Enter your Particle login email and password into the form. If you have MFA (multi-factor authentication) enabled on your account, you will need your MFA code to generate the access token. 
4. Click the **Create token** button to generate a token. Keep this token confidential. 

![alt text](https://raw.githubusercontent.com/marynablynk/blynk-particle-integration/main/images/particle-token.png "Particle Access Token")

## Blynk webhook setup
Control of the Particle hardware remotely from the Blynk web dashboard or mobile app is accomplished using a Blynk webhook and the Particle HTTP API. When the state of the switch widget on the Blynk web dashboard or mobile app is changed, a Blynk webhook assigned to the same datastream is called. The webhook makes a Particle HTTP API call to a Particle cloud function with a device-unique token that sends data to the Particle hardware. 

1. In the [Blynk.Console](https://blynk.cloud/), navigate to **Settings** > **Webhooks** and create a new webhook for datastream V16 based on the information shown in the next image. 
2. After you are finished configuring the webhook, click **Test webhook** to verify it doesn’t throw an error (it won’t send the datastream value here, so don’t expect to see the LED on your Particle device change). 
3. Click **Create Webhook** to save it and close the dialog. 

Note that the **Blynk webhook request quota is 1 per minute*** so any datastream value changes sooner than 60 seconds will not execute the webhook.

![alt text](https://raw.githubusercontent.com/marynablynk/blynk-particle-integration/main/images/blynk-webhook.png "Blynk Webhook")


**Webhook URL**

The format is: 
```
https://api.particle.io/v1/devices/[your 24 character Particle device ID]/blynk_led
```  

Replace your Particle [device ID](https://console.particle.io/) and the 40-character access token you generated earlier in the corresponding form fields. Your 24-character Particle device ID is available in the [Particle console[(https://console.particle.io/).  

The "blynk_led" at the end of the WEBHOOK URL is the Particle cloud function key that is referenced in the firmware as:

```
void setup() {
  ...
  Particle.function("blynk_led", blynkLED);
  ...
}
```

**HTTP Headers (optional)**

The 'HTTP Headers' with the key "Authorization" has a value consisting of the string "Bearer " (with a space after it), and then followed by the 40-character Particle access token.  
```
Bearer 40_character_Particle_access_token
```

## Testing
1. Test the Particle cloud function running in the firmware by calling it from the Particle console. With your Particle hardware running, visit [here](https://docs.blynk.io/en/hardware-guides/particle-part-ii#firmware) for detailed instructions on how to call 'blynk_led'. Go to your [Particle console](https://console.particle.io/), select the Particle device, and then under the **Functions** section on the right side of the screen you will see the function key of ‘blynk_led’ listed. Enter **‘on’** in the **Argument** input area and click the **CALL** button. Observe the Particle device to confirm that the built-in blue LED on D7 turns on. Repeat with the **‘off’** argument to turn off the LED.   
![alt text](https://raw.githubusercontent.com/marynablynk/blynk-particle-integration/main/images/functions.png "Particle Function")
2. Verify that your Particle access token is correct by using the Particle API to test it. Detailed instructions on how to do this using [Postman](https://www.postman.com/) can be found [here](https://docs.blynk.io/en/hardware-guides/particle-part-ii#particle-api).
3. Test the Blynk webhook by installing the firmware on your Particle hardware, and then click **Test webhook** to verify it doesn’t throw an error (it won’t send the datastream value here, so don’t expect to see the LED on your Particle device change). Then from the Blynk web dashboard or mobile app, toggle the switch assigned to datastream V16 and observe the built-in LED on the hardware if it exists, or the Blynk LED widget if no built-in LED exists. Wait 60 seconds between each toggle of the switch widget.
4. Review the Particle device log to confirm the device is connected and to see what data has been published from the hardware to the Particle cloud. 
5. Review the Particle integration log to see if it was triggered successfully and the data that was pushed to it from the Particle device. 

## Troubleshooting
- Make sure the BLYNK_AUTH_TOKEN in your firmware matches what is shown in the Blynk console **Search** > **Device** > **Device Info**.
- If your Particle device has a built-in RGB then it should be breathing cyan if it is connected to the Particle cloud. 
- Perform all of the tests under **Testing** to be sure that each communication step from the Particle hardware to Blynk and back works properly.

# Example use cases
After your device is connected to Blynk, this integration can be easily modified to include more functionality. 

- Blynk no-code [Web Dashboard](https://docs.blynk.io/en/blynk.console/templates/dashboard) and [Mobile App](https://docs.blynk.io/en/blynk.apps/constructor) builder make it easy to create custom interfaces to visualize data and interact with an IoT device.
- Blynk [Automations](https://docs.blynk.io/en/concepts/automations) allows the end-user of your app to create scenarios where the device automatically performs one or more actions based on a condition. For example, you can trigger a phone notification or send an email when the Particle device detects a sensor condition of interest.
- Blynk [User Management](https://docs.blynk.io/en/concepts/users) functionality allows you to share devices with other users - from a few to thousands and offers a simple and flexible way to set up and manage multi-tenant IoT applications at any scale. 
- Blynk [Organizations](https://docs.blynk.io/en/concepts/organizations) allow you to categorize your devices and users, assigning them roles, permissions, and locations.

 
# Related Links
- [Blynk Troubleshooting guide](https://docs.blynk.io/en/troubleshooting/general-issues)
- [Blynk Documentation](https://docs.blynk.io/)
