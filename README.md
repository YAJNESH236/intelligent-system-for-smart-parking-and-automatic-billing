**Intelligent system for Real-time parking and automatic billing**

**AIM:**
The aim of the project, IOT based fully automated parking system for vehicle parking stations implemented by microcontroller and the RFID module. The main aim of this project is reduces human interaction in parking area.

**OBJECTIVE:**
Optimizing Parking Space: Efficiently manage available parking spots by providing real-time data on available spaces and guiding drivers to open spots. This minimizes time spent searching for parking and reduces congestion.

Enhancing User Experience: Offer a seamless experience for drivers by providing user-friendly interfaces or apps that allow them to easily find parking, reserve spots, and navigate to their designated space.

Real-Time Monitoring: Employ sensors or cameras to monitor parking spaces in real time, allowing for accurate information on available spots and enabling immediate updates for drivers.

Automatic Billing: Implement a system that calculates parking fees based on duration and automatically bills users through various payment methods (e.g., credit card, mobile payment) upon exiting the parking area.

Integration and Accessibility: Ensure compatibility with different devices and platforms, making the system accessible to a wide range of users. Integration with navigation apps or in-car systems can further streamline the parking process.

Security and Safety: Employ measures to ensure the safety of vehicles and users within the parking area, including surveillance, emergency assistance features, and secure payment transactions.

Data Analysis and Optimization: Utilize data collected from the system to analyze parking patterns, peak times, and user behavior, enabling better resource allocation and future planning.

**COMPONENTS REQUIRED:**
Arduino uno
20*4 LCD display
12C LCD module
Mini servomotor SG-90
IR sensor
Jumper cable
Bread board
5V 2amp power adapter

**WORKING:**
•	To implement an intelligent system for real-time parking with automatic billing, you need a combination of hardware and software components.
•	Use sensors, cameras, or IoT devices to monitor each parking space in real-time.Employ image recognition or machine learning algorithms to identify whether a space is occupied or vacant.
•	Establish a reliable communication network to connect all the parking space sensors to a central server.Ensure low-latency communication for real-time updates.
•	Set up a centralized server that receives and processes data from parking space sensors.Implement algorithms to aggregate and analyse the data to determine parking space availability.
•	Implement a reservation system that allows users to reserve a parking space for a specific duration.
•	Integrate a secure payment gateway for automatic billing.
•	Track the duration of the parking session, calculate charges based on a predetermined rate, and process payments seamlessly.

**FLOW CHART:**

![Screenshot 2023-11-17 194426](https://github.com/Shreedharagowda8266/Intelligent-system-for-smart-parking-and-automatic-billing/assets/109616711/5008e013-6a47-4264-a176-0e2be35747d3)

**MIND MAP:**
![Blue Simple Professional Business Brainstorm (1)](https://github.com/Shreedharagowda8266/Intelligent-system-for-smart-parking-and-automatic-billing/assets/109616711/49aa8d9b-3ad4-47bf-9852-526079dfc721)

**Circuit diagram:**
![WhatsApp Image 2023-11-22 at 4 40 03 PM](https://github.com/Shreedharagowda8266/Intelligent-system-for-smart-parking-and-automatic-billing/assets/119619029/94328aa2-ea1d-4ab1-a2e0-06584f22c34f)

**BLOCK DIAGRAM:**

![WhatsApp Image 2023-11-22 at 2 27 20 PM](https://github.com/Shreedharagowda8266/Intelligent-system-for-smart-parking-and-automatic-billing/assets/119619029/4b561668-1bc5-49d0-a314-6134d8bc414f)

**APPLICATION:**
Real-Time Parking Management:
Sensor Integration: Install sensors in parking spots to detect vehicle presence. These sensors can be ultrasonic, infrared, or camera-based.

Data Collection: Sensors relay real-time data to a central system, indicating which parking spots are vacant or occupied.

Machine Learning for Prediction: Algorithms can analyze patterns in parking data to predict peak hours, helping optimize operations.

Mobile App or Interface: Users can access an app or interface showing available parking spaces in real-time, guiding them to empty spots.

Automated Guidance: Once a user selects a spot, the system can provide navigation to that spot through the app.

Automatic Billing:
Entry/Exit Recognition: Use cameras or sensors at entry/exit points to detect vehicles and record their entry and exit times.

License Plate Recognition (LPR): Employ computer vision with LPR to identify vehicles and link them to registered accounts.

Billing Calculation: Calculate parking fees based on duration and rates. Machine learning models can help adjust pricing based on demand.

Automated Payments: When a vehicle exits, the system automatically charges the user's registered payment method.

Alerts and Notifications: Send digital receipts or notifications to users confirming payment and summarizing parking details.

CODE:
#include <Servo.h>

#include <LiquidCrystal.h>

#include <SPI.h>
#include <MFRC522.h>

#define RST_PIN         9           // Configurable, see typical pin layout above
#define SS_PIN          10    
#include<SoftwareSerial.h>
SoftwareSerial SUART(0, 1); //SRX = DPin-2; STX = DPin-3

      // Configurable, see typical pin layout above

MFRC522 mfrc522(SS_PIN, RST_PIN); 
// Pin configuration for the LCD
const int rs = 8;
const int en = 7;
const int d4 = 5;
const int d5 = 4;
const int d6 = 3;
const int d7 = 2;

// Pin configuration for IR sensors
const int irSensorPin1 = A0;
const int irSensorPin2 = A1;
const int irSensorPin3 = A2;
const int irSensorPin4 = A3;
const int irSensorPin5 = A4;
Servo servoMotor;
const int servoPin=6;
// Create an LCD object
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);

// Parking variables
int totalSpaces = 4;
int availableSpaces = 4;

void setup() {
  // Set up the LCD columns and rows
  lcd.begin(20, 4);
  Serial.begin(9600);
  SUART.begin(9600);

  SPI.begin();                                                  // Init SPI bus
  mfrc522.PCD_Init();                                              // Init MFRC522 card
  Serial.println(F("Read personal data on a MIFARE PICC:"));
  // Simulate initialization of parking system
  lcd.print("Smart Parking");
  delay(2000);
  lcd.clear();
  servoMotor.attach(servoPin);
}

void loop(){
 // Simulate updating parking status
   MFRC522::MIFARE_Key key;
  for (byte i = 0; i < 6; i++) key.keyByte[i] = 0xFF;

  //some variables we need
  byte block;
  byte len;
  MFRC522::StatusCode status;

  //-------------------------------------------

  // Reset the loop if no new card present on the sensor/reader. This saves the entire process when idle.
  if ( ! mfrc522.PICC_IsNewCardPresent()) {
    return;
  }

  // Select one of the cards
  if ( ! mfrc522.PICC_ReadCardSerial()) {
    return;
  }

  Serial.println(F("**Card Detected:**"));

  //-------------------------------------------

  mfrc522.PICC_DumpDetailsToSerial(&(mfrc522.uid)); //dump some details about the card

  //mfrc522.PICC_DumpToSerial(&(mfrc522.uid));      //uncomment this to see all blocks in hex

 
 char myMsg[] = "141-142 Love Road";
 //-------------------------------------------
Serial.println(myMsg); 
SUART.println(myMsg); 
  Serial.print(F("Name: "));

  byte buffer1[18];

  block = 4;
  len = 18;

  //------------------------------------------- GET FIRST NAME
  status = mfrc522.PCD_Authenticate(MFRC522::PICC_CMD_MF_AUTH_KEY_A, 4, &key, &(mfrc522.uid)); //line 834 of MFRC522.cpp file
  if (status != MFRC522::STATUS_OK) {
    Serial.print(F("Authentication failed: "));
    Serial.println(mfrc522.GetStatusCodeName(status));
    return;
  }

  status = mfrc522.MIFARE_Read(block, buffer1, &len);
  if (status != MFRC522::STATUS_OK) {
    Serial.print(F("Reading failed: "));
    Serial.println(mfrc522.GetStatusCodeName(status));
    return;
  }

  //PRINT FIRST NAME
  for (uint8_t i = 0; i < 16; i++)
  {
    if (buffer1[i] != 32)
    {
      Serial.write(buffer1[i]);
    }
  }
  Serial.print(" ");

  //---------------------------------------- GET LAST NAME

  byte buffer2[18];
  block = 1;

  status = mfrc522.PCD_Authenticate(MFRC522::PICC_CMD_MF_AUTH_KEY_A, 1, &key, &(mfrc522.uid)); //line 834
  if (status != MFRC522::STATUS_OK) {
    Serial.print(F("Authentication failed: "));
    Serial.println(mfrc522.GetStatusCodeName(status));
    return;
  }

  status = mfrc522.MIFARE_Read(block, buffer2, &len);
  if (status != MFRC522::STATUS_OK) {
    Serial.print(F("Reading failed: "));
    Serial.println(mfrc522.GetStatusCodeName(status));
    return;
  }

  //PRINT LAST NAME
  for (uint8_t i = 0; i < 16; i++) {
    Serial.write(buffer2[i] );
  }


  //----------------------------------------

  Serial.println(F("\n*End Reading*\n"));

  delay(1000); //change value if you want to read cards faster

  mfrc522.PICC_HaltA();
  mfrc522.PCD_StopCrypto1();

  updateParkingStatus();

  // Display parking status on the LCD
  displayParkingStatus();

  // Your main code for real-time updates can go here

  delay(1000);  // Update every 1seconds

}



void updateParkingStatus() {
  // Read IR sensor inputs
  int irSensorValue1 = digitalRead(irSensorPin1);
  int irSensorValue2 = digitalRead(irSensorPin2);
  int irSensorValue3 = digitalRead(irSensorPin3);
  int irSensorValue4 = digitalRead(irSensorPin4);
int irSensorValue5= digitalRead(irSensorPin5);
if(irSensorValue5==HIGH)
{
  servoMotor.write(90);
}
else
{ 
  servoMotor.write(0);
}


  // Adjust available spaces based on IR sensor inputs
  availableSpaces = totalSpaces;
  if (irSensorValue1 == LOW) {
    availableSpaces--;
  }
  if (irSensorValue2 == LOW) {
    availableSpaces--;
  }
  if (irSensorValue3 == LOW) {
    availableSpaces--;
  }
  if (irSensorValue4 == LOW) {
    availableSpaces--;
  }

  // Ensure available spaces do not go below 0
  availableSpaces = constrain(availableSpaces, 0, totalSpaces);
}

void displayParkingStatus() {
  lcd.clear();

  for (int i = 0; i < totalSpaces; ++i) {
    lcd.setCursor(0, i );
    lcd.print("Space ");
    lcd.print(i + 1);
    lcd.print(": ");

    if (i < availableSpaces) {
      lcd.print("Available");
    } else {
      lcd.print("Occupied");
}
}
}

RESULT:

As a conclusion, this project will help in reducing the amount of time a driver has to spend around the parking just to find an available spot, reducing the amount of traffic around the parking and also reducing the bad parking around the parking space.Adopting parking management system significantly reduces the amount of time consumed in seeking the parking space, renders valuable data upon the availability of the parking area, accurate mapping of the parking space, offers guidance and suggestion for proper vehicle

![WhatsApp Image 2023-11-25 at 17 46 27_0e5d9f3a](https://github.com/YAJNESH236/intelligent-system-for-smart-parking-and-automatic-billing/assets/149752107/3ac4949e-1d16-4168-ba62-a9ab6900886c)








