# VHDL Car Parking System

Proof of Concept College Parking System created in VHDL which includes many features, such as: 
  - Ultra-Sonic Range sensors for each of the 50 parking spaces.
  - ID system for college students and staff. 
  - Ticket system for guests (including payment system). 
  - Fire Sensor for emergencies. 
  - Vending machine.
  - Identification System to check for a valid AAST ID.

# Program in action

- Once the Identification system senses a valid AAST ID, the car is let through. If not, the guest car is assigned a ticket and their entry time is recorded.
- Once inside, the car can enter one of 50 parking spaces, each with an Ultra-Sonic range sensor.
- The Ultra-Sonic range sensors measure the distance between the car and the edge of the parking space. Once the car has entered the parking space and the sensors have detected their entry, the car is considered parked.
- Once the car is parked, there are two main results. Firstly, the LED Light of the parking spot is turned off to indicate it has been filled. Secondly, the number of free parking spaces is decreased by one.
- The number of free parking spaces is displayed at the entrance to the parking garage in a 7-Segment Display.
- The garage is equipped with a Fire Sensor which detects the presence of high temperatures capable of causing damage. If a high temperature is detected, a fire alarm occurs.
- The garage also includes a vending machine which serves various options, including Coffee, Water, Hot Chocolate, and more. Simply press the button of the desired beverage, pay the written price, and receive the drink.

# Screenshots

![WhatsApp Image 2023-01-09 at 3 57 21 PM](https://user-images.githubusercontent.com/87129311/215566999-0fc9f616-95e6-4a35-9aef-fca082426b5e.jpeg)

![WhatsApp Image 2023-01-10 at 1 18 03 AM](https://user-images.githubusercontent.com/87129311/215567161-0e45291a-b0dd-41c8-96a5-750a4c7c53ce.jpeg)

# Credits

Coded by:
- Mahmoud Hany.
- Ismail Fakhr.
- Ahmed Kamal.
- Alaa Imam.
- Anton Ashraf.
- Ahmed Mohamed El-Dalil.
