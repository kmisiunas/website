---
title: "Tips on plotting"
date: "2018-10-14"
draft: false
toc: true
typora-root-url: ../../../static
#type: posts
---

Producing clear figures is one of the key skill set.

**DRAFT: please be considerate and help improve it**



# Clarity 



## Missing data

Missing data should be treated carefully and always focus on the message behind the visualisation [1]. Common methods:

1. Show the gaps
2. Treat it as a category 
3. Interpolate

## Rules of thumb

- **Never use piecharts** because they are [difficult to interpret](https://www.businessinsider.com/pie-charts-are-the-worst-2013-6?IR=T). We are simply not that great at finding the biggest piece of the pie :disappointed::cake:



---

# Styling

**Frame** with ticks on all edges helps to read out the values because it easy to draw horizontal lines 

## Axes labels

**Naming**



**Units** should be always present and unambiguous. SI unit convention suggests using a divider [2], as in $k_B T / J$  or $v / ms^{-1}$. But I find this notation a little confusing since the unit could be interpreted as a symbol  ($J$ is also a current density in physics). Therefore, I prefer using brackets to indicate units as suggested by [APS guidelines](https://journals.aps.org/authors/axis-labels-and-scales-on-graphs-h18) [3]. For example:

{{< figure src="/images/2018/plotting_axis_units.svg" 
caption="(Left) Units are shown in the brackets. (Right) the brackets are omitted for unit-less quantities." >}}

**Separator** between description and mathematical symbol introduces clarity. Lately, I use comma followed by few spaces. This visually separates the two while avoiding ambiguity introduces by other symbols. For example, a dash separator could be interpreted as a minus sign, thus causing confusion. 

## Fonts

**Typeface** should be easy to read on the screen and in printed media. My personal choice is sans-serif fonts, such as Helvetica or Myriad Pro.

**Font size** is sometimes a tricky matter because figure often is rescaled after placing it on a website, slides, or an article. As rule of thumb, the font *must* be large enough to see without straining your eyes. In a presentation, people in the back row must be able to read all the labels. In a paper, I normally go for size 7pt for small panels and size 9pt for large panels. 



## Ticks



## Colors

Adding a vivid color can guide the reader on specific features, but it also can also introduce biases during interpretation. This is highly undesirable and thus should be used with care. In addition, a small fraction of the people are [colourblind](https://en.wikipedia.org/wiki/Color_blindness), and are unable to tell some colors apart. For these two reasons, I often use 



| Color | R, G, B          | HEX  |
| ----- | ---------------- | ---- |
|       | 0.35, 0.43, 0.99 |      |
|       |                  |      |
|       |                  |      |

### Color gradients

[8]



---

# Software

The best tool for producing plots should be easily integrated with your analysis pipeline. I will share some notes about the software that I use and what I liked about it.

## Wolfram Mathematica 



## Python matplotlib.pyplot

Python has many plotting libraries ([overview](https://speakerdeck.com/jakevdp/pythons-visualization-landscape-pycon-2017)), but I normally stick with matplotlib because it supports raster image output, vector graphics (PDF or SVG), and detailed customisation. Unfortunately, the default looks of matplotlib plots is not great, but it can easily be customised to look amazing. 

Additional recourses:

- [Seaborn](http://seaborn.pydata.org/) - styling extension with few useful plotting extension 
- [Calendar heatmap library](http://pythonhosted.org/calmap/)
- [Effective matplotlib](http://pbpython.com/effective-matplotlib.html) article
- [Anatomy Of Matplotlib](http://nbviewer.jupyter.org/github/WeatherGod/AnatomyOfMatplotlib/blob/master/AnatomyOfMatplotlib-Part1-Figures_Subplots_and_layouts.ipynb) article

## Illustrator / Inkscape 

For the most important figures, that go into articles or presentations, I prefer composing the figure and adding annotations in a vector graphics editor. This allows the precise control over the font sizes, positions, and other annotations can be placed with an ease. For example, this is a figure from my paper where teal color indicates items added after plotting:

{{< figure src="/images/2018/vector_software_example.svg" width="70%"
caption="Example panel, where everything in teal was added using vector graphics editor." >}}

Notice that I keep the data panels untouched and only add annotations on top. This avoids accidental distortion of the data. I export the finished figure using PDF or SVG formats (with sRGB color space). 

---

# Acknowledgements 



---

# References 

1. [“Visualizing Incomplete and Missing Data”](https://flowingdata.com/2018/01/30/visualizing-incomplete-and-missing-data/) by FlowingData
2. [Boer, J. de. (1995). On the History of Quantity Calculus and the International System. *Metrologia*, *31*(6), 405–429.](https://doi.org/10.1088/0026-1394/31/6/001)
3. <https://journals.aps.org/authors/axis-labels-and-scales-on-graphs-h18>
4. [Nature blog: Heuristics for better figures](http://blogs.nature.com/onyourwavelength/2018/07/18/heuristics-for-better-figures/) (todo)
5. ["Your Friendly Guide to Colors in Data Visualisation”](https://blog.datawrapper.de/colorguide/) ([HN](https://news.ycombinator.com/item?id=17660640)[ comments](https://news.ycombinator.com/item?id=17660640)) (todo)
6. [“Why should engineers and scientists be worried about color”](https://github.com/frankMilde/interesting-reads/blob/master/ibm-research__why-should-engineers-and-scientists-be-worried-about-color.pdf) by IBM research (todo)
7. “[Points of view: Color coding](https://www.nature.com/articles/nmeth0810-573)” in Nature Methods 
8. [“Points of view: Mapping quantitative data to color”](https://www.nature.com/articles/nmeth.2134) in Nature Methods
9. “[GUIDE TO PREPARING FINAL ARTWORK](https://www.nature.com/documents/NRJs-guide-to-preparing-final-artwork.pdf)” by Nature 