# DigitalElectronics2_Lab8
Laboratory 8: I2C

## Link GitHub

Link to my GitHub: https://github.com/raulgoi/DigitalElectronics2_Lab8/edit/main/README.md

Raúl Gómez Ibáñez


## Arduino pins

![arduino completado](https://user-images.githubusercontent.com/91128806/141768993-3b2b78c2-79fa-4901-996c-fa7578aae635.png)


## I2C



![I2C Communication](https://user-images.githubusercontent.com/91128806/141847315-15fb3db0-58ca-4d34-af87-3b70d09cb2ca.jpeg)


### Code


     ISR(TIMER1_OVF_vect)
     {
   
        static state_t state = STATE_IDLE;  // Current state of the FSM
        static uint8_t addr = 7;            // I2C slave address
        uint8_t result = 1;                 // ACK result from the bus
        char uart_string[2] = "00"; // String for converting numbers by itoa()

          // FSM
          switch (state)
        {
        // Increment I2C slave address
          case STATE_IDLE:
              addr++; //Increase the value of address
                  if(addr > 7 && addr < 120)  //set the values to put state on STATE_SEND
                      state = STATE_SEND;
                  if(addr > 120) //If address esces the hogh value, put at 0
                  {
                      uart_puts_P("\r\nScan I2C-bus for devices:\r\n");
                      addr = 0;
                   }

              break;
    
              // Transmit I2C slave address and get result
              case STATE_SEND:
                // I2C address frame:
                // +------------------------+------------+
                // |      from Master       | from Slave |
                // +------------------------+------------+
                // | 7  6  5  4  3  2  1  0 |     ACK    |
                // |a6 a5 a4 a3 a2 a1 a0 R/W|   result   |
                // +------------------------+------------+
                result = twi_start((addr<<1) + TWI_WRITE);
                twi_stop();
                /* Test result from I2C bus. If it is 0 then move to ACK state, 
                * otherwise move to IDLE */
                
                if (result == 0)
                    state = STATE_ACK;
                else
                    state = STATE_IDLE;

              break;

                // A module connected to the bus was found
              case STATE_ACK:
                 // Send info about active I2C slave to UART and move to IDLE
                    itoa(addr, uart_string, 10); //Set the funtion to send the information and moveit
                    uart_puts_P("Found device at address: ");
                    uart_puts(uart_strings);
                    uart_puts_P("\r\n"); //We send the info about I2C to UART
                    state = STATE_IDLE; //Move to IDLE

              break;

                // If something unexpected happens then move to IDLE
              default:
                  state = STATE_IDLE;
              break;
        }
    }



### Hand-Draw



## Scheme: meteo station


![FLW LAB8](https://user-images.githubusercontent.com/91128806/141811660-9adc2ca4-e883-4c0e-a3a3-26a22dfe8bc9.png)



