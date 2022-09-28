1. How does the clocking block handles synchronous reset?


Synchronous reset  is sampled  only with respect to clock event. When reset is enabled, it will be effective only after next active clock edge.    

     clockingblock  TB_CB @(posedge clk)   
     default input #1step output #0;   
     input rst;
     output a,b;
     input y;

**Note:** `The clocking block is only designed to handle synchronous reset` - it should  happen only be based on clock event. Reset in clocking block  should be handled elsewhere. If the clocking block outputs are variables, you can procedurally assign them outside of a clocking block on a reset.    

2. How does the modport handle asynchronous reset?

Asynchronous Reset  

In asynchronous reset sampling is  done by independent of clk. When reset is enabled ,it will be effective immediately within the same the clock edges.  

![image](https://user-images.githubusercontent.com/110484152/192695050-b55d72c7-d71c-423e-8cca-aab9433be1f7.png)  

                                             Fig 1: Design example   

Here, in this example there are three signals a,b and c. 'a' is continuous signal and asynchronous. 'b' and 'c' are synchronous signals. Using modport we can change the asynchronous signal 'a' to synchronous signal.  
`modport TB_MP(TB_CB , output a);` 

**Note:** `In Modport, the design can handled with asynchronous reset.` The asynchronous reset will be happen at any time.  

3.What is  synchronous and asynchronous  reset?      

Synchronous reset means reset is sampled with respect to clock. In other words, when reset is enabled, it will not be effective till the next active clock edge.  

module synchronous_reset_test (input logic clk, reset, in1, output logic out1)  
always @(posedge clk)  
if(reset) out1 <= 1'b0;  
else out1 <= in1;  
endmodule  

The out1 will be changed only with the posedge of clk. To get the effect of reset, reset should be wide enough to be captured by the next posedge of clk.  

![image](https://user-images.githubusercontent.com/110484152/192996653-a09a4755-a5c5-4b47-aeda-97d2156185a1.png)  

                                                   Fig 2: Synchronous Reset  

Asynchronous Reset  

In asynchronous reset, reset is sampled independent of clk. That means, when reset is enabled it will be effective immediately and will not check or wait for the clock edges.  

module asynchronous_reset (input logic clk, reset, in1, output logic out1);  
always @(posedge clk or posedge reset)  
if(reset) out1 <= 1'b0;  
else out1 <= in1;  
endmodule  

![image](https://user-images.githubusercontent.com/110484152/192997263-68f7de6a-da15-4deb-a429-86597ebc3e43.png)  

                                                      Fig 3: Asynchronous Reset     

4.What is the usage of blocking and non blocking assignments in sampling and driving signals?  

The blocking assignment (=) used for sampling signals in active region.  
The non blocking assignment (<=)used for driving signals in active-NBA region.  
So we can avoid the race condition in verilog.  

5.



