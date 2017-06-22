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
`./asciipainter.py [-h] -i [INPUT] [-m [{256,RGB}]] [-a [FLOAT]] [-c [FLOAT]] [--auto [{h,w}]] [-s [FLOAT]] [-p [FLOAT]]`
* -h, --help : Show the help message and exit.
* -i [INPUT], --input [INPUT] : Path to the image in jpg format.
* -m [{256,RGB}], --mode [{256,RGB}] : Color palette used for the output : RGB or 256 colors.If your terminal supports TrueColour, go for RGB.
* -a [FLOAT], --ascii [FLOAT] : ASCII level correction factor. Changes the assignment of a pixel to a higher or lower level character.
* -c [FLOAT], --color [FLOAT] : Color saturation correction factor. Changes the intensity of colors on the output.
* --auto [{h,w}] : Automatically scale the output based on height (h) or width (w) of your terminal. Default : h
* -s [FLOAT], --scale [FLOAT] : Rescale the output by this correction factor.
* -p [FLOAT], --pixel [FLOAT] : Correction factor to account for the difference in height/width of a character compared to a pixel. Default : 1.9
Only the input image is mandatory for the script to work.

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

# Example
Original (Photo by Vlad Savov / The Verge)
![alt text](https://raw.githubusercontent.com/cbouy/asciipainter/master/example/car.jpg "Original image")
Output with default `factor` and `color_sat`, in xterm-256 colors
![alt text](https://raw.githubusercontent.com/cbouy/asciipainter/master/example/Capture.PNG "Default terminal output")
Output with `1.1 factor` and `1.5 color_sat`, in xterm-256 colors
![alt text](https://raw.githubusercontent.com/cbouy/asciipainter/master/example/Capture2.PNG "Terminal output with small adjustments")
