Documentation Author: Niko Procopi 2020

This tutorial was designed for Visual Studio 2019
If the solution does not compile, retarget the solution
to a different version of the Windows SDK. If you do not
have any version of the Windows SDK, it can be installed
from the Visual Studio Installer Tool

Welcome to the BlurVR Tutorial!
Prerequesites: Time Stamps, Render To Target, Gaussian Blur

In this tutorial, the screen is clear in the center of each eye, and
gradually gets blurrier as the pixels get farther from the center
of each eye. This draws the focus of eyes in VR headsets to 
the center.

In main.cpp I changed moveX from -0.5 to -0.2, because it reduces
double-vision when the camera is close to objects

Rendering to target and applying post processing in C++ is 
exactly the same in this tutorial as it was in previous tutorials,
so that should already be understood

In 2ndFrag GLSL, the first calculations the shader does is find
the distance of the pixel from the center of the left eye, and the 
center of the right eye. This is hard-coded to the window configuration
that works on my headset. If anyone adjusted where the screens are 
drawn to center each eye on a different headset, this will need to be
modified.

Next step is to get the smallest distance, which basically determines
if the pixel is in the left eye, or the right eye

With the distance from the center of the eye, we calculate the intensity
of the blur. You dont need to use the exact formula I used
	distance * distance * 50
You can use any formula that feels satisfying, as long as pixels
are clear in the center and blurry on the edges

In previous tutorials, you could increase blur intensity by changing
how many samples were used to blur the pixels, but that is not what
we do here. 

To blur a pixel, we add the colors of neighboring pixels. As blur intensity
increases, it grabs neighboring pixels that are non-constant distance from
the pixel being blurred. This is what makes the blurring change

Also, a mistake in the previous tutorial that was fixed here, when looping
through pixels

	for(int i = 0; i < samples; i ++)
	{
		for(int j = 0; j < samples; j++)
		{

When calculating sampleX and sampleY, you need to subtract
half of the total samples. That way you grab pixels from left, right, up, and down.
Otherwise, you only grab pixels to the right, and above

How to Improve:
Do blurring in two separate renderpasses, one for horizontal blur, one for vertical blur,
	and use time stamps to prove that performance boosts (I guarantee it will)