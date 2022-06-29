# Remote-measurement
Organization of data transmission in measurement systems. Development of a communication protocol for measuring devices operating in a single-master / multi-slave configuration and the use of the developed protocol to perform remote measurements.

Assumptions:
- Communication takes place between two computer applications running on two workstations
computers, one of them plays the superior role (Master) and the other - the child (Slave). Process control
measurement takes place only through the master application, while the slave application allows
remote access to the measuring device.
- The communication frame is to contain the following fields:
  - Starting character (choose by yourself)
  - Target device address (in the range 0 to 9, where 0 is reserved for the Master)
  - Code / symbol of the function (described below, select codes / symbols yourself)
  - Data field (with length and content depending on the type of function)
  - Number of characters in the whole frame (to check the correctness of transmission, assume that this field always contains two characters)
- Protocol functions:
  - a) Query for a single measurement result (AIN3 input of the slave device)
  - b) Query about a series of measurement results (AIN0-AIN3 inputs of the Slave device, sent as one string)
  - c) Send a single voltage setting (for the DAC0 converter of the Slave device, with acknowledgment of receipt)
  - d) Sending a text message to the selected Slave device (up to 90 characters, with confirmation of receipt)
- Protocol properties:
  - a) The protocol is to support ASCII mode
  - b) Communication is always initiated by the master
  - c) The Master device repeats the query / command after waiting 1 second if:
    - I) The response frame is of the wrong length
    - II) The reply frame has a wrong start character
  - d) The renewal of the query / command is signaled on the panel of the Master device
  - e) The received frame is interpreted on condition that the address contained in the frame and the device address match
- The exercise should be carried out using 2 computer stations connected by a serial interface or use a computer network and TCP protocol. One station functions as a Master device, the other one Slave devices:
  - a) Slave position:
    - I) Has the ability to change his address (addresses 1-9)
    - II) Depending on the selected function:
      - functions a and b: after receiving the inquiry, it receives the measurement results from the measurement card (input AIN3 or inputs AIN0-AIN3), presents them on the desktop and sends a response with the results
      - function c: sets the received value on the DAC0 output, displays it on the desktop and sends a confirmation
      - function d: displays text messages sent by the master and sends an acknowledgment
    - III) It presents the received and sent frame and signals the ongoing transmission
    - IV) Indicates an error when an invalid frame is received
  - b) Master position:
    - I) It enables the sending of inquiries, settings and messages to the slave according to the function list
    - II) It allows you to change the address of the Slave device with which communication is currently taking place (1-9)
    - III) Indicates ongoing communication on the panel
    - IV) It presents a received and sent frame
    - V) Indicates an error when an invalid frame is received
    - VI) Presents the received measurement results on the user panel (functions a and b)
