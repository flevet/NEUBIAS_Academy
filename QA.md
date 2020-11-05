# Table of contents
* [Colocalization methods for SMLM](#colocSMLM)
  * [For using Voronoï analysis, is there a github or program that is widely available that I could use?](#github)
  * [Which technique did you use for 3D ( biplane, cylindrical lens, double helix...)?](#3Dtechnique)
  * [Is those two softwares available for mac?](#MacAvailability)
  * [Can Voronoi analysis be applied to segmentations of spots (local maxima segmentation) based on confocal images?](#spotSegmentation)
  * [How large can a dataset be to analyse it with your software? (F.ex is there a maximal number of localizations per dataset)](#maxNb)
  * [Plans for a Fiji plugin?](#FijiPlugin)
  * [I usually acquire two channels seperately. So there will be two lists of coordinates. Is it possible to upload two channels separately in the software you introduced?](#separatedChannels)
  * [Is there an example format of the csv that the program reads?](#exampleCSV)
  * [Is it possible for the Tesseler to be integrated with Napari?](#NapariIntegration)
* [Statistical tools for analyzing the spatial distribution of objects](#statsColocObjects)
  * [For SODA, can I import a csv? Is there an example format? If I have an error in each colour localisation. Can it be incorporated in the calculations?](#SODAcsv)
  * [For the formation of the annuli for non circular objects, does it match the contours of the object or does it assume circular shells?](#annuli)
  * [How do you apply icy to SML data?](#icySMLM)
  * [Can localisation error be incorporated in the two colour analysis?](#locError)
  

# Colocalization methods for SMLM <a name="colocSMLM"></a>

## For using Voronoï analysis, is there a github or program that is widely available that I could use? <a name="github"></a>
The SR-Tesseler Github acount (with a Windows installer and the code-source) is [here](https://github.com/flevet/SR-Tesseler).

The Coloc-Tesseler Github acount (with only a Windows installer) is [here](https://github.com/flevet/Coloc-Tesseler).

## Which technique did you use for 3D ( biplane, cylindrical lens, double helix...)? <a name="3Dtechnique"></a>
All the data we produced were using astigmatism. Nevertheless, both SR- and Coloc-Tesseler use as input point clouds (coordinates of the localization) already localized with an other software platform (Thunderstorm for instance). As such, they can quantify point clouds localized with any of these 3D modalities.

## Is those two softwares available for mac? <a name="MacAvailability"></a>
There is no installer for Mac as of right now. Code-source of SR-Tesseler is available on my Github [here](https://github.com/flevet/SR-Tesseler). I tried to not use the MFC API thus it should be possible to compile it for Mac with little code modification. Code-source of Coloc-Tesseler is still not available. I'm currently merging SR- and Coloc-Tesseler in the same platform that will be released as open-source in the near future.

## Can Voronoi analysis be applied to segmentations of spots (local maxima segmentation) based on confocal images? <a name="spotSegmentation"></a>
I'm not sure to exactly understand the question. Voronoi diagrams are constructed from a set of seeds, hence they need to be identified beforehand (and that could be the local maxima?). They could be used to identify local maxima (as point) in a set of points though. 

## How large can a dataset be to analyse it with your software? (F.ex is there a maximal number of localizations per dataset) <a name="maxNb"></a>
In the public version of SR-Tesseler, the software can construct a 2D Voronoi diagram with a dataset composed of a few millions of points. If the data has more than 3 or 4 millions of points, the generation time will take dozens of hours.

This limitation will be lifted in the merged SR- and Coloc-Tesseler platform. In this platform, generating 2D and 3D Voronoi for millions of points will be possible in a few minutes.

## Plans for a Fiji plugin? <a name="FijiPlugin"></a>
There is no plan to integrate the methods in Fiji. We develop our methods in C++ in order to be sure to have a very efficient method. As FIJI is developed in Java, converting our software to Java will take a huge time as Tesseler is composed of thousands of line of code. Such an effort would necessitate hiring a developer specifically on this problem.

## I usually acquire two channels separately. So there will be two lists of coordinates. Is it possible to upload two channels separately in the software you introduced? <a name="separatedChannels"></a>

Well, in fact this is the only way to use Coloc-Tesseler. You have to have separate localization files for both channels.

## Is there an example format of the csv that the program reads? <a name="exampleCSV"></a>
There is an exemple in the SR-Tesseler manual. Basically, the format we are using is the the one defined by the csv files generated with Thunderstorm.

## Is it possible for the Tesseler to be integrated with Napari? <a name="NapariIntegration"></a>
Napari is developed in Python. Unfortunately, the answer is the same as for the FIJI integration: it will necessitate too much work as we will need a developer to work full-time on this.

# Statistical tools for analyzing the spatial distribution of objects <a name="statsColocObjects"></a>

## For SODA, can I import a csv? Is there an example format? If I have an error in each colour localisation. Can it be incorporated in the calculations? <a name="SODAcsv"></a>
Yes, you can import a csv file. But you should first convert it in a .xls file (one column per parameter). Then to import your localizations in your Icy protocol, you need to use the "Import localizations" block (http://icy.bioimageanalysis.org/plugin/import-localizations-from-files/) where you specify which columns correspond to x,y,z coordinates, and which one corresponds to the number of collected photons (if any, in that case you can specify a threshold (quantile) above which you keep localizations).

## For the formation of the annuli for non circular objects, does it match the contours of the object or does it assume circular shells? <a name="annuli"></a>
For SODA, it creates circular annuli. But we do have another work (available [here](https://ieeexplore.ieee.org/abstract/document/9122432)) that does match each object contour. The corresponding G-SODA plugin will be soon available on Icy.

## How do you apply icy to SML data? <a name="icySMLM"></a>
To apply Icy and SODA to SML data, you need to import your localizations using the "import localizations" block (see <a name="SODAcsv"></a>), and use the SODA STORM block (you can find examples of protocols in 2D here http://icy.bioimageanalysis.org/protocol/soda-storm-2d/ and 3D there: http://icy.bioimageanalysis.org/protocol/soda-storm-3d/).

## Can localisation error be incorporated in the two colour analysis? <a name="locError"></a>
This is a work in progress.
