# Integration with Blynk IoT

Particle has teamed up with Blynk to create a webhook integration with the [Blynk IoT platform](https://blynk.io/?utm_source=particle&utm_medium=referral&utm_campaign=integr&utm_content=docs).

Blynk is a low-code IoT software platform for connecting devices to the cloud, building mobile apps to remotely control and monitor them, and managing users and IoT devices at any scale. To visualize data and interact with devices, Blynk offers native mobile apps for iOS and Android and a web dashboard that can be built with a drag-and-drop constructor, eliminating the need to write code for the front end. Blynk also includes built-in functionality for over-the-air firmware updates, device provisioning, advanced device and user management tools, alerts and notifications, automations, and data analytics. 

![alt text](https://raw.githubusercontent.com/marynablynk/blynk-particle-integration/main/images/blynk-particle-logos.png "Particle Blynk Logo")

This integration enables bi-directional communication between any Particle hardware and Blynk IoT. It allows visualization of the data and control of the Particle device remotely via the Blynk web and mobile dashboard.

## Key concepts in Blynk IoT

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


## Sign up for a Blynk account

If you don't already have one, you'll need to sign up for a Blynk account to use this integration.

Go to the [Blynk Console](https://blynk.cloud/?utm_source=particle&utm_medium=referral&utm_campaign=integr&utm_content=docs) to sign in or to create a new account. A Free account is available for the platform exploration. To access more features for advanced personal or commercial use, check out the Blynk [pricing page](https://blynk.io/pricing?utm_source=particle&utm_medium=referral&utm_campaign=integr&utm_content=docs) to learn more about subscription options.


## Activate a Blueprint

To simplify the setup process, we've created a Blueprint for this integration. It includes a pre-configured template tailored for this project, a firmware, and a detailed guide on setting up webhooks in both the Particle Console and Blynk. 

1. Go to the **Blynk Console** > **Templates** > **Blueprints** > **All Blueprints** > **[Connect a Particle Device](https://blynk.cloud/dashboard/blueprints/Library/TMPL4ej7--Xu_)**
2. Click **Use Blueprint** in the top right and follow the guide to setup webhooks and connect your device.


## Example use cases
After your device is connected to Blynk, this basic integration can be easily modified to include more functionality. 

- Blynk no-code [Web Dashboard](https://docs.blynk.io/en/blynk.console/templates/dashboard?utm_source=particle&utm_medium=referral&utm_campaign=integr&utm_content=docs) and [Mobile App](https://docs.blynk.io/en/blynk.apps/constructor?utm_source=particle&utm_medium=referral&utm_campaign=integr&utm_content=docs) builder make it easy to create custom interfaces to visualize data and interact with an IoT device.
- Blynk [Automations](https://docs.blynk.io/en/concepts/automations?utm_source=particle&utm_medium=referral&utm_campaign=integr&utm_content=docs) allows the end-user of your app to create scenarios where the device automatically performs one or more actions based on a condition. For example, you can trigger a phone notification or send an email when the Particle device detects a sensor condition of interest.
- Blynk [User Management](https://docs.blynk.io/en/concepts/users?utm_source=particle&utm_medium=referral&utm_campaign=integr&utm_content=docs) functionality allows you to share devices with other users - from a few to thousands and offers a simple and flexible way to set up and manage multi-tenant IoT applications at any scale. 
- Blynk [Organizations](https://docs.blynk.io/en/concepts/organizations?utm_source=particle&utm_medium=referral&utm_campaign=integr&utm_content=docs) allow you to categorize your devices and users, assigning them roles, permissions, and locations.
