#Molecular Dynamics Project
Due November 17, 2023  


##Download and test Tinker (MD package)

1) Go to <https://dasher.wustl.edu/tinker/>, scroll at the bottom of the page and find the "Tinker Resources" section. Download the Tinker Executables (i.e. NOT the source code that you would have to compile yourself) for your operating system.

2) For this project, you will need to use analyze, minimize and dynamic. To make sure it works properly on your system, download the files `Test.xyz, amoebabio18.prm` and `Test.key` from <https://github.com/WelbornGroup/Chem6624_MDproject>

3) In your terminal, create a directory with all the files you downloaded in Step 2. Run the command `~/Tinker/analyze Test.xyz -k Test.key` and enter `E` when prompted (note that you may have to update the path to the Tinker executables). You should get the following result:

```sh
     ######################################################################
   ##########################################################################
  ###                                                                      ###
 ###            Tinker  ---  Software Tools for Molecular Design            ###
 ##                                                                          ##
 ##                      Version 8.10.2   February 2022                      ##
 ##                                                                          ##
 ##               Copyright (c)  Jay William Ponder  1990-2022               ##
 ###                           All Rights Reserved                          ###
  ###                                                                      ###
   ##########################################################################
     ######################################################################


 READXYZ  --  Atom Labels not Sequential, Attempting to Renumber

 The Tinker Energy Analysis Utility Can :

 General System and Force Field Information [G]
 Force Field Parameters for Interactions [P]
 Total Potential Energy and its Components [E]
 Energy Breakdown over Each of the Atoms [A]
 List of the Large Individual Interactions [L]
 Details for All Individual Interactions [D]
 Electrostatic Moments and Principle Axes [M]
 Internal Virial & Instantaneous Pressure [V]
 Connectivity Lists for Each of the Atoms [C]

 Enter the Desired Analysis Types [G,P,E,A,L,D,M,V,C] :  E

 READXYZ  --  Atom Labels not Sequential, Attempting to Renumber

 Total Potential Energy :         1219999867.9954 Kcal/mole

 Energy Component Breakdown :           Kcal/mole        Interactions

 Bond Stretching                  1220009901.5748             4331
 Angle Bending                          1100.7150             2179
 Stretch-Bend                           -114.3084               18
 Urey-Bradley                           -104.0566             2160
 Out-of-Plane Bend                         0.5001                6
 Torsional Angle                           2.1514               25
 Pi-Orbital Torsion                        0.0289                1
 Van der Waals                         19017.3412          1273061
 Atomic Multipoles                    -20239.3380          1279368
 Polarization                          -9696.6129          1279368
```

This is the output of an energy calculation that was performed using the coordinates provided in `Test.xyz` and the force field parameters provided in `amoeababio18.prm`. Note that `Test.xyz` also includes a column that specify the force field atom type (first column after the coordinates) and the list of atom index to which it is connected. `Test.key` is the input file that tells the code where to find the parameters and provide additional information such as the size of the simulation box, etc. The output splits the energy into different categories, you will recognize a bond, angle, torsion term and others that we have discussed in class (as well as others that we have mentioned could exist but not discussed).

4) In the same directory, run the command `~/Tinker/minimize Test.xyz -k Test.key` and enter `0.1` when prompted. You should get the following result:

```sh

     ######################################################################
   ##########################################################################
  ###                                                                      ###
 ###            Tinker  ---  Software Tools for Molecular Design            ###
 ##                                                                          ##
 ##                      Version 8.10.2   February 2022                      ##
 ##                                                                          ##
 ##               Copyright (c)  Jay William Ponder  1990-2022               ##
 ###                           All Rights Reserved                          ###
  ###                                                                      ###
   ##########################################################################
     ######################################################################


 READXYZ  --  Atom Labels not Sequential, Attempting to Renumber

 Enter RMS Gradient per Atom Criterion [0.01] :  0.1

 Limited Memory BFGS Quasi-Newton Optimization :

 QN Iter     F Value      G RMS      F Move   X Move   Angle  FG Call  Comment

     0    0.1220D+10 0.3005D+07                                   1
   100   -21654.2663     2.4635    111.2192   0.0155   62.45    138    Success 
   200   -24050.0954     1.2165      9.7045   0.0052   77.03    279    Success 
   300   -24784.7981     0.9848      5.6441   0.0052   78.74    379    Success 
   400   -25198.8849     0.5812      3.5862   0.0052   78.52    479    Success 
   500   -25422.1844     0.5102      1.5524   0.0031   76.69    579    Success 
   600   -25546.7526     0.4977      1.1393   0.0027   74.08    679    Success 
   700   -25614.6359     0.3559      0.9373   0.0052   84.46    779    Success 
   800   -25661.0772     0.2595      0.2473   0.0036   84.21    879    Success 
   900   -25699.1452     0.1710      0.2127   0.0012   78.10    979    Success 
  1000   -25719.5534     0.2061      0.2389   0.0021   80.28   1080    Success 
  1100   -25751.9037     0.2389      0.4218   0.0021   78.04   1180    Success 
  1200   -25770.0915     0.1503      0.1401   0.0013   80.83   1280    Success 
  1229   -25773.0198     0.0983      0.0908   0.0004   75.23   1309   SmallGrad

 LBFGS  --  Normal Termination due to SmallGrad

 Final Function Value :       -25773.0205
 Final RMS Gradient :              0.0983
 Final Gradient Norm :             7.9232
```

This is the output of an energy minimization performed on that same input file. You can see that the energy (under the F Value column) is minimized until you reach your convergence criterion (RMS gradient of 0.1 per atom - see the F Move column). Note that there is one additional output file from this calculation: the coordinates of the system after minimization. Tinker labels this file with the same prefix as your input file, adding a number at the end. Here, you should see the file `Test.xyz_2`. However, if you run this multiple times without deleting files, it will keep numbering such that your minimized coordinates may be in `Test.xyz_3,` for example. 

5) In the same directory, run the command `~/Tinker/dynamic Test.xyz_2 -k Test.key 200 1.0 1.0 4 300.00 1.0 > Dynamics.log`. Note that you are using the output of the energy minimization as an input for this dynamic calculation. In this command line, the numbers set the conditions for the molecular dynamic simulation: 

- 200 is the total number of steps
- 1.0 is the time step in femtoseconds
- 1.0 is the time period at which you print information in picosecond
-  4 is the ensemble type specifying a NPT ensemble. A NVT ensemble will be specified by the number 2 (this is a Tinker convention) 
-  300.00 is the temperature 
-  1.0 is the pressure (note that you will not have this entry if you are running a NVT ensemble)


You will normally have three outputs: (i) a log file that we named `Dynamics.log` in the command line, (ii) `Test.dyn` that prints out the velocities in case you need to restart. You should not need to restart a simulation, but if you do, further instructions are available here <https://sites.google.com/site/amoebaworkshop/exercise-1#TOC-Restarting-a-simulation> and (iii) a trajectory file that prints out the frames at intervals specified in the command line. Since we chose 1 ps here and we ran 200 1fs time steps, we do not have a trajectory file. In the simulations you will run below, you will get one, named with the same prefix as your input file and with extension `.arc`.

The `Dynamics.log` file should contain the following

```sh

     ######################################################################
   ##########################################################################
  ###                                                                      ###
 ###            Tinker  ---  Software Tools for Molecular Design            ###
 ##                                                                          ##
 ##                      Version 8.10.2   February 2022                      ##
 ##                                                                          ##
 ##               Copyright (c)  Jay William Ponder  1990-2022               ##
 ###                           All Rights Reserved                          ###
  ###                                                                      ###
   ##########################################################################
     ######################################################################


 Molecular Dynamics Trajectory via Nose-Hoover NPT Algorithm

 Average Values for the Last   100 Out of      100 Dynamics Steps

 Simulation Time              0.1000 Picosecond
 Total Energy            -19892.9251 Kcal/mole   (+/-  29.4041)
 Potential Energy        -22818.7427 Kcal/mole   (+/- 487.2923)
 Kinetic Energy            2925.8176 Kcal/mole   (+/- 471.0430)
 Temperature                  151.24 Kelvin      (+/-    24.35)
 Pressure                    -713.28 Atmosphere  (+/-  3920.39)
 Density                      0.9400 Grams/cc    (+/-   0.0000)

 Average Values for the Last   100 Out of      200 Dynamics Steps

 Simulation Time              0.2000 Picosecond
 Total Energy            -19868.2029 Kcal/mole   (+/-  11.7811)
 Potential Energy        -22589.8186 Kcal/mole   (+/-  73.9033)
 Kinetic Energy            2721.6157 Kcal/mole   (+/-  75.7728)
 Temperature                  140.69 Kelvin      (+/-     3.92)
 Pressure                   -2128.22 Atmosphere  (+/-  3292.82)
 Density                      0.9401 Grams/cc    (+/-   0.0000)
```

6) The individual frames (file with extension `.xyz`) and trajectory files (with extension `.arc`) can be read by the visualization software VMD (<http://www.ks.uiuc.edu/Research/vmd/>) although you may have to manually set the file type as Tinker when loading a new molecule. Install VMD and make sure you can open each type of files. There is a sample `Example.arc` file in the Github directory for this purpose.   
  

##Run your simulations

1) Run an energy calculation on the input file I sent you by email.

2) Run an energy minimization on this input file.

3) Run an energy calculation on the output of the energy minimization.

4) Run a molecular dynamics simulation using the output of the energy minimization (Step 2) as input. Follow the instructions you were given by email. This may take mutliple hours. Plan accordingly.

##Analyze your simulations
NB: Be rigorous and precise. Explain your reasoning. State all the assumptions made.

1) Describe your system.

2) Your input file with extension `.xyz` ends on each line with the list of atom index that a given atom is connected to. This is called the connectivity information. Why do we need connectivity information when using a force field?

3) Plot the energy of your system as a function of the number of iterations performed during the energy minimization. Analyze the graph incorporating the information provided by the two energy calculations you have done.

4) Plot the temperature of your system as a function of time. Calculate the ensemble average of the temperature. Calculate the standard deviation of the temperature. What is the meaning of these fluctuations?

5) Plot the volume of your system as a function of time. Calculate the ensemble average of the volume. Calculate the standard deviation of the volume. What is the meaning of these fluctuations?

6) Plot the pressure of your system as a function of time. Calculate the ensemble average of the pressure. Calculate the standard deviation of the pressure. What is the meaning of these fluctuations?
  
7) Calculate the radial pair distribution function as specified in the email you received. Interpret (you may want to compare to experimental data). Note that you can calculate the radial pair distribution easily in VMD>Extensions>Analysis. See <https://www.ks.uiuc.edu/Research/vmd/plugins/gofrgui/> for instructions if needed. 
