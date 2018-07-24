# Meta Find Me
## level 2 - forensics - 70 points

### Description
> Find the location of the flag in the image: [image.jpg](./data/image.jpg). Note: Latitude and longitude values are in degrees with no degree symbols,/direction letters, minutes, seconds, or periods. They should only be digits. The flag is not just a set of coordinates - if you think that, keep looking!

### Hints
> * How can images store location data? Perhaps search for GPS info on photos.

### Solution

The flag and the location are hidden into photo [EXIF](https://it.wikipedia.org/wiki/Exchangeable_image_file_format) data. I used `identify` command from `imagemagick` to retrieve them:

```
$ identify -verbose image.jpg
Image: image.jpg
  Format: JPEG (Joint Photographic Experts Group JFIF format)
  Mime type: image/jpeg
  Class: DirectClass
  Geometry: 500x500+0+0
  Resolution: 72x72
  Print size: 6.94444x6.94444
  Units: PixelsPerInch
  Colorspace: sRGB
  Type: TrueColor
  Base type: Undefined
  Endianess: Undefined
  Depth: 8-bit
  Channel depth:
    Red: 8-bit
    Green: 8-bit
    Blue: 8-bit
  Channel statistics:
    Pixels: 250000
    Red:
      min: 0  (0)
      max: 255 (1)
      mean: 236.347 (0.92685)
      standard deviation: 40.4046 (0.158449)
      kurtosis: 27.6645
      skewness: -5.39004
      entropy: 0.113592
    Green:
      min: 0  (0)
      max: 255 (1)
      mean: 238.346 (0.934689)
      standard deviation: 40.8612 (0.16024)
      kurtosis: 27.7488
      skewness: -5.39886
      entropy: 0.110906
    Blue:
      min: 0  (0)
      max: 193 (0.756863)
      mean: 142.497 (0.558812)
      standard deviation: 24.3041 (0.0953103)
      kurtosis: 27.4199
      skewness: -5.3627
      entropy: 0.125959
  Image statistics:
    Overall:
      min: 0  (0)
      max: 255 (1)
      mean: 205.73 (0.806784)
      standard deviation: 35.19 (0.138)
      kurtosis: 1.84188
      skewness: -1.38897
      entropy: 0.116819
  Rendering intent: Perceptual
  Gamma: 0.454545
  Chromaticity:
    red primary: (0.64,0.33)
    green primary: (0.3,0.6)
    blue primary: (0.15,0.06)
    white point: (0.3127,0.329)
  Matte color: grey74
  Background color: white
  Border color: srgb(223,223,223)
  Transparent color: none
  Interlace: JPEG
  Intensity: Undefined
  Compose: Over
  Page geometry: 500x500+0+0
  Dispose: Undefined
  Iterations: 0
  Compression: JPEG
  Quality: 93
  Orientation: TopLeft
  Properties:
    comment: "Your flag is flag_2_meta_4_me_<lat>_<lon>_23c1. Now find the GPS coordinates of this image! (Degrees only please)"
    date:create: 2018-07-20T19:24:22+02:00
    date:modify: 2018-07-20T19:23:45+02:00
    exif:ColorSpace: 1
    exif:DateTime: 2016:02:10 11:55:56
    exif:ExifImageLength: 500
    exif:ExifImageWidth: 500
    exif:ExifOffset: 176
    exif:GPSInfo: 218
    exif:GPSLatitude: 11/1, 0/1, 0/1
    exif:GPSLongitude: 31/1, 0/1, 0/1
    exif:GPSVersionID: 2, 3, 0, 0
    exif:Orientation: 1
    exif:ResolutionUnit: 2
    exif:Software: Adobe Photoshop CS6 (Windows)
    exif:thumbnail:Compression: 6
    exif:thumbnail:JPEGInterchangeFormat: 402
    exif:thumbnail:JPEGInterchangeFormatLength: 6989
    exif:thumbnail:ResolutionUnit: 2
    exif:thumbnail:XResolution: 72/1
    exif:thumbnail:YResolution: 72/1
    exif:XResolution: 720000/10000
    exif:YResolution: 720000/10000
    jpeg:colorspace: 2
    jpeg:sampling-factor: 1x1,1x1,1x1
    signature: b6b522b21c9803c5b96da49ab524bae6ef91c8179d78fac1a0289978e480f5bd
  Profiles:
    Profile-8bim: 9330 bytes
    Profile-exif: 7398 bytes
    Profile-icc: 3144 bytes
    Profile-xmp: 3511 bytes
  Artifacts:
    verbose: true
  Tainted: False
  Filesize: 44265B
  Number pixels: 250000
  Pixels per second: 12.5MB
  User time: 0.010u
  Elapsed time: 0:01.019
  Version: ImageMagick 7.0.8-6 Q16 x86_64 2018-07-09 https://www.imagemagick.org
```

So the flag is `flag_2_meta_4_me_<lat>_<lon>_23c1`, we have to check the properties `exif:GPSLatitude` and `exif:GPSLongitude`, replace `<lat>` and `<lon>` with the proper numbers (without `/1`) and submit.

You can solve it with a lot of tools, an easier way to get the informations is "Get Info" command while right-clicking the image on MacOS. Windows has a similar procedure too.

### Flag
```
flag_2_meta_4_me_11_31_23c1
```