# LSDTTInstallationGuides

# Installation instructions (Ubuntu 19.10)

## Install LSDTT

### Some prerequisites

```sh
sudo apt install git
sudo apt install cmake
sudo apt install libpcl-dev
sudo apt install libfftw3-dev
```

#### Make a directory for LSDTT and download it from GitHub.
```sh
mkdir LSDTopoTools 
cd LSDTopoTools
git clone https://github.com/LSDtopotools/LSDTopoTools2.git
```
#### Navigate to newly created LSDTopoTools2 directory and run the start up script.

```sh
cd LSDTopoTools2
bash lsdtt2_setup.sh
```
#### Add the binary to your local path.

```sh
cd bin
pwd
export PATH=/path/to/your/bin:$PATH

```
LSDTT should now be installed on your computer. Test it is running by typing lsdtt and see if it will autocomplete. If not, run the start up code described in the next step and test again.

# Important note
Every time you want to use LSDTT, you must navigate to ~/LSDTopoTools/LSDTopoTools2 and run

```sh
sh lsdtt2_terminal.sh
```
This will start up LSDTT in your terminal window. You can now access all the LSDTT tools by simply typing the driver in the command line. 

## Test the install by running a channel extraction on example data.
### Download the example data to test the LSDTT install and to test the Terraces at a later step.
(Set to do on your desktop, but you can do this anywhere.)

```sh
# Test is set for your desktop, but you can do this anywhere
cd $HOME/Desktop

# Create folder
mkdir Eel_River_Terrace_Example
cd Eel_River_Terrace_Example

# Download DEM
wget https://raw.githubusercontent.com/LSDtopotools/ExampleTopoDatasets/master/FloodplainTerraceData/Eel_River_DEM.bil

wget https://raw.githubusercontent.com/LSDtopotools/ExampleTopoDatasets/master/FloodplainTerraceData/Eel_River_DEM.hdr

# Download parameter file (Text file that tells LSDTT what to do)
wget https://raw.githubusercontent.com/LSDtopotools/ExampleTopoDatasets/master/example_parameter_files/ExampleFiles_TerraceExtraction/LSDTT_terraces.param

# Download coordinates file: upstream and downstream ends of analysis reach
wget https://raw.githubusercontent.com/LSDtopotools/ExampleTopoDatasets/master/example_parameter_files/ExampleFiles_TerraceExtraction/Eel_River_DEM_coordinates.csv
```

### Create the parameter file
```sh
vi eel_area_threshold.driver




# Install LSDTT Terrace driver.


The floodplains driver works similarly.
```sh
git clone https://github.com/LSDtopotools/LSDTopoTools_FloodplainTerraceExtraction.git
cd LSDTopoTools_FloodplainTerraceExtraction/driver_functions_Floodplains-Terraces/
sudo bash get_terraces.sh

```



Next, open `LSDTT_terraces.param` and edit it to include the proper path to
your `Eel_River_Terrace_Example` folder.

Now, test the Terrace driver by runnning the Eel River Example.

```sh
#Navigate back to the Terrace driver functions folder.

cd LSDTopoTools_FloodplainTerraceExtraction/driver_functions_Floodplains-Terraces/

#Run the terrace code inserting the path to your Eel River files.

./get_terraces.out /path/to/DEM/location/ name_of_parameter_file.param

```







Is lsdtt-channel-extraction something that should be installed in a preceding step via LSDTT2 and pip?
I'm turning these into a Markdown guide that I will put on our group's GitHub.




