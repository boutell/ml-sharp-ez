`ml-sharp-ez` helps you take a collection of still images and turn them into a collection of "3-D splat" scenes you can explore in VR, using any WebXR-capable device, such as an Oculus Quest 3 (tested) or Apple Vision Pro (untested).

This package is intended to democratize participation in 2d-3d image conversion research by easing the use of Apple's [ml-sharp model](https://github.com/apple/ml-sharp), which converts still images into 3d splat files in `.ply` format. This package then packs them up as a little WebXR application for headsets. It's pretty cool, but keep in mind you [must still abide by Apple's license for the model](https://github.com/apple/ml-sharp/blob/main/LICENSE_MODEL).

# Setting expectations

The results are really neat! It's like being inside a needlepoint "painting." But, the results are not high definition and the model doesn't try too hard to predict things that aren't part of the original image. Also, see below re: intentional resolution reductions to allow the Quest 3 to handle it.

# Before you install

## OS requirements

You must have MacOS, Linux, WSL or similar. Unfortunately these scripts won't run on vanilla Windows.

## Build environment

You need a build environment.

* For Mac, install the xcode dev tools:

```bash
xcode-select --install
```

* For Ubuntu- or Debian-flavored Linux, install the needed dependencies for `pyenv` to build Python with all the trimmings:

```bash
sudo apt install -y build-essential zlib1g-dev libssl-dev libbz2-dev \
  libreadline-dev libsqlite3-dev curl libncursesw5-dev xz-utils tk-dev \
  libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```

* For Red Hat-flavored Linux:

```bash
sudo dnf groupinstall "Development Tools"
sudo dnf install -y zlib-devel openssl-devel bzip2-devel \
  readline-devel sqlite-devel curl ncurses-devel xz-devel \
  tk-devel libxml2-devel xmlsec1-devel libffi-devel xz-devel
```

# Installing

Run:

```bash
git clone https://github.com/boutell/ml-sharp-ez
# always cd into the repo
cd ml-sharp-ez 
./install
```

If there are any errors you will need to resolve them before proceeding.

It is OK to run the install script more than once if you have to fix issues and try again.

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

However note you probably will not have a usable "Enter VR" button this way. It's just a way to test drive your 3d scenes using mouse-based navigation.

To enable the "Enter VR" button, you will need to deploy the `./website` folder to the static hosting of your choice, which must support HTTPS. But just about everything comes with HTTPS support these days.

Then you'll be able to access it from your Quest 3 or other headset and click "Enter VR."

(Yes, you can deploy `./website` as a subdirectory of another site if you want to.)

**The website is built with reduced-size `.ply` files** to prevent the Quest 3 from juddering in a very unpleasant manner. If you have something more hardcore, feel free to patch the `./generate-website` script.

> Note: I don't know for a scientific fact whether the issue with large splat files is the Quest 3 itself or something more specific to WebXR and ThreeJS. Feedback on this is welcome.

Once you have successfully opened the page on your Quest 3 or other WebXR headset, be sure to clear some space in your living room. Click "Enter VR" and walk around your image!

Yes, it looks like everybody is a handmade doll and the model doesn't try to simulate the back of anything, but it's still very cool.

**Use the "Prev" and "Next" buttons to switch between scenes.** You might have to walk back to see them again.

# Adding more images

If you add more images, just run `./convert` and `./generate-website` again. Existing files are not regenerated, so if you really want them to be, remove the existing `.ply` files from the `./output` directory first. However, note that browsers will probably cache the old ones anyway. My advice for that is to change the filenames.

# "Hey, it's slow"

Yeah, if you don't have an NVIDIA card it'll probably wind up using your CPU rather than your GPU unless you do some patching. So you won't get the "one second" results you may have seen mentioned. But it still does the job in about a minute per image even on my relatively wimpy AMD laptop. And in just 18 seconds on my Macbook Pro! Whaddaya want, it's going to space.

# "I found a bug / I have an idea"

Pull requests and github issues welcome. Remember, we have to abide by Apple's license, this is a research tool not a product.

Contributions to get the GPU working for more people more of the time would be great.

# Apple's license says it's for research. How do I participate in research?

I suggest providing **helpful, constructive** feedback on interesting results, as well as reporting any bugs you encounter when the `sharp` command itself is running, via [github issues on Apple's ml-sharp repo](https://github.com/apple/ml-sharp/issues).

But if you encounter a bug outside of the `sharp` command itself, i.e. in my `install` script or other parts of `convert` or `generate-website`, please submit [an issue on ml-sharp-ez](https://github.com/boutell/ml-sharp-ez/issues) instead.

If you're not sure, start by reporting it on `ml-sharp-ez`.

# Thanks

* Thanks to Apple, of course, for allowing research use of the [ml-sharp model](https://github.com/apple/ml-sharp).
* Thanks to the [ThreeJS](https://threejs.org/) team.
* Thanks to Felix Hirt for the [Gaussian Decimator](https://github.com/feel3x/Gaussian_Decimator) utility, used here to simplify the splat files just enough for a good experience on the Quest 3.
