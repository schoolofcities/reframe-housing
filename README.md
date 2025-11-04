Objective

- Create an interactive map to fit within this soon-to-be-launched website: https://reframehousing.org/
- Map is of the world, showing country borders, where a small set of the countries can be clicked on to show some text about their housing policy. See table here for text: https://docs.google.com/document/d/1BE2dEhezWdwQM-L189ljEs1p9HQf7BKdx-LPcr1hTbQ/edit?tab=t.0
- Map + text will need to be viewable across all screen sizes
- We can have this in our own GitHub repo and hosted on GitHub pages, and then it'll just be iframed into the site 
- Max view width 1180px wide, min view width ~360px wide.

Recommendations

- We don't need a fancy basemap or anything, just simplified country polygons and maybe graticules, so D3 with simplified natural earth data would the easiest I think: https://www.naturalearthdata.com/downloads/110m-cultural-vectors/
- Pick a fun map projection that shows the whole world fairly (i.e. no Mercator :) see here for some examples https://d3js.org/d3-geo/cylindrical 
- Not sure if better to have a pop-up for when click or just a side-by-side map and text, leaning towards the latter since it could wrap on mobile
- Pick colours that are same as website palette (green and yellow palette)

Timing/deadline

- Have a working draft finished by Nov 14, overall website to be launched week of Nov 24th
