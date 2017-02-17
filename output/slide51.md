
* As expected, we pulled down `python:latest`.  It's a very large image, 694.3 MBs.  That's fine for playing around, but for an actual container deployment, that's laughably large.  Let's pull down a couple of other python images that are better for actual usage.  
	* **:slim** is a tag on python that provides a "skinnied" down version of the image
	* **:alpine** is an image based on alpine linux.  A specific distribution designed for compactness for use in containers.

