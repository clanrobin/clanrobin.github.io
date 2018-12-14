# Gallery

Here is a compilation of data visualization examples that I have generated as part of my research and work.

## Multi-Component Fitting and Data Slicing to reveal Milky Way Substructure

In this project we analyzed the entire 20,000 square degrees of the PanSTARRS-1 survey. This is an imaging survey of about 1/3 of the entire night sky. It includes all the Northern Hemisphere and a large portion of the Southern Hemisphere. We divided it into 4x4 degree blocks and generated stellar density profiles for each region as a function of distance. In simple terms, we are making an estimate of the number of stars in that direction per distance bin.

This can be seen as the data points in this first figure. Having obtained this, we can attempt the understand the distribution of the stars in that particular direction by fitting a model. In this case, the model describes different structural components of the Milky Way and the sum of the those components should match the data. By fitting them we can gain insights into the Milky Way itself.

![Multi-Component Fits](https://clanrobin.github.io/Gallery/Multi-ComponentFitting.png)

Given that this data has around 20,000 lines-of-sight we can reconstruct an image of the night sky, as seen by this telescope where we select not only the distance we wish to probe, but also scale it to the stellar density at that distance. In this next figure, the footprint of each line-of-sight (pixel) is rendered to the exact shape it needs to be to fit into this Aitoff projection. The stellar densities are scaled to highlight the features. 

The Milky Way runs across the x-axis and is excised from the figure, so as not to obscure the faint stellar streams and overdensities in the Milky Way Halo. The large white portions at the edges of the map are the regions of sky that the telescope cannot see.

![Aitoff Projection of Milky Way Stellar Densities](https://clanrobin.github.io/Gallery/ACS_EBS_nowf_distplots.png)

## Leveraging Data from Multiple Sources

In this example, we show the data from our survey of the Magellanic Bridge, this is the region between our two nearest companion dwarf galaxies (The Large and Small Magellanic Clouds). We are interested in studying how gas in this region relates to the stars that we detected in our survey. So the background contours are from a Radio survey taken to study the gas, where we have sliced that dataset to the gas velocity we require. Then we have overplotted both the footprint of the observations we took (in green) and a subset from that which represents the youngest stars in our data. In this manner, we can directly relate the location of the young stars with respect to the underlying gas from which they might have been born. All of this is plotted on an accurate scale.

![Young Stars in the Magellanic Bridge](https://clanrobin.github.io/Gallery/MAGIC1_2_YMS_SMC_MB_gasmaps.png)

## Building Images from Spectra

Spectroscopic observations split the light from the object (star/galaxy/nebula etc) so that we can see at which wavelengths this object predominantly absorbs or emits light. This then gives us information as to what sort of elements or molecules might be present in the system and their abundances. Different elements can also be used as diagnostic tools, for instance, determining the temperature of the object. So typically we do not create images from this sort of data, however, if you plan your observations carefully, it is possible to reconstruct the object by slicing and compiling the spectra to rebuild the image. This is similar to how the Milky Way image in the first figure was made by slicing the stellar density array at the distances of interest. In this case, we will be slicing the spectra at certain wavelengths to reveal structure.

{Full Disclaimer: this analysis is work done by Rhiannon Gardner who was the student of myself and Dr Djazia Ladjal at Gemini South Observatory. I designed and implemented the survey and supervised Rhiannon with Dr Ladjal in developing the technique to process the data.}

A single observation consists of placing a slit across the object and the light which passes through the slit is dispersed across a grating which generates the spectra. For a given spectra, only the y-direction (along the slit) contains any spatial information about the object. The x-direction becomes the wavelength of the light that is being detected.

In this figure we can see how each the slit is pointed onto the object and a spectra is obtained. Then what we do is we move the spectra one slit-width over and take another spectra. We then repeat this until we have a spectra at each position across the object. This can might need anywhere between 50-100 observations.

![OnSky](https://clanrobin.github.io/Gallery/slitscanmethod.001.png)

A single observation produces data which looks like this:

![OnSky](https://clanrobin.github.io/Gallery/slitscanmethod.002.png)

If we take a single pixel row from the above image, the typical spectra then looks like this and a lot of work is done to remove the background continuum and then isolate the lines. The blue line is the corrected spectrum and the red line shows where the continuum was and to fit that the emission lines needed to masked out to avoid skewing the fit.

![Spectra](https://clanrobin.github.io/Gallery/SpectraProcessing.png)

Once the spectra is processed, the lines need to identified, and in this figure you can see the lines isolated after a multi-gaussian fitting algorithm has been applied and some of the lines have been named.

![Finding the emission lines](https://clanrobin.github.io/Gallery/SpectraAnalysis.png)

Putting all this together, we can take all the spectra, select the line of interest and then reconstruct the image.
In the following image, every pixel in the x-direction represents a single observation as discussed about and every pixel in the y-direction was a spectra which was processed to remove the background, fit the lines, identify them, measure their intensity and then select them for plotting. There are about 90000 spectra analyzed to produce this image. Here you can see the data at different wavelengths to highlight how the structure changes depending on the line being used.

![Step 1](https://clanrobin.github.io/Gallery/PNe_atdiffwavelengths.png)

![Step 1](https://clanrobin.github.io/Gallery/Presentation_JustPal5_BCC.001.png)

![Step 1](https://clanrobin.github.io/Gallery/Presentation_JustPal5_BCC.001.png)

![Step 1](https://clanrobin.github.io/Gallery/Presentation_JustPal5_BCC.001.png)

![Step 1](https://clanrobin.github.io/Gallery/Presentation_JustPal5_BCC.001.png)

![Step 1](https://clanrobin.github.io/Gallery/Presentation_JustPal5_BCC.001.png)

![Step 1](https://clanrobin.github.io/Gallery/Presentation_JustPal5_BCC.001.png)


