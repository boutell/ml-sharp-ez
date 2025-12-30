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

Now convert them:

```bash
# cd into this repo you just checked out
cd ml-sharp-ez 
./photo-to-ply
```

After this command `./output` will contain:

* Full-size `.ply` files for each input file.
* Reduced-size `.ply` files for each input file, for use with the Quest 3 headset.

# Viewing your splat files with WebXR

```bash
# cd into this repo you just checked out
cd ml-sharp-ez 
./generate-website
```

This will create a `./website` folder in `ml-sharp-ez`.

You can serve that `./website` folder:

```bash
cd ./website
python3 -m http.server
```

Then visit the resulting test site at:

http://localhost:8000

However note you probably will not have a usable "Enter VR" button this way.

For that, you will need to deploy the `./website` folder to the static hosting of your choice, which must support HTTPS, otherwise the "Enter VR" buttons will not work. But just about everything comes with HTTPS support these days.

Yes, you can deploy it as a subdirectory of another site if you want to.

**The website is built with the reduced-size `.ply` files** for Quest 3 support. If you have something more hardcore, feel free to patch the `./generate-website` script.

# Updates

If you add more images, just run `./photo-to-ply` and `./generate-website` again. Existing files are not regenerated, so if you really want them to be, remove the existing `.ply` files from the `./output` directory first.

## "Hey, it's slow"

If you don't have an NVIDIA card it'll probably wind up using your CPU rather than your GPU. So you won't get the "one second" results you may have seen mentioned. But it still does the job in about a minute, even on my relatively wimpy Intel laptop. So it's still pretty cool.
