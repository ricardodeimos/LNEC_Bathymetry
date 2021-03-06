# LNEC_Bathymetry

This directory contains all the scripts and files needed to run the LNEC application.

Things to take into account:

You can use the script CreateOutputGrid.py to create the grid or your own script. It's your call.

The image K5_Sesimbra_zoom.tif is being used with the run_wings.sh bash script and the zip S1B_IW_GRDH_1SDV_20170724T064228_20170724T064253_006626_00BA71_D464 is being used with the run_wings_preprocessing.sh bash script.

If you want to try another image or another zip, make sure to change the name accordingly in the respective bash file and adjust the Tilling, FFT and Merge calls.

Let's say you want to run run_wings.sh with an image called Aveiro.tif in which the grid has 200 grid points:


./Tiling.py -i ./K5_Sesimbra_zoom.tif  -u tile-zip-1 tile-zip-2 ... -u tile-zip-98 tile-zip-99 ----------------------> ./Tiling.py -i ./Aveiro.tif -u tile-zip-1 tile-zip-2 ... -u tile-zip-199 tile-zip-200



./FFT_bath.py -i tile-zip-1 -o textfilebath_1.txt -a config_file.ini -t 13 --------------------> ./FFT_bath.py -i tile-zip-1 -o textfilebath_1.txt -a config_file.ini -t

./FFT_bath.py -i tile-zip-2 -o textfilebath_2.txt -a config_file.ini -t 13 --------------------> ./FFT_bath.py -i tile-zip-2 -o textfilebath_2.txt -a config_file.ini -t 13

...												... (more calls to FFT here than before)

./FFT_bath.py -i tile-zip-98 -o textfilebath_98.txt -a config_file.ini -t 13 ------------------> ./FFT_bath.py -i tile-zip-199 -o textfilebath_199.txt -a config_file.ini -t 13

./FFT_bath.py -i tile-zip-99 -o textfilebath_99.txt -a config_file.ini -t 13 ------------------> ./FFT_bath.py -i tile-zip-200 -o textfilebath_200.txt -a config_file.ini -t 13


./Merge.py -i textfilebath_1.txt textfilebath_2.txt ... -i textfilebath_98.txt textfilebath_99.txt  ---> 

./Merge.py -i textfilebath_1.txt textfilebath_2.txt ... -i textfilebath_199.txt textfilebath_200.txt


Now, if you run the bash script: run_wings_preprocessing.sh, there are going to be some errors with NaNs. Those are the errors that arrive when we use the preprocessing tools (calibration and speckle filter).

Since the errors probably depend on your functions in CSAR.py, as discussed previously, we think that you're in the best position to solve the errors. Ŵe already checked that calibration and speckle filter behave as expected.

Take into account that both the calibration and the speckle filter scripts have some default values for the arguments. You can change this as you see fit. Example:

parser.add_argument('--PoutputGammaBand',
                    dest="PoutputGammaBand", metavar='<boolean>',
                    help="Output gamma0 band.",
                    type=bool,
                    default=False)
                    
You are free to change the default to True, if you want.

If you have any doubt please contact Nuno Grosso and put Ricardo Alberto in CC.

Good luck!
