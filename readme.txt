***********************************************
6/4/2014
wei Tian
w.tian@umiami.edu
***********************************************
1. test if single precsion source codes are right.  Results show it is right.
2. change single into double
   --use lf instead of f in sci_reader
   --solver_gs.c: tmp
3. revise on coupled simulation part of FFD and generate DLL of double precision
   --put REAL definition into modelica_ffd_common.h such that REAL can be used everywhere
   --change the area read from Modelica as float
   --tested sucessfully
4. The third step is not accurate and can't take full advantage of double precision. That is because we modelica gives data to ffd, the external c function truncate the double precison into a single precision, which definitely would result in loss of accuracy on FFD side. FFD's output is also truncated into single precision before sendint to modelica, because the predefined common data which can be accessed by both FFD and modelica is also set to be single precision in modelica_ffd_common. Then, in this step, I did following change based on the third step:
   -- change the data defined in modelica_ffd_common into double precision
   -- change the external c functions into double precision 
      --delete fload in compare_area, added in last step.
      --change float in modelica_ffd_common into REAL
      --change all the things in external c functions from float to double
      --update the FastFluidDynamics if source codes are edited outside the folder, because modelica_ffd_common is cited. 
      --update the dll and lib in the Buildings/bin/
5. upload to the github (https;//github.com/tianwei1989/modelica-buildings.git)
   when shift between double precisiona and single precision please remember to check the below three points:
   -- *.lib and *.dll in Resource/bin
   -- c-source in Resources/
   -- FastFluidDynamics in Resources/src/
*****************************************************************************************