# LSDTopoTools and LSDTopoTools Terraces Installation instructions (Ubuntu 19.10)
See bottom of page for links to the LSDTopoTools official installation guides and help.

## Install LSDTopoTools2 (LSDTT2)

This program contains the main landscape analysis and channel-extraction algorithms.

### Prerequisites

```sh
sudo apt install git
sudo apt install cmake
sudo apt install libpcl-dev
sudo apt install libfftw3-dev
```

### Download

Make a directory for LSDTT2 and download it from GitHub.
```sh
mkdir $HOME/LSDTopoTools
cd $HOME/LSDTopoTools
git clone https://github.com/LSDtopotools/LSDTopoTools2.git
```

### Install

Navigate to newly created LSDTopoTools2 directory and run the start up script.
```sh
cd $HOME/LSDTopoTools/LSDTopoTools2
sh lsdtt2_setup.sh
```

If this completed successfully, LSDTT2 should now be installed on your computer. You can test it by typing:
```sh
cd bin
./
```

### Add LSDTT2 to your path

#### Permanent path

Open your bash profile.
```sh
vi ~/.profile
```
*Note: sometimes you will edit `~/.bashrc` instead of `~/.profile`.*

Add the following lines, with a path to LSDTopoTools that is updated for your computer, to the bottom of your `.profile`. Before pasting, press `i` to enter "insert mode" in vi:
```sh
# LSDTT2
export PATH="$PATH:/home/<YOUR_USERNAME>/LSDTopoTools/LSDTopoTools2/bin"
```
*To paste within the terminal, press `CTRL+SHIFT+V`.*

Then press `escape` to return to command mode, and `:x`, followed by `enter`, to save and exit.

This will set your path each time you open a new terminal window. However, you opened your current terminal window before changing your `.profile`. To refresh your environment variables (such as your `PATH`) from your `.profile`, type:
```sh
source ~/.profile
```

Test whether the install worked by typing `lsdtt` in any directory EXCEPT `$HOME/LSDTopoTools/LSDTopoTools2/bin` and seeing if it will autocomplete (i.e., by hitting tab twice).

#### Temporary path

Instead of altering your profile, you can also just run a startup command for LSDTT2 directly in your terminal window every time you want to start an LSDTT2 session.

To do this, navigate to `$HOME/LSDTopoTools/LSDTopoTools2` and run:
```sh
sh lsdtt2_terminal.sh
```
This will start up LSDTT2 in your terminal window. You can now access all the LSDTT2 tools by simply typing the command name (`lsdtt-<something>`) followed by the driver file name into the command prompt.


#### You can now test the install by running a channel extraction on example data.

## Getting Example Data
Download the example data to test the LSDTT2 install and to test the Terraces at a later step. (Set to do on your desktop, but you can do this anywhere.)

```sh
# Test is set for your desktop, but you can do this anywhere
cd $HOME/Desktop

# Create folder
mkdir Eel_River_Terrace_Example
cd Eel_River_Terrace_Example

# Download DEM
wget https://raw.githubusercontent.com/LSDtopotools/ExampleTopoDatasets/master/FloodplainTerraceData/Eel_River_DEM.bil

wget https://raw.githubusercontent.com/LSDtopotools/ExampleTopoDatasets/master/FloodplainTerraceData/Eel_River_DEM.hdr

# Download coordinates file: upstream and downstream ends of analysis reach (for Terraces later)
wget https://raw.githubusercontent.com/LSDtopotools/ExampleTopoDatasets/master/example_parameter_files/ExampleFiles_TerraceExtraction/Eel_River_DEM_coordinates.csv
```
For more example data to use to test LSDTT2, see 
https://lsdtopotools.github.io/LSDTT_documentation/LSDTT_installation.html#_getting_the_example_data

## Test the LSDTT2 Install 
Running a channel extraction using the example data to test the LSDTT2 install.

#### Create a parameter file
In the Eel River folder you just downloaded the example data into, create a parameter file. This will tell the channel extractor what to do.

```sh
vi eel_area_threshold.param
```

Populate your parameter file with this text. Modify the read and write path to match your computer's file system:

```sh
# Parameters for channel extraction
# Comments are preceeded by the hash symbol
# Documentation can be found here:
# https://lsdtopotools.github.io/LSDTT_documentation/LSDTT_channel_extraction.html

# These are parameters for the file i/o
# IMPORTANT: You MUST make the write directory: the code will not work if it doesn't exist.
read path: /home/<YOUR_USERNAME>/Desktop/Eel_River_Terrace_Example
write path: /home/<YOUR_USERNAME>/Desktop/Eel_River_Terrace_Example
read fname: Eel_River_DEM
write fname: Eel_River_DEM
channel heads fname: NULL

# Parameter for filling the DEM
min_slope_for_fill: 0.0001

# Parameters for selecting channels and basins
threshold_contributing_pixels: 5000
print_area_threshold_channels: true
print_wiener_channels: false
print_pelletier_channels: false
print_dreich_channels: false

# write hillshade: true
print_stream_order_raster: true
print_sources_to_csv: true
```
Save and exit from the parameter file.

For more information on input parameters, see 

https://lsdtopotools.github.io/LSDTT_documentation/LSDTT_basic_usage.html#_basic_file_input_and_output

#### Run the Channel Extraction

Now, run the channel extraction.
```sh
lsdtt-channel-extraction eel_area_threshold.param
```
This will output a number of files, including your channel network (Eel_River_DEM_AT_CN.csv) and your channel heads (EEL_River_DEM_ATsources.csv). Test that these make sense by viewing these files in a GIS.

Once you have your extracted channel network and channel heads, you can run the Terrace tool. You must first download and install it.

## Install LSDTT2 Terrace driver.
Downloading and installing the LSDTT2 Terrace driver.

Make sure you have already downloaded LSDTT2 and tested it by running a channel extraction before proceeding to this step. Ensure you have all the prerequesite packages installed as well.

```sh
cd $HOME/LSDTopoTools
git clone https://github.com/LSDtopotools/LSDTopoTools_FloodplainTerraceExtraction.git
cd LSDTopoTools_FloodplainTerraceExtraction/driver_functions_Floodplains-Terraces/
sh get_terraces.sh
```
This will create the executable `get_terraces.out`.

Going forward, you can run the terrace driver by navigating to this folder and typing the command:
```sh
./get_terraces.out /path/to/DEM/location/ name_of_parameter_file.param
```
You can also create a permanent path in your `.profile` going forward.

#### Permanent path for terraces

Open your bash profile.
```sh
vi ~/.profile
```
*Note: sometimes you will edit `~/.bashrc` instead of `~/.profile`.*

Add the following lines, with a path to `.get_terraces.out` that is updated for your computer, to the bottom of your `.profile`. If you already added in LSDTopoTools to your permanent path, you can add the terraces below this. Before pasting, press `i` to enter "insert mode" in vi:
```sh
# LSDTT2 Terraces
export PATH="$PATH:/home/<YOUR_USERNAME>/LSDTopoTools/LSDTopoTools_FloodplainTerraceExtraction/driver_functions_Floodplains-Terraces"
```
*To paste within the terminal, press `CTRL+SHIFT+V`.*

Then press `escape` to return to command mode, and `:x`, followed by `enter`, to save and exit.

This will set your path each time you open a new terminal window. However, you opened your current terminal window before changing your `.profile`. To refresh your environment variables (such as your `PATH`) from your `.profile`, type:
```sh
source ~/.profile
```

Test whether the install worked by typing `get_terraces` in any directory EXCEPT `$HOME/LSDTopoTools/LSDTopoTools_FloodplainTerraceExtraction/driver_functions_Floodplains-Terraces` and seeing if it will autocomplete (i.e., by hitting tab twice).

In order to use the Terrace driver going forward you will just need to enter:

```sh
get_terraces.out /path/to/DEM/location/ name_of_parameter_file.param
```
Ensure that your DEM and parameter file are both in the same folder before running this command. 

We will now test the terrace install.

## Test the Terrace install

To run the terraces you must have a channel heads (or "sources") file: Eel_River_DEM_ATsources.csv created by running the channel extraction, a coordinates file identifying upstream and downstream points (Eel_River_DEM_coordinates.csv), and a parameter file (LSDTT_terraces.param) in your folder.

Navigate to the Example Data folder.
```
cd $HOME/Desktop/Eel_River_Terrace_Example
```
Create `LSDTT_terraces.param` and populate it with the text below. Edit it to include the proper path to
your `Eel_River_Terrace_Example` folder.

The parameter file should have this text (modify the `read path` for your computer):

```sh
# This is a parameter file for LSDTopoTools
# Any lines with the # symbol in the first row will be ignored

# File information
dem read extension: bil
dem write extension: bil
read path: /LSDTopoTools/Topographic_projects/Eel_River/
read fname: Eel_River_DEM
CHeads_file: Eel_River_DEM_ATsources
coords_csv_file: Eel_River_DEM_coordinates.csv

# Parameters for DEM processing
Filter topography: true
Min slope filling: 0.0001
surface_fitting_window_radius: 6

# Parameters for swath post-processing
HalfWidth: 1000
search_radius: 10
Threshold_SO: 3
NormaliseToBaseline: 1

# Pararmeters for terrace extraction
QQ threshold: 0.005
Relief lower percentile: 25
Relief upper percentile: 75
Slope lower percentile: 25
Slope upper percentile: 75
Min patch size: 1000
Min terrace height: 5
```
For more information on input parameters for the terrace extraction, see LSDTT2's help page (Table 2, Section 5.3): https://lsdtopotools.github.io/LSDTT_documentation/LSDTT_floodplains_terraces.html#_creating_the_parameter_file

#### Now, test the Terrace driver by running the Eel River Example.

Navigate back to the Terrace driver functions folder.

```sh
cd $HOME/LSDTopoTools/LSDTopoTools_FloodplainTerraceExtraction/driver_functions_Floodplains-Terraces/
```

Run the terrace code inserting the path to your Eel River files.
```sh
./get_terraces.out $HOME/Desktop/Eel_River_Terrace_Example LSDTT_terraces.param
```

This will create a number of files. Including:

DEM_name_terrace_IDs.bil and .hdr: A raster of terrace locations with each terrace labelled with a unique ID

Try opening this .bil in GIS to see how your terraces look.

**You should now have both LSDTT2 and the Terraces driver successfully installed. For more detailed instructions and troubleshooting, see links below.**

## Tips and Troubleshooting

### Terraces

* The filtered DEM takes some time to make. It is best to make it only once, and then update the `.param` file to point to this if you need to make any changes.
* The filtered DEM has an area that is smaller than the unfiltered DEM. The channel endpoints must lie some distance from the edges of this. Leave some space as padding.
* The output terraces file(s) will give entirely NULL values if any part of the swath (whose topogrpahy the algorimth analyzes to find terraces) is outside of the DEM (and therefore contains a NULL value).

## LSDTT2 Official Installation instructions:
The steps above were adapted from the installation guides below.

### LSDTT2 Documentation Home Page
https://lsdtopotools.github.io/LSDTT_documentation/

### LSDTT2 Installation:
https://lsdtopotools.github.io/LSDTT_documentation/LSDTT_installation.html#_installing_natively_on_a_linux_system

### Terraces:
https://lsdtopotools.github.io/LSDTT_documentation/LSDTT_floodplains_terraces.html#_the_methodology
