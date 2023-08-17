# Cat Monitor

Authors: John Culley

Date: 2023-03-21

### Solution Design

For our cat monitor there are three overarching functions: turning the LED on and off, reading the temperature data and displaying it on a chart, and having a livestream from the camera. For the LED functionality, the design specification was that the user should be able to click a button on a remote portal which will turn on and off the LED which is connected to an ESP32. The first step to solving this was to determine how the data was going to be transmitted between the remote portal and the ESP32. Because a front-end HTML page is unable to transmit data over the network to the ESP32 on its own, a middle-man node server is necessary. The node server will be run by the raspberry pi. The button on the HTML page will toggle between on and off, so when the user clicks on the button an HTTP Request will be initiated and send a message to the node server. The node server now has access to a variable that indicates that the LED should be on or off. The next step was to determine how to get the data from the node server to the ESP32. This data communication was done using User Datagram Protocol (UDP). The node server was the client because it is the one that will be sending the message, and the ESP32 with a connected LED was the server. After this data communication was completed using UDP, the ESP32 now has access to a variable called “LED_status” which will either be 0 or 1 and will turn on or off the LED depending on the value. 
  
  For the temperature sensor data, the design specification stated that the temperature sensor data should be sent to a front end where the data is live and accurate, with the ability to scroll to past data. This was a slightly different implementation than the LED because now the data will be moving from the ESP32 to the front end to be displayed. Similarly, the process requires a node server to be the middle-man between the front and back end. The temperate data was gathered from the ESP32 in Celcius and sent via UDP to the node server. The temperature is passed to the node server as a double. In the node server, there is a function “writeData” which is responsible for creating the JSON file that will eventually be read on the HTML page. The “writeData” function has the temperature as a parameter and then creates a variable called “date” which uses JavaScript’s date functions to get the current time from the server. Next, a file called “data.json” is read to obtain the current JSON object. The new data that is going to be appended to the JSON object is then prepared in an array with [time,temperature] as the key and value. This is then pushed into the JSON Object. Now the JSON Object is written back into the “data.json” file. Now that the data is stored in a JSON file, it can be accessed in the HTML file. Inside the HTML file, an AJAX call is used to access the data from the file. The AJAX call then calls a function called “processdata” which is used to extract the data. Because the data is already stored in JSON format, it makes it very easy to implement it into Apex Charts. A Brush Chart was used from Apex Charts which allows for the user to scroll to data in the past and adjust the size of the window, which was a key specification for the design. The next step for the HTML page was the ability to update the data series as the file gets updated to provide real time data to the remote portal. For this, a function called “updateSeries” is used. The “updateSeries” function starts by reinitiating the AJAX call to access the new data from the “data.json” file. Then the chart options are then updated with the new data to ensure that the chart is updated in real time. 
  
  For the livestream from the camera, the design specification stated that the live feed should be displayed on a remote portal. The Raspberry Pi Zero camera is connected via wire to the Raspberry Pi. The Raspberry Pi settings then need to be configured to recognize the camera and adjust the interface to include the camera for the remote host. Next, we installed a software called “motion” onto the Raspberry Pi which allows the camera to operate and the livestream to be uploaded onto the remote portal. When the command “motion” is then called inside of the Raspberry Pi terminal, it begins the livestream. 
  
Local ports:
- 8080: Motion control page
- 8081: Camera Streaming
- 8082: Web Protal(control LED and read temperature datas)

Port forwarding:
- Local 8080 -> External 3080
- Local 8081 -> External 3081
- Local 8082 -> External 3082

### Sketches/Diagrams
<img width="891" alt="Screen Shot 2023-03-21 at 8 29 55 PM" src="https://user-images.githubusercontent.com/91488781/226771612-93a34d1c-79fb-49e8-ac04-6505fe0f2970.png">
Flow Chart showing the passages that the data flows though our systems.
</p>
<img width="949" alt="Screen Shot 2023-03-21 at 8 31 18 PM" src="https://user-images.githubusercontent.com/91488781/226771759-e3f99f21-2ce8-4b6e-9e1b-12034ad07a38.png">
Diagrams of the two ESP 32s and the Raspberry PI to show the port numbers and resistior values.
</p>
<img width="630" alt="Screen Shot 2023-03-21 at 8 10 58 PM" src="https://user-images.githubusercontent.com/91488781/226771231-9a0c8625-ff95-46db-b1fc-03027470dcd9.png">
Our real circuts for the LED, thermistor, and the camera with the Raspberry PI.
</p>
<img width="951" alt="Screen Shot 2023-03-21 at 3 05 25 PM" src="https://user-images.githubusercontent.com/91488781/226771256-9b71654d-4772-400e-a1ca-ac5a932536e8.png">
Image of our chart with the LED toggle and the live data of the thermistor.
</p>
<img width="363" alt="Screen Shot 2023-03-21 at 8 27 29 PM" src="https://user-images.githubusercontent.com/91488781/226771313-0e268c22-4222-4aa6-8881-766a6ba4b230.png">
Method by which the live data chart works.
</p>
<img width="875" alt="Screen Shot 2023-03-21 at 8 28 15 PM" src="https://user-images.githubusercontent.com/91488781/226771400-68f07578-d9fc-44c2-8205-affa9a33498f.png">
Image of the HTML site that the live camera footage is being displayed on.
</p>
<img width="973" alt="Screen Shot 2023-03-21 at 8 28 51 PM" src="https://user-images.githubusercontent.com/91488781/226771466-56235cfa-be75-4407-a868-8abfacb64ea5.png">
Diagram like above that also includes the router.

### Supporting Artifacts
- [Link to video demo](https://drive.google.com/file/d/1cqoyowgOMQD84HBG7u9QK6C-4om4HYGg/view?usp=sharing). Not to exceed 120s


### Modules, Tools, Source Used Including Attribution
ESP32, Raspberry Pi, Visual Studio Code, Linksys Router, Afraid.org DDNS, Raspberry Pi Zero Camera, LED Light, Node.js, Apex Charts, Ajax, JSON, UDP, Motion package, Temperature Sensor


### References
No References. 

