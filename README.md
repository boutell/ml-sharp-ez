This package is intended to democratize participation in 2d-3d image conversion research by easing the use of Apple's ml-sharp model, which converts still images into 3d splat files in `.ply` format. It's pretty cool, but keep in mind you [must still abide by Apple's license for the model](https://github.com/apple/ml-sharp/blob/main/LICENSE_MODEL).

# Before you install

Make sure you have Python available as either `python3` or `python`, and that it is at 3.x version.

# Installing

Run:

```bash
# cd into this repo you just checked out
cd ml-sharp-ez 
./install
```

If there are any errors you will need to resolve them before proceeding.

# Generating `.ply` 3d splat files from your JPEGs

First, **copy at least one JPEG to start** into the `./input` folder in this repo. I suggest trying just one until you are sure this works.

Now convert them (you should `cd` back into `ml-sharp-ez` first if you are not there anymore):

```bash
./convert
```

After this command `./output` will contain:

* Full-size `.ply` files for each input file.
* Reduced-size `.ply` files for each input file, for use with the Quest 3 headset.

# Building a website to view your splat files with WebXR

```bash
./generate-website
```

This will create a `./website` folder in `ml-sharp-ez`.

As a sanity check, you can serve that `./website` folder locally:

```bash
cd ./website
# yours might be just "python"
python3 -m http.server
```

Then visit the resulting test site at:

http://localhost:8000

However note you probably will not have a usable "Enter VR" button this way.

For that, you will need to deploy the `./website` folder to the static hosting of your choice, which must support HTTPS, otherwise the "Enter VR" buttons will not work. But just about everything comes with HTTPS support these days.

Then you'll be able to open it from your Quest 3 or other headset.

Yes, you can deploy it as a subdirectory of another site if you want to.

**The website is built with the reduced-size `.ply` files** to prevent the Quest 3 from juddering in a very unpleasant manner. If you have something more hardcore, feel free to patch the `./generate-website` script.

> Note: I don't know for a scientific fact whether the issue with large splat files is the Quest 3 itself or something more specific to WebXR and ThreeJS. Feedback on this is welcome.

Once you have successfully opened the page on your Quest 3 or other WebXR headset, be sure to click "Enter VR." Behold your image... clear some space first in your living room... and try walking forward into it!

# Updates

If you add more images, just run `./photo-to-ply` and `./generate-website` again. Existing files are not regenerated, so if you really want them to be, remove the existing `.ply` files from the `./output` directory first.

## "Hey, it's slow"

Yeah, if you don't have an NVIDIA card it'll probably wind up using your CPU rather than your GPU unless you do some patching. So you won't get the "one second" results you may have seen mentioned. But it still does the job in about a minute per image even on my relatively wimpy Intel laptop. And in just 18 seconds on my Macbook Pro! So it's still pretty cool.
