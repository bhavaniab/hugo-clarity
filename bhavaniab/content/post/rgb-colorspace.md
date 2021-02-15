+++
author = "Bhavani Anantapur Bache"
title = "Representation of images in the Digital world -The RGB Colorspace"
date = "2015-07-15"
description = "10 most frequently used commands in Linux for beginners"
featured = true
tags = [
    "DSP",
    "Embedded Systems",
    "Technology",
]
categories = [
    "DSP",
    "Embedded Systems",
]
series = ["DSP"]
aliases = ["migrate-from-jekyl"]
thumbnail = "images/rgbcolor.png"
+++

Visible spectrum is the portion of electromagnetic spectrum that is visible to the human eye. Electromagnetic spectrum in this range of radiation is called visible light. Colors containing only one wavelength are called as pure colors. Pure colors are red, green and blue. Colors such as yellow and magenta can be made by a mix of multiple wavelengths. Combination of all the three colors, red green and blue constitute white.


![alt text](images/rgbcolorspace1.png "Logo Title Text 1")

<h2>Representation of Images in RGB colorspace. </h2>
An image is divided into small homogeneous elements called as pixels. A color of a pixel in the RGB color model is represented by indicating how much of Red Green and blue is included. The color is represented by RGB triplet. Each component can vary from 0 to a maximum value(which is usually 255). If Red, Green and blue components are zero, the color is black. If Red, Green and blue components are maximum, the color is white. If Red and Green are maximum and blue is zero, the color is yellow.

<h2>RGB32 color format</h2>
In RGB32 color format, 8 bits are allocated for representing each color, that is, 8 bits for Red, 8 for Green and 8 for blue. The last 8 bits are usually discarded. Sometimes, the last 8 bits are used to store alpha, which represents the transparency of an image. The transparency plays an important role while blending of two images. This technique is used to represent transparent logos on the original images. In the picture below, the yellow apple is a transparent image, which is superimposed on the flower, which is the original image. (Techniques of alpha blending will be discussed in detail in the next blog). 32 bit representation is used to represent 16,777,216 colors.
