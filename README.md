# Lecture 9 - CYBR 2980

## Image processing and manipulation
To edit images in python, we will need to install a third party module called Pillow. Pillow is a tool that will allow us to access pixel-level data of common image file types.

[Pillow Documentation](https://pillow.readthedocs.io/en/latest/handbook/index.html)
### Installing Pillow
At a command prompt, you should type the following commands:
```
python3 -m pip install --upgrade pip
python3 -m pip install --upgrade Pillow
```
If there are issues, try to trouble shoot.
- Is pip installed?
- Does it work if I type python rather than python3?
- Is the current verison of python the default one?
```
python --version
```
### Opening an Image
Pillow can open most image filetypes, for our early projects, we will stick mainly to Portable Network Graphics (.png) files as they allow for transparency.

The next trick is being able to access the folder containing your image. In your python terminal:
```
>>> import os
>>> os.getcwd()
```
That will tell you where your python is currently looking for files.
If you are not in the folder with the file you wish to open, you can change that with the following python commands.
```
>>> os.chdir('C:\\folder_with_image_file')
```
Now that we are working in the same folder as our image, we can open an image using pillow.
```
>>> from PIL import Image
>>> myImg = Image.open('durango.png')
>>> myImg.size
>>> myImg.filename
>>> myImg.format
>>> myImg.format_description
>>> myImg.show()
```
Python will open the image in your default image application. In my case, it is MS Paint.
### Creating a new image
To create a new image using Pillow, we need to give a few arguments.
Mode: 'rgba' - red, green, blue, alpha for pixel data.
Size: A 2-tuple containing (width, height) in pixels
Color: Background color for the new image, default is black.
```
>>> from PIL import Image
>>> im = Image.new('RGBA', (200, 300), 'red')
>>> im.show()
>>> im.save('redRectangle.png')
```
We have just created a new image with a red background.
Image formats
### Filter functions
To change an image, we need to visit every pixel and make some decision about how to change the image.

We will create all of our filter functions in **ImageFilters.py**
#### Black & White filter
This function will take in an image as a parameter. The function will then touch each pixel, looking at the RGB values. Finally, it will set each value (R,G,B) to the average of the RGB. The result is a grayscale version of the original.

A few things to note in this function.
```
pixels = img.load()
```
This turns the image object into a 2-dimensional list, or a list of lists. We can now go to each pixel like it is an XY grid. This is done with nested loops, the first for the row, the second for the column.
```
width, height = img.size
for x in range(width):
  for y in range(height):
    <operations on the pixels happen here>
```
Another new thing is the width, height assignment. This works because img.size is a 2-tuple value and python will allow assignment to be split.
Finally, this image can be displayed or saved to the drive.
```
img.show()
img.save("bwImg.png", 'png')
```
#### Color Swap (Green-Blue)
This function will swap the green and blue values of every pixel in the image.
Try to show the image in the end, perhaps try to swap different values to see how color channels affect the overall image.

#### Darken
This function should take an image, and darken the RGB values of every pixel in that image by a set amount. Darken in the case of color means to reduce the value (subtraction). When subtracting, you should make sure that you don't go beyond zero.
A useful tool is the max() function which takes multiple arguments, and returns the largest. This reduces the need for if statements in your code.
```
>>> max(10, 15)
15
>>>
```
#### Built-in Filter Functions
The Pillow module offers a number of built-in filters. To access these you will have to import the ImageFilter package from PIL.
```
from PIL import ImageFilter
im1 = myImg.filter(ImageFilter.BLUR)
im1.show()
```
Read about the other available filters on the [Pillow Documentation site](https://pillow.readthedocs.io/en/latest/reference/ImageFilter.html)

### Quiz
1. How do you open an image using Pillow?
- myImg = Image.open('durango.png')
- myImg = Image.open('durango', 'png')
- myImg = open('durango.png')
- myImg = Pillow.open('durango.png')

2. If I wanted to create a vertical line of red pixels
```
pixels = img.load() #Pixels is the pixel map, a 2-dimensional list of pixel data
width, height = img.size
```
Which code would accomplish that task?
-  ```
  for x in range(width):
    for y in range(height):
      pixels[x,y] = (255, 0, 0, 255)
  ```

-  ```
  for x in range(width):
    pixels[x,20] = (255, 0, 0, 255)
  ```
-  ```
  for y in range(height):
    pixels[20,y] = (255, 0, 0, 255)
  ```
-  ```
  for x in range(width):
    for y in range(height):
      pixels[20, 20] = (255, 0, 0, 255)
  ```

3. If you were adding a fixed amount to a color, which of the following would ensure you didn't go beyond the color range?
- r = min(255, red + amount)
- r = max(255, red + amount)
- r = abs(red - amount)
- r = abs(amount - red)

4. Based on the documentation for the built-in filter functions, which is NOT a built-in filter?
- BLUR
- EDGE_ENHANCE
- SHARPEN
- ZOOM

5. Using an RGBA format with 256-bits per channel, how many bytes would a 100x100 pixel image take. Remember that 1 byte = 8 bits.
