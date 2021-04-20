# Local-Mobile-Device-Farm
**Intent:** To create a mobile device lab to execute mobile tests via azure devops Self-Hosted agent.

**Tools Used:**
1.	Selenium Grid 3.141
2.	Appium Server(s)
3.	Azure Devops agent 
4.	Mobile Device

## Setup

### Install and configure Selenium Grid Infrastructure:
1.	Download Selenium Grid 3.141.59 from **https://www.selenium.dev/downloads/**
2.	Setup Local Hub 
	-	Open cmd within the selenium-grid.jar dir and run the below command.
		-	java -jar selenium-server-standalone.jar -role hub 
		    ![GridCmd](https://user-images.githubusercontent.com/46636658/115090074-e020ac80-9ee1-11eb-90f3-17362b29a2ac.png)
	 
    - Once the grid is initialized, verify the grid by navigating to 
       -  **http://localhost:4444/grid/console**
           ![image](https://user-images.githubusercontent.com/46636658/115090169-22e28480-9ee2-11eb-9718-4844d2da744b.png)



### Create a config file per device.
An `Appium server instance` can only interact with a single device, so we would need to spin up one server per device.
Let's start by creating a `device config`, this would be a **.json** file `per device` needed for selenium grid to match our execution request.

**Capabilities** : This is required by appium to interact with our devices

**Configuration**: This is required by selenium grid to know about the appium server

```json 
=======Android========
{
	"capabilities": [
    	{
			"deviceName": "moto_g_7__power",
			"platformName": "Android",
			"browserName":"Chrome",
			"maxInstances": 1,
			"NoReset" : "false",
                        "Udid" : "ZY326QJ8MR"
			
    	}
	],
	"configuration": {
			"cleanUpCycle": 2000,
			"timeout": 30000,
			"proxy": "org.openqa.grid.selenium.proxy.DefaultRemoteProxy",
			"host": "127.0.0.1",
			"port": 4726,
			"maxSession": 1,
			"register": true,
			"url": "http://127.0.0.1:4726/wd/hub",
			"registerCycle": 5000,
			"hubPort": 4444,
			"hubHost": "127.0.0.1"
	}
}
```
```json
=======IOS=======
{
	"capabilities": [{
		"deviceName": "iphone",
		"platformName": "iOS",
		"automationName": "XCUITest",
		"platformVersion": "12.14",
		"udid": "isdhsd32335d",
		"browserName": "Safari",
		"maxInstances": 1
	}],
	"configuration": {
		"cleanUpCycle": 2000,
		"timeout": 30000,
		"proxy": "org.openqa.grid.selenium.proxy.DefaultRemoteProxy",
		"host": "127.0.0.1",
		"port": 4725,
		"maxSession": 1,
		"register": true,
		"url": "http://127.0.0.1:4725/wd/hub",
		"registerCycle": 5000,
		"hubPort": 4444,
		"hubHost": "127.0.0.1"
	}
}
```

### Run Appium server for each device

1.	Execute the below command from your command prompt.
	(remember to update the parameters with the required details)

``` 
appium --session-override --nodeconfig <LocationOfYourDeviceConfig.json> -p <portFromConfig> --bootstrap-port <anyPort> --udid <deviceUDID>

```

2.	Once the above command is successfull, an appium server instance should start and get registered to the Grid.
	   - 	Navigate to http://localhost:4444/grid/console to view running appium instance/s.	   	
			![image](https://user-images.githubusercontent.com/46636658/115158646-2d755900-a05d-11eb-8812-f95cbb67b669.png)
			

3. 	(Optional Step) Initialize your appium driver using the selenium hub url i.e. http://yourGridServerIP:4444/wd/hub to verify test execution
	 	


### Execute Your Tests via Azure Devops using Self-Hosted agents

1. 	Register the machine with the Selenium Grid server as a [Self-Hosted agents](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/v2-windows?view=azure-devops) 

2.	Download, configure and run the agent 

3.	In your code where you initialize the `Appium driver` using the selenium grid url

		```
		e.g.  driver = new AndroidDriver<AndroidElement>(new Uri("http://127.0.0.1:4444/wd/hub"), appiumOptions, TimeSpan.FromMinutes(config.DriverInitTimeOut));
		
		```
	
5.	Assign your agent to your release/pipeline agent job

6.	Deploy/Run your tests

7.	**On Success** 

   	You would notice the Grid Console Registering your requests and tests being executed on the registered devices

```
11:13:49.557 INFO [RequestHandler.process] - Got a request to create a new session: Capabilities {app: C:\agent\_work\r1\a\_Test F..., appActivity: com.pegasustranstech.transf..., appPackage: com.pegasustranstech.transf..., autoGrantPermissions: true, deviceName: moto_g_7__power, deviceOrientation: landscape, fullReset: true, newCommandTimeout: 120, noReset: false, platformName: android, tabletOnly: true, udid: AM@ZING}
11:13:49.565 INFO [TestSlot.getNewSession] - Trying to create a new session on test slot {server:CONFIG_UUID=7e3bf1c8-6caf-4241-ae50-72e5a16d9ae4, Udid=emulator-5554, seleniumProtocol=WebDriver, browserName=Chrome, maxInstances=1, platformName=Android, NoReset=false, deviceName=emulator-5554, platform=ANDROID}

```


