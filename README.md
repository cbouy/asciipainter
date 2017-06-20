# asciipainter
Transforms an image into an ASCII style colored text on your terminal.

# Requirements
* Python 3
* numpy library
* PIL library (aka Pillow)
* For xterm-256 colors : almost any terminal
* For RGB colors : Terminals supporting TrueColour

See https://gist.github.com/XVilka/8346728 for more info on terminals.

# Usage
`./asciinator.py image scale factor color_sat mode`
* image : path to jpg image
* scale : resize the image to scale*original_size (1 for default)
* factor : intensity correction for ASCII assignment (1 for default)
* color_sat : color-intensity correction (1 for default)
* mode : RGB if your terminal supports it, else 256 color (256/RGB)

# Quick terminal test
For xterm-256 colors :
```bash
for fgbg in 38 48; do 
  for color in {0..256} ; do 
    echo -en "\e[${fgbg};5;${color}m ${color}\t\e[0m"
    if [ $((($color + 1) % 10)) == 0 ] ; then 
      echo
    fi
  done
  echo
done
```
Should output a rainbow looking color table

For True Colour :
```bash
awk 'BEGIN{
  s="/\\/\\/\\/\\/\\"; s=s s s s s s s s;
  for (colnum = 0; colnum<77; colnum++) {
    r = 255-(colnum*255/76);
    g = (colnum*510/76);
    b = (colnum*255/76);
    if (g>255) g = 510-g;
    printf "\033[48;2;%d;%d;%dm", r,g,b;
    printf "\033[38;2;%d;%d;%dm", 255-r,255-g,255-b;
    printf "%s\033[0m", substr(s,colnum+1,1);
  }
  printf "\n";
  }'
```
Should output a line of "/\" with colors going from red to blue in the foreground and background.
If most of the background colors are skipped, your terminal doesn't support True Colour.
