# Using GIS to test terrace-definition thresholds

The terrace tool in LSDTopoTools ([Clubb et al., 2017](https://www.earth-surf-dynam.net/5/369/2017/)) picks terraces by:

1. Generating a swath of elevations above a channel long profile following the method of [Hergarten et al. (2014)](https://www.earth-surf-dynam.net/2/97/2014/esurf-2-97-2014.html);
2. Finding slope and relief-above-channel thresholds to define terraces, based on user input and (most commonly) an analysis of the hypsometry within the swath; and
3. Using these along with a contiguous-area thresholds to define terrace surfaces, number them sequentially, and map the structure of their relief above the channel.

But what if the initially selected thresholds do not select the terraces as you hoped? LSDTT is set up to re-build the swath, which can take a long time as you test different inputs.

However, LSDTT ***exports the swath raster***, to which you can apply arbitrary thresholds and test which landscape features are picked out of the swath. If using this, don't forget to save and record the threshold values that you used in order to keep your analyses reproducible!

Variable definitions for the following examples (replace these with your raster map names):
* **swath**: The swath raster of elevations above the channel
* **swath_raster_filename**: The name of the swath raster file (as opposed to the name of the raster when included in a GIS)
* **slope**: Slope from the swath raster
* **R_min**: lower relief (above channel) threshold
* **R_max**: upper relief (above channel) threshold
* **S_min**: lower slope threshold (you may not want to use this)
* **S_max**: upper slope threshold
* **terrace_relief**: Raster with relief of terraces only

## GRASS GIS

***Untested***

```sh
# Import swath raster
r.in.gdal input=swath_raster_filename output=swath
# Obtain slope
# Note that this is in percent: multiply your usual threshold(s) by 100.
r.slope.aspect input=swath output=slope format=percent
# Select what you think will be the terraces
r.mapcalc "terrace_relief = swath * ( ( slope > S_min) * (slope <= S_max) \
                                        * (swath > R_min) * (swath <= R_max) )"
# Convert the 0s to NULL values
r.null map=terrace_relief setnull=0
```

After this, you can view the raster using `d.mon` or the GUI, and you can export it using `r.out.gdal`.

## QGIS

1. Import **swath_raster_filename** as **swath** using the data source manager.
2. Create the slope raster (Menu: Raster --> Analysis --> Slope).
3. Use the raster calculator compute a raster using the slope and relief thresholds. (Menu: Raster --> Raster Calculator). Your expression may look something like this: `("swath@1" > R_min) * ("swath@1" <= R_max) * ("slope@1" <= S_max) * "swath@1"`. You can save the output terrace map to a non-temporary file and/or display it in the current workspace.
4. Convert 0s to NULL values. This can be done with [Menu: Raster --> Conversion --> Translate] or by opening the processing toolbox, finding `r.null` from within the `GRASS` tools, and applying it to the raster.

***Or you can use GRASS within QGIS!*** QGIS contains by default all of the GRASS GIS tools! Therefore, you can apply a similar approach to the GRASS one (above) as well, instead of the one enumerated above, but with a graphical interface.

## Additional steps

While not covered here, you can also compute the histogram of the topography using these GIS tools and find the percentile(s) where your desired terrace-picking thresholds lie. You may also have noticed that we did not apply any minimum-area threshold. This can also be applied. It is also straightforward to vectorize your terraces (e.g., `r.to.vect` in GRASS), manipulate them in other ways, and produce output maps for publication. Just remember to record all your steps and keep your analyses reproducible.
