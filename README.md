# nicar20-imagery-with-bash
[Great reference](https://natelandau.com/my-mac-osx-bash_profile/) on bash profiles.

Dependencies: [FFmpeg](https://www.ffmpeg.org/), [GDAL](https://gdal.org/), [Gifsicle](https://www.lcdf.org/gifsicle/), [ImageMagick](https://imagemagick.org/index.php)


## Video/Motion
### Take all images (in alphabetical order) with the same file extension and make a video at a specified frame rate.
```
function video_from_images(){
  frame_rate=$1
  file_extension=$2
  output=$3
  ffmpeg -r $frame_rate -f image2 -pattern_type glob -i '*.$file_extension' -acodec libmp3lame $output
}
```

### Make any video websafe. Helpful when a video seems broken (but isn't), or to maximize compatibility.
```
function websafe(){
  suffix=_websafe.mp4
  file_name="${1%%.*}"
  ffmpeg -an -i $1 -vcodec libx264 -pix_fmt yuv420p -profile:v baseline -level 3 -vf "pad=ceil(iw/2)*2:ceil(ih/2)*2" $file_name$suffix
  echo saved $file_name$suffix
}
```

### Gif sequence of images with fade transition
```
function gif_fade(){
  file_extension=$1
  fade_length=$2
  output=$3
  convert *.$file_extension -morph $fade_length $output
}
```

### Optimize gif
```
function optimize_gif(){
  input=$1
  output=$2
  gifsicle -b -O3 $input -o $output
}
```
