## Imagemagick, Convert to Progressive JPEG

#### entire directory
`mogrify -interlace plane *.jpg`

#### single file
`convert inputfile.jpg -interlace plane outputfile.jpg`

#### Progressive JPEG will state, Interlace: JPEG
`identify -verbose file.jpg`

## Convert .pdf -> .jpg with ImageMagick

```bash
# %d will iterate using 1 digit, %3d 3 digits
convert -density 150 art.pdf -quality 90 %d.jpg
``` 

##  Stretches the shortest side to match the longest side so that the image becomes square
```bash
ls * | xargs -L 1 -I {} magick {} -gravity center -background white -extent "%[fx:max(w,h)]x%[fx:max(w,h)]" sq-{}
```

