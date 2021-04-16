# Local-Mobile-Device-Farm
**Intent:** The idea is to investigate the feasibility of executing mobile tests via azure devops agent on a local Appium Instance.

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



### Configure and Register Appium server for each device.

### Run Appium server for each device




