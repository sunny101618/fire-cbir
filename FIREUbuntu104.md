To facilitate installation, I provide here a description how to compile the FIRE server on Ubuntu 10.4 from scratch.

The description applies to FIRE V 2.3 (SVN checkout of July 26, 2010).


## Compiling FIRE ##

  * default Ubuntu 10.4 installation (in May 2010 in virtualbox)
  * upgrade to recent state (sudo apt-get update; sudo apt-get dist-upgrade) on July 26, 2010
  * sudo apt-get install subversion g++ libmagick++-dev
  * svn checkout http://fire-cbir.googlecode.com/svn/trunk/ fire-cbir
  * cd fire-cbir
  * make

After that the bin-Directory contain a total of 47 binaries and the FIRE server is ready to use.



## Integration with Apache ##
  * sudo apt-get install apache2
  * cd ~/fire-cbir/
  * sudo cp Python/firesocket.py /usr/lib/cgi-bin
  * cd WebInterface
  * sudo cp fire.py feature.py img.py config.py fire-template.html /usr/lib/cgi-bin
  * sudo mkdir /usr/lib/cgi-bin/images
  * sudo cp neutral.png positive.png negative.png fire-logo.png i6.png /usr/lib/cgi-bin/images


Now all the files should be in place for the FIRE webinterface to work

## Setting up a dataset ##
  * create a directory mkdir ~/fire-img
  * put a set of medium sized images (e.g. 500x500 pixels) into this directory. For a start use about 100 images.
  * convert all images into jpg (not necessary but often avoids problems)
    * cd ~/fire-img
    * mogrify -format jpg `*`
  * extract color histograms
    * ~/fire-cbir/bin/extractcolorhistograms --color --images ~/fire-img/`*`.jpg
  * create a filelist for this dataset
    * cd ~/fire-img/
    * echo FIRE\_filelist > filelist
    * echo suffix color.histo.gz >> filelist
    * echo path this >> filelist
    * ls `*`.jpg | sed 's/^/file /' >> filelist

Now you have a dataset where every image is represented using a color histogram.
Other features can be added by extracting them and adding the corresponding suffix lines to the filelist.


## Starting the FIRE server and accessing it through the webinterface ##
  * cd ~/fire-cbir
  * ./bin/fire -f ~/fire-img/filelist -r 10
  * Access the fire webinterface throught http://localhost/cgi-bin/fire.py
  * There you should be able to click an image and get similar images (according to color histograms)