# nicar20-imagery-with-bash
[Reference](https://natelandau.com/my-mac-osx-bash_profile/) on Bash profiles.

Here are a few basic [Bash](https://en.wikipedia.org/wiki/Bash_(Unix_shell)) scripts to get you started with earth imagery processing in the terminal.

Dependencies: [FFmpeg](https://www.ffmpeg.org/), [GDAL](https://gdal.org/), [Gifsicle](https://www.lcdf.org/gifsicle/), [ImageMagick](https://imagemagick.org/index.php)

If [wrangling imagery in the terminal](https://medium.com/planet-stories/a-gentle-introduction-to-gdal-part-1-a3253eb96082) is not your cup of tea 🍵 and you're lucky enough to have a Photoshop license, have no fear. The concepts outlined by [Tom Patterson](https://twitter.com/MtnMapper) for Landsat 8 [here](http://www.shadedrelief.com/landsat8/) can be easily applied to other sensors.  

If you'd like to compose your imagery in the browser, [Pierre Markuse](https://twitter.com/Pierre_Markuse) and [others](https://custom-scripts.sentinel-hub.com/) share custom scripts for visualizing things like [fire](https://apps.sentinel-hub.com/eo-browser/#lat=43.50174856516506&lng=16.510391235351562&zoom=11&datasource=Sentinel-2%20L1C&time=2017-07-17&preset=CUSTOM&layers=B01,B02,B03&evalscript=Ly8gKioqCi8vIFZpc3VhbGl6aW5nICh3aWxkKWZpcmVzIGluIFNlbnRpbmVsLTIgaW1hZ2VyeQovLyBGb3IgdXNlIGluIFNpbmVyZ2lzZSBFTyBCcm93c2VyIChodHRwOi8vYXBwcy5zZW50aW5lbC1odWIuY29tL2VvLWJyb3dzZXIpCi8vIFBpZXJyZSBNYXJrdXNlIChAcGllcnJlX21hcmt1c2UpCi8vICoqKgovLyBGdW5jdGlvbnMKZnVuY3Rpb24gQShhLCBiKSB7cmV0dXJuIGEgKyBifTsKZnVuY3Rpb24gc3RyZXRjaCh2YWwsIG1pbiwgbWF4KSAgewoJcmV0dXJuICh2YWwgLSBtaW4pIC8gKG1heCAtIG1pbik7Cn0KCi8vIEJhbmQgY29tYmluYXRpb25zCnZhciBOYXR1cmFsQ29sb3JzID0gW3N0cmV0Y2goMy4xICogQjA0LDAuMDUsMC45KSwgc3RyZXRjaCgzICogQjAzLDAuMDUsMC45KSwgc3RyZXRjaCgzLjAgKiBCMDIsMC4wNSwwLjkpXTsKdmFyIEVuaGFuY2VkTmF0dXJhbENvbG9ycyA9IFtzdHJldGNoKCgzLjEgKiBCMDQgKyAwLjEgKiBCMDUpLDAuMDUsMC45KSwgc3RyZXRjaCgoMyAqIEIwMyArIDAuMTUgKiBCMDgpLDAuMDUsMC45KSwgc3RyZXRjaCgzICogQjAyLDAuMDUsMC45KV07CnZhciBOSVJTV0lSQ29sb3JzID0gW3N0cmV0Y2goMi42ICogQjEyLDAuMDUsMC45KSwgc3RyZXRjaCgxLjkgKiBCMDgsMC4wNSwwLjkpLCBzdHJldGNoKDIuNyAqIEIwMiwwLjA1LDAuOSldOwp2YXIgUGFuQmFuZCA9IFtzdHJldGNoKEIwOCwwLjAxLDAuOTkpLHN0cmV0Y2goQjA4LDAuMDEsMC45OSksc3RyZXRjaChCMDgsMC4wMSwwLjk5KV07CnZhciBOYXR1cmFsTklSU1dJUk1peCA9IFtzdHJldGNoKCgyLjEgKiBCMDQgKyAwLjUgKiBCMTIpLDAuMDEsMC45OSksIHN0cmV0Y2goKDIuMiAqIEIwMyArIDAuNSAqIEIwOCksMC4wMSwwLjk5KSwgc3RyZXRjaCgzLjIgKiBCMDIsMC4wMSwwLjk5KV07CnZhciBQYW5UaW50ZWRHcmVlbiA9IFtCMDggKiAwLjIsIEIwOCwgQjA4ICogMC4yXTsKdmFyIEZpcmUxT1ZMID0gW3N0cmV0Y2goKDIuMSAqIEIwNCArIDAuNSAqIEIxMiksMC4wMSwwLjk5KSsxLjEsIHN0cmV0Y2goKDIuMiAqIEIwMyArIDAuNSAqIEIwOCksMC4wMSwwLjk5KSwgc3RyZXRjaCgyLjEgKiBCMDIsMC4wMSwwLjk5KV07CnZhciBGaXJlMk9WTCA9IFtzdHJldGNoKCgyLjEgKiBCMDQgKyAwLjUgKiBCMTIpLDAuMDEsMC45OSkrMS4xLCBzdHJldGNoKCgyLjIgKiBCMDMgKyAwLjUgKiBCMDgpLDAuMDEsMC45OSkrMC41LCBzdHJldGNoKDIuMSAqIEIwMiwwLjAxLDAuOTkpXTsKLy8gQ2hvb3NlIGEgQ29sb3IsIEJhbmQgQ29tYmluYXRpb24sIG9yIG1ha2UgdXAgeW91ciBvd24gdmlzdWFsCi8vIEluIGNhc2UgaXQgZG9lc24ndCBoYXZlIHRvIGxvb2sgYWVzdGhldGljYWxseSBwbGVhc2luZyB5b3UgY2FuIHB1dCBSRUQgYW5kIFlFTExPVyBoZXJlIAp2YXIgRklSRTEgPSBGaXJlMU9WTDsgLy8gT3V0ZXIgZmlyZSB6b25lLCBkZWYgPSBGaXJlMU9WTAp2YXIgRklSRTIgPSBGaXJlMk9WTDsgLy8gSW5uZXIgZmlyZSB6b25lLCBkZWYgPSBGaXJlMk9WTAovLyBJIHJlY29tbWVuZCBOYXR1cmFsTklSU1dJUk1peCBvciBOSVJTV0lSQ29sb3JzIGZvciBiZXN0IHZpc3VhbHMKLy8gVHJ5IE5hdHVyYWxDb2xvcnMsIEVuaGFuY2VkTmF0dXJhbENvbG9ycywgUGFuVGludGVkR3JlZW4sIGFuZCBQYW5CYW5kIGFzIHdlbGwKdmFyIE5PRklSRSA9IE5hdHVyYWxOSVJTV0lSTWl4OwovLyBVc2luZyBCMTIgYW5kIEIxMSB0byBoaWdobGlnaHQgcG9zc2libGUgZmlyZXMgaW4gdHdvIHpvbmVzIHRvIGdldCBzb21lIG1vcmUgZGlzdGluY3Rpb24KLy8gSW5jcmVhc2UgU0VOU0lUSVZJVFkgZm9yIG1vcmUgcG9zc2libGUgZmlyZXMgYW5kIG1vcmUgd3JvbmcgaW5kaWNhdGlvbnMKdmFyIFNFTlNJVElWSVRZID0gMS4wOwpyZXR1cm4gKEEoQjEyLCBCMTEpID4gKDEuMCAvIFNFTlNJVElWSVRZKSkKPyAoQShCMTIsIEIxMSkgPiAoMi4wIC8gU0VOU0lUSVZJVFkpKSA%2FIEZJUkUyIDogRklSRTEKOiBOT0ZJUkU7Cg%3D%3D) and [urban areas](https://apps.sentinel-hub.com/eo-browser/?lat=33.8610&lng=-6.9688&zoom=12&time=2019-08-15&preset=CUSTOM&datasource=Sentinel-2%20L2A&layers=B01,B02,B03&evalscript=dmFyIE5EV0k9aW5kZXgoQjAzLEIwOCk7IAp2YXIgTkRWST1pbmRleChCMDgsIEIwNCk7CnZhciBCYXJlU29pbD0yLjUgKigoQjExICsgQjA0KS0oQjA4ICsgQjAyKSkvKChCMTEgKyBCMDQpKyhCMDggKyBCMDIpKTsKIAppZiAoTkRXSSA%2BIDAuMikgewogcmV0dXJuIFswLCAwLjUsIDFdCn0KaWYoKEIxMT4wLjgpfHwoTkRWSTwwLjEpKXsKICByZXR1cm5bMSwxLDFdCn0KaWYgKE5EVkk%2BMC4yKXsKICByZXR1cm4gWzAsIDAuMypORFZJLCAwXQp9CmVsc2UgewogcmV0dXJuIFtCYXJlU29pbCwgMC4yLCAwXQp9) using [Sentinel Hub](https://www.sentinel-hub.com/). [Go](https://apps.sentinel-hub.com/sentinel-playground/?source=S2&lat=29.879454214599534&lng=-90.06499797105789&zoom=12&preset=CUSTOM&layers=B01,B02,B03&maxcc=99&gain=1.0&gamma=1.0&time=2019-08-01%7C2020-02-27&atmFilter=&showDates=false&evalscript=cmV0dXJuIFtCMDQqMy41LEIwMyozLjUsQjAyKjMuNV0%3D)… [wild](https://apps.sentinel-hub.com/eo-browser/?lat=29.8640&lng=-90.0697&zoom=10&time=2017-09-08&preset=CUSTOM&datasource=Landsat%208%20USGS&layers=B07,B03,B02&evalscript=cmV0dXJuIFtCMDcqMi41LEIwMyoyLjUsQjAyKjIuNV07).

Band combinations here are examples, not scientific law. I encourage you to experiment with your own combinations. If you need a starting point, [Harris Geospatial](https://www.harrisgeospatial.com/Learn/Blogs/Blog-Details/ArtMID/10198/ArticleID/15691/The-Many-Band-Combinations-of-Landsat-8) and [Esri](https://www.esri.com/arcgis-blog/products/product/imagery/band-combinations-for-landsat-8/) have both published fun little rundowns of Landsat 8 false color band combinations. [Here's](https://landsat.gsfc.nasa.gov/wp-content/uploads/2015/06/Landsat.v.Sentinel-2.png) a quick rundown of how Landsat 8 and Sentinel-2 bands compare. 

Basically, there's many ways to approach this. Pick your favorite. If your favorite is Bash, here we go:

## Composing Imagery 
#### Create an RGB image from raw Landsat 8 bands in a folder
Takes no input. File extension in this command may need to be altered (TIF v TIFF) depending on source. 👈 This is true of all of the Landsat 8 scripts here. 
```
function l8rgb() {
	prefix=${PWD##*/}
	gdal_merge.py -separate -co "PHOTOMETRIC=RGB" -of GTiff *\B4.TIF *\B3.TIF *\B2.TIF -o $prefix"_l8rgb.tif"
	open $prefix"_l8rgb.tif"	
}
```

#### Create a [pansharpened](https://en.wikipedia.org/wiki/Pansharpened_image) RGB image from raw Landsat 8 bands in a folder
```
function l8ps(){
	prefix=${PWD##*/}
	gdal_pansharpen.py  *\B8.TIF  *\B4.TIF  *\B3.TIF  *\B2.TIF $prefix"_l8_ps_rgb.tif" -r cubic -co PHOTOMETRIC=RGB
	open $prefix"_l8_ps_rgb.tif"
}
```

#### Create an RGB image from raw Sentinel-2 bands in a folder
Takes no input. Requires proper GDAL driver install (good luck!) 
```
function s2rgb() {
	prefix=${PWD##*/}
	gdal_merge.py -separate -co "PHOTOMETRIC=RGB" -of GTiff *\B04.jp2 *\B03.jp2 *\B02.jp2 -o $prefix"_s2rgb.tif"
	open $prefix"_s2rgb.tif"
}
```

#### Scale data values to min/max and convert to a byte image for toning
```
function image_min_max() {
	file_name="${1%%.*}"
	gdal_translate $1 -scale -ot byte -co COMPRESS=LZW $file_name"_stretched_to_min_max.tif"
	open $file_name"_stretched_to_min_max.tif"
}
```

#### Create a false color image using Landsat 8 bands in order to highlight active fires
```
function l8fire() {
	prefix=${PWD##*/}
	gdal_merge.py -separate -co "PHOTOMETRIC=RGB" -of GTiff *\B7.TIF *\B3.TIF *\B2.TIF -o $prefix"_l8_fire_false_color.tif"
	open $prefix"_l8_fire_false_color.tif"
}
```

#### Create a false color image using Sentinel-2 bands in order to highlight active fires
```
function s2fire() {
	prefix=${PWD##*/}
	gdal_merge.py -separate -co "PHOTOMETRIC=RGB" -of GTiff *\B12.jp2 *\B11.jp2 *\B05.jp2 -o $prefix"_s2_fire_false_color.tif"
	open $prefix"_s2_fire_false_color.tif"
}
```

#### Landsat 8 [NDVI](https://eos.com/ndvi/) Vegetation Mapping 
```
function l8ndvi() {
	prefix=${PWD##*/}
	gdal_calc.py -A *\B5.TIF -B *\B4.TIF --outfile=$prefix"_l8ndvi.tif" --calc="(A.astype(float)-B.astype(float))/(A.astype(float)+B.astype(float))" --NoDataValue=0 --type=Float32
	open $prefix"_l8ndvi.tif"
}
```

#### Sentinel-2 NDVI Vegetation Mapping 
```
function s2ndvi() {
	prefix=${PWD##*/}
	echo naming based on $prefix folder
	gdal_calc.py -A *\B08.jp2.tif -B *\B04.jp2.tif --outfile=$prefix"_s2ndvi.tif" --calc="(A.astype(float)-B.astype(float))/(A.astype(float)+B.astype(float))" --NoDataValue=0 --type=Float32
	open $prefix"_s2ndvi.tif"
}

```

#### Landsat 8 [NDWI](https://www.sentinel-hub.com/eoproducts/ndwi-normalized-difference-water-index) Water Mapping
```
function l8ndwi() {
	prefix=${PWD##*/}
	gdal_calc.py -A *\B3.TIF -B *\B5.TIF --outfile=$prefix"_l8ndwi.tif" --calc="(A.astype(float)-B.astype(float))/(A.astype(float)+B.astype(float))" --NoDataValue=0 --type=Float32
	open $prefix"_l8ndwi.tif"
}
```

#### Sentinel-2 NDWI Water Mapping
```
function s2ndwi() {
	prefix=${PWD##*/}
	gdal_calc.py -A *\B03.jp2 -B *\B08.jp2 --outfile=$prefix"_s2ndwi.tif" --calc="(A.astype(float)-B.astype(float))/(A.astype(float)+B.astype(float))" --NoDataValue=0 --type=Float32
	open $prefix"_s2ndwi.tif"
	
}
```



## Video/Motion
#### Take all images (in alphabetical order) with the same file extension and make a video at a specified frame rate.
```
function video_from_images(){
	frame_rate=$1
	file_extension=$2
	output=$3
	ffmpeg -r $frame_rate -f image2 -pattern_type glob -i '*.'"$file_extension" -acodec libmp3lame $output
}
```

#### Make any video (relatively) websafe. Helpful when a video seems broken (but isn't), or to maximize compatibility.
```
function websafe(){
	suffix=_websafe.mp4
	file_name="${1%%.*}"
	ffmpeg -an -i $1 -vcodec libx264 -pix_fmt yuv420p -profile:v baseline -level 3 -vf "pad=ceil(iw/2)*2:ceil(ih/2)*2" $file_name$suffix
	echo saved $file_name$suffix
}
```

#### Gif sequence of images with fade transition
```
function gif_fade(){
	file_extension=$1
	fade_length=$2
	output=$3
	convert *.$file_extension -morph $fade_length $output
}
```

#### Optimize gif
```
function optimize_gif(){
	input=$1
	output=$2
	gifsicle -b -O3 $input -o $output
}
```
