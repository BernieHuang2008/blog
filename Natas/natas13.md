# Analyze
As we examined the code, a PHP function named 'exif_imagetype' involved. 

If we search for this function, and look through the [PHP Manual](https://www.php.net/manual/en/function.exif-imagetype.php), we will see the following description:
> exif_imagetype() reads the first bytes of an image and checks its signature.

So, in order to pass the check, we shall add some 'magic numbers' of PNG/JPG before the actual PHP Script.

In this case, it's `89 50 4E 47 (·PNG)` for PNG file.

However, I chose to insert my code into the bunch of PNG datas, so it will also success even Natas checks my last bytes.

# Solution
1. Write Those bytes to a file:

![image](https://github.com/BernieHuang2008/blog/assets/88757735/1ad31424-26e0-43b2-b4b3-afb6198c4ac4)

2. and then reset the 'filename' in '<form>' to '.php' file, upload.

The highlighted part in the image will be executed as PHP Script, while the un-highlighted, original PNG data will be printed as they are.

As the result, the result might display like this:
![image](https://github.com/BernieHuang2008/blog/assets/88757735/479563a2-d6d1-45f0-a4da-ce351d53c4a4)

And the content after 'IHDR' (which is highlighted), will be the flag.
