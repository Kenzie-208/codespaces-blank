In this project, my aim was to evaluate antifungal drug combinations for their effectiveness on 
Candida albicans (yeast) with a combination of image processing techniques from object detection and 
image intensity analysis. Effectiveness of the drug combination is evaluated by the growth of the yeast. 
The less yeast growth, the more effective the drug combination is. There were four experimental groups 
and one control group for comparison. 
Control (no drug) 
200 nM Colchicine 
500 nM Colchicine 
200 nM Colchicine + 2 µg Fluconazole 
500 nM Colchicine + 2 µg Fluconazole 
Each group had a picture taken of an agar plate with spot plating over a period of 48 hours in intervals of 
12 hours. Colchicine is a drug that inhibits tubulin polymerization of fungi such as yeast, preventing basic 
functions such as cell division and cell movement. Fluconazole is a drug that inhibits ergosterol synthesis 
in fungi such as yeast, preventing cell membrane formation and removing the protection that the cell 
membrane provides. These two drugs with distinct mechanisms of action can be used in combination to 
increase the efficacy of treatment for fungal infections. The question is, how effective will the combo 
treatment be at various concentrations when compared to colchicine alone? 
My approach was to follow the hints in order, starting with what I knew I’d need and working 
toward the parts I was less sure about. I began by preallocating the intensity arrays for each time point, 
then set up a for loop to pull in my image files. I started with the control set: stringing the filenames, 
reading them, and filtering noise using double, a Gaussian filter, and imfilter. I knew I’d eventually need 
to extract the red channel, so I extracted it early to avoid confusion once the image changed to grayscale 
for later code. One detail that took me a moment to catch on was that the background image (at time 0) 
also needed to be noise-filtered, smoothed, and reduced to its red channel before I used it to subtract the 
background from the image at i=1,2,3,4.  
My next decision was whether to draw boxes around each spot or use object detection to quantify 
the fluorescence of the whole image and divide by 16. I chose the latter since I didn't want to be clicking 
16×4×5 times. This was challenging because I haven't used object detection in this way, so I had to build 
it step by step: normalize, threshold, convert to grayscale, mask, run edge detection, fill the objects, and 
then isolate those filled regions to get an image containing only the cells. At this point, I had multiple 
debugging fixes, but one notable one was moving the line that removed negative intensities further down 
and not immediately after background subtraction. Once I had the array of red‑channel intensities, I 
summed them, divided by 16, and stored the value for each i. Then I calculated SEM for each time point 
and preallocated that array as well. I copied this code block for each condition with updated variable 
names.  
Outside the loop, I generated a line graph with time on the x‑axis and red intensity on the y‑axis. I 
added a legend, color-coding, and axis labels to make the figure presentable. When something went 
wrong, the graph made it obvious which code block was causing the issue. I also realized I needed to 
visually review the filtered and masked images, since mistakes there would throw off the intensities. I 
used the imshow function to check all time points for all conditions, but only kept the final image for each 
condition in the submitted code to keep things concise. Finally, I added an if statement and a subplot to 
make it easier to see the filtered images, as imshow will overwrite itself. 
