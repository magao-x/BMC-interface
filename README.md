# BMC-Interface
Interface the modern BMC API with cacao

To compile with cacao and the BMC SDK on exao2:

    gcc -O3 -o build/runBMC2K runBMC2K.c -lopencv_core -lopencv_imgproc -laprutil-1 -Wl,-rpath /home/kvangorkom/BMC-interface/ -I/opt/Boston\ Micromachines/include -L/opt/Boston\ Micromachines/lib -Wl,-rpath-link,/opt/Boston\ Micromachines/lib -lBMC -lBMC_PCIeAPI -lncurses -lImageStreamIO -lrt -lcfitsio -lpthread -lm

with libstdc++.so.6.0.21 in /home/kvangorkom/BMC-interface (linked as libstdc++.so.6 in the same directory — the rpath must point to the directory with libstdc++).
    
To enter the control loop with the default settings:

    ./runBMC2K "<DM serial number>" <shared memory image>
    
This connects to the DM and creates a shared memory image which can be updated via cacao. Inputs are expected as single-precision floats in a 50x50 array in units of microns. `ctrl+c` will interrupt the control loop and safely reset and release the DM.
  
To run with mean-subtraction and a fixed bias (given in fractional volts between 0 and 1):

    ./runBMC2K "<DM serial number>" <shared memory image> --bias=value
    
To disable taking the square root of the inputs (which is enabled by default):

     ./runBMC2K "<DM serial number>" <shared memory image> --linear
     
 To send commands in fractional volts rather than microns:
 
    ./runBMC2K "<DM serial number>" <shared memory image> --fractional
    
For help:

    ./runBMC2K --help
    
Before running, set the path to a user configuration directory

    export bmc_calib=/some/path/

which should contain:

    bmc_2k_actuator_mapping.fits #2D image of actuator positions
    bmc_2k_actuator_mask.fits #2D binary image of active/inactive actuators
    bmc_2k_userconfig.txt #calibrated gain and volume conversion factors

and bmc_2k_userconfig.txt is a plaintext file with content:

    -1.1572 # actuator gain (microns/fractional voltage^2)
    0.5275 # volume conversion factor

    


