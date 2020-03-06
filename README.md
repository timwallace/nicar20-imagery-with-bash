# nicar20-imagery-with-bash
[Great reference](https://natelandau.com/my-mac-osx-bash_profile/) on bash profiles.

Dependencies: [FFmpeg](https://www.ffmpeg.org/), [GDAL](https://gdal.org/), [Gifsicle](https://www.lcdf.org/gifsicle/), [ImageMagick](https://imagemagick.org/index.php)

If [wrangling imagery in the terminal](https://medium.com/planet-stories/a-gentle-introduction-to-gdal-part-1-a3253eb96082) is not your cup of tea 🍵 and you're lucky enough to have a Photoshop license, have no fear. The concepts outlined by [Tom Pattersosn](https://twitter.com/MtnMapper) for Landsat 8 [here](http://www.shadedrelief.com/landsat8/) can be easily applied to other sensors.  

## Composing Imagery 
#### Create an RGB image from raw Landsat 8 bands in a folder
Takes no input. File extension in this command may need to be altered (TIF v TIFF) depending on source.
```
function l8rgb() {
  prefix=${PWD##*/}
  gdal_merge.py -separate -co "PHOTOMETRIC=RGB" -of GTiff *\B4.TIF *\B3.TIF *\B2.TIF -o $prefix_l8rgb.tif
  open $prefix_l8rgb.tif	
}
```

#### Create an RGB image from raw Sentinel-2 bands in a folder
Takes no input. Requires proper GDAL driver install (good luck!) 
```
function s2rgb() {
  prefix=${PWD##*/}
  gdal_merge.py -separate -co "PHOTOMETRIC=RGB" -of GTiff *\B04.jp2 *\B03.jp2 *\B02.jp2 -o $prefix_s2rgb.tif
  open $prefix_s2rgb.tif

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
