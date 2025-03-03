This is an I2C internal flash programming for MachXO2/MachXO3L/MachXO3LF using Raspberry Pi 5.

The MachXO3 and MachXO2 devices feature internal flash memory that supports single boot, eliminating the need for an external flash and reducing component count. However, an external flash can still be used to store a golden image.For more information
please check FPGA-TN-02155 ([MachXO2 Programming and Configuration User Guide](https://www.latticesemi.com/view_document?document_id=39085)) and FPGA-TN-02055 ([MachXO3 Programming and Configuration User Guide](https://www.latticesemi.com/view_document?document_id=50123)).
These documents also outline the instructions used in this programming example.

Source files:

main.c -contains the main programming procedure

rbpi-i2c.c- contains the functions for i2c transaction and programming commands for MachXO3

data.c- contains the bitstream data


Steps to run the project: 

1. Compile by running the makefile
2. Run the executable generated ./rhodz_i2c

Some keypoints and other pointers for this example. 

1. This example, uses I2C make sure that the I2C_PORT is enabled on your project and that wishbone is instantiated in your design with a valid clock. FPGA-TN-02155 and FPGA-02055 explains this:
![image](https://github.com/user-attachments/assets/59011226-2374-469c-b09c-764dd91350a6)
Enable the i2c port at the Spreadsheet View=>Global Preferences
![image](https://github.com/user-attachments/assets/bb1da39e-55c9-458d-8f70-6490e9a5cbb4)

2. When doing read operation, do not use a stop condition after sending a read comand, instead send an I2C restart and do an I2C Read:
![image](https://github.com/user-attachments/assets/fdb4821e-2f62-49c3-97db-86db4a3f750a)


3.  Jedec file contains binary bitstream data. It is helpful to convert it to hex to be able to use hexadecimal array for programming. Lattice has a Deployment Tool which can be used to convert jedec into hex file:
   ![image](https://github.com/user-attachments/assets/d6d42e34-0e62-4db6-812e-4f32163a7bed)

Sample hex output is below:

![image](https://github.com/user-attachments/assets/b8b0a46a-594c-480d-b277-9d0650d59acb)

You could use a simple script to turn this into a hexadecimal array:
![image](https://github.com/user-attachments/assets/dd8faefe-c209-4f83-ac28-e14e0e8a349f)

Flow in main.c:

![image](https://github.com/user-attachments/assets/6b1f75c6-83cc-41bc-a1d4-bae2691cf4dd)

Sample Terminal Run:
![image](https://github.com/user-attachments/assets/ff8fc970-c4b6-4cd5-9377-b3cdc39a92fe)

