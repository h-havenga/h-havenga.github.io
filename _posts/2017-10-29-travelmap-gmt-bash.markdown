---
layout: post
title:  "How I made the travel maps in this site"
date:   2017-11-22 14:43:28 +0200
categories: gmt map travel
---

This is a very hacked script I used to create my travel map

{% highlight bash %}
#!/bin/bash
#Use ISO2 country codes
visited="ZA,BW,NA,GB,ZM,MW,NL,CH,FR,FI,DE,BE,LU,CZ,US,RE,US,AT,LS,"
usa="US.NC,US.SC,US.NY,US.VA,US.WY,US.MO,US.SD,US.TN,US.WV,US.KS,US.CO,US.MA,US.GA,US.MI,US.AZ,US.KY,US.NJ,US.PA,US.TX,US.DC,US.OR,US.ID,US.MS,US.CA,US.NE,US.NM,US.AL,US.IL,US.AL,US.LA,US.IA,US.MD"

# World
gmt pscoast -Rd -JN0/27.5 -Xc -Yc -Bg30/g15 -Dh -E$visited+p0.25p,black+g100/255/100 -N1/0.25p,black -A1000 -Glightgray > travelmap_world.ps
convert -density 300 travelmap_world.ps -rotate 90 travelmap_world.png 
mogrify -format png -gravity south -chop 0x300 -gravity north -chop 0x300 travelmap_world.png

# Africa
gmt pscoast -R-20/55/-38/40 -JM17 -P -Xc -Yc -Ba60f30/a30f15 -Dh -E$visited+p0.25p,black+g100/255/100 -A100 -N1/0.5p,black -Glightgray > travelmap_africa.ps
convert -density 300 travelmap_africa.ps travelmap_africa.png 
mogrify -format png -gravity south -chop 0x500 -gravity north -chop 0x500 travelmap_africa.png

# Europe
gmt pscoast -R-20/50/35/72 -JL15/35/35/55/25c -Xc -Yc -B10a0 -Dh -E$visited+p0.25p,black+g100/255/100 -Glightgrey -N1/0.5p,black -A100 > travelmap_europe.ps
convert -density 300 travelmap_europe.ps -rotate 90 travelmap_europe.png 

# North America
gmt pscoast -R-130/-70/24/52 -JL-100/35/33/45/25c -Xc -Yc -B10a0 -Dh -E$usa+p0.25p,black+g100/255/100 -N1/0.5p,black -Glightgrey > travelmap_na.ps
convert -density 300 travelmap_na.ps -rotate 90 travelmap_na.png 
mogrify -format png -gravity south -chop 0x300 -gravity north -chop 0x300 travelmap_na.png
{% endhighlight %}

# Update
This is much more effcient, in the previous code I drew the maps individually and merged them in inkscape, here I draw it all in GMT.

{% highlight bash %}
#!/bin/bash
# Use ISO2 country codes
visited="ZA,BW,NA,GB,ZM,MW,NL,CH,FR,FI,DE,BE,LU,CZ,US,RE,US,AT,ES"
usa="US.NC,US.SC,US.NY,US.VA,US.WY,US.MO,US.SD,US.TN,US.WV,US.KS,US.CO,US.MA,US.GA,US.MI,US.AZ,US.KY,US.NJ,US.PA,US.TX,US.DC,US.OR,US.ID,US.MS,US.CA,US.NE,US.NM,US.AL,US.IL,US.AL,US.LA,US.IA,US.MD"

# World
gmt pscoast -Rd -JN0/24 -Xc -Y33 -Bg30/g15 -Dh -E$visited+p0.25p,black+g100/255/100 -N1/0.25p,black -A1000 -Glightgray --PS_MEDIA=45.5cx30c -K > travelmap_world.ps

# Africa
gmt pscoast -R-20/55/-38/40 -JM12 -X-1.5 -Y-15 -Ba30f30/a30f15 -Dh -E$visited+p0.25p,black+g100/255/100 -A100 -N1/0.5p,black -Glightgray -O -K >> travelmap_world.ps

# Europe
gmt pscoast -R-20/50/35/72 -JL15/35/35/55/13c -X14 -Y1.5 -B10a0 -Dh -E$visited+p0.25p,black+g100/255/100 -Glightgrey -N1/0.5p,black -A100 -O -K >> travelmap_world.ps

# North America
gmt pscoast -R-130/-70/24/52 -JL-100/35/33/45/24c -X-12 -Y-17 -B10a0 -Dh -E$usa+p0.25p,black+g100/255/100 -N1/0.5p,black -Glightgrey -O -K >> travelmap_world.ps
{% endhighlight %}
