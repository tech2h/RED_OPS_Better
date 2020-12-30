# RED_OPS_Better
Introduction
------------

red_ops_better is a script which automatically transcodes and uploads these
files to Redacted or Orpheus.

The following command will scan through every FLAC you have ever
downloaded or uploaded (if it is in , determine which formats are needed, transcode
the FLAC to each needed format, and upload each format to Redacted or Orpheus -- automatically.

    $ ./red_ops_better

Installation
------------

You're going to need to install a few dependencies before using
red_ops_better.

First and foremost, you will need Python 3.6 or newer. NOTE: this version
has been ported to Python 3.x and will not run under Python 2.x.

Once you've got Python installed, you will need a few modules: mechanize,
mutagen, and requests. Try this:

    $ pip3 install -r requirements.txt

	
If you are on a seedbox, or a system without root priviliages, try this:


    $ pip3 install --user -r requirements.txt


Furthermore, you need several external programs: mktorrent 1.1+, flac,
lame, and sox. The method of installing these programs varies
depending on your operating system, but if you're using something like
Ubuntu you can do this:

    # aptitude install mktorrent flac lame sox
	

If you are on a seedbox and you lack the privilages to install packages,
you could contact your provider to have these packages installed.

At this point you may execute the following command:

    $ ./red_ops_better

And you will receive a notification stating that you should edit the
configuration file at same directory /config (if you're lucky).

Configuration
-------------

You've made it far! Congratulations. Open up the file 
config in a text editor. You're going to see something
like this:

	[RED]
	username = 
	password = 
	session_cookie = 
	data_dir = 
	output_dir = 
	torrent_dir = 
	formats = flac, v0, 320
	media = dat, cd, soundboard, web, sacd, vinyl, dvd, blu-ray
	24bit_behaviour = 0
	tracker = https://flacsfor.me/
	mode = both
	api = https://redacted.ch
	source = RED
	piece_length = 18

	[OPS]
	username = 
	password = 
	session_cookie = 
	data_dir = 
	output_dir = 
	torrent_dir = 
	formats = flac, v0, 320
	media = sacd, vinyl, soundboard, web, blu-ray, dat, cd, dvd
	24bit_behaviour = 0
	tracker = https://home.opsfet.ch/
	mode = both
	api = https://orpheus.network
	source = OPS
	piece_length = 18

`username` and `password` are your Redacted and Orpheus login credentials(Not required if using session_cookie). 

`session_cookie` working session cookie for tracker of your choice.

`data_dir` is the directory where your downloads are stored. 

`output_dir` is the directory where your transcodes will be created. If
the value is blank, `data_dir` will be used. You can also specify
per format values such as `output_dir_320` or `output_dir_v0`.

`torrent_dir` is the directory where torrents should be created (e.g.,
your watch directory). Same per format settings as output_dir apply.

`formats` is a list of formats that you'd like to support
(so if you don't want to upload V2, just remove it from this list).

`media` is a list of lossless media types you want to consider for
transcoding. The default value is all What.CD lossless formats, but if
you want to transcode only CD and vinyl media, for example, you would
set this to 'cd, vinyl'.

`24bit_behaviour` defines what happens when the program encounters a FLAC 
that it thinks is 24bits. If it is set to '2', every FLAC that has a bits-
per-sample property of 24 will be silently re-categorized. If it set to '1',
a prompt will appear. The default is '0' which ignores these occurrences.

`tracker` is the base url to use in the torrent files.

`api` is the base url to use for api requests.

`mode` is which list of torrents to search for candidates. One of:

`source` source flag for tracker.

`piece_length` used with MKtorrent default value.

 - `snatched` - Your snatched torrents.
 - `uploaded` - Your uploaded torrents.
 - `both`     - Your uploaded and snatched torrents.
 - `seeding`  - Better.php for your seeding torrents.
 - `all`      - All transcode sources above.
 - `none`     - Disable scraping.


You should end up with something like this:

	[RED]
	username = YOur_username//LEAVE black if you are using Session Cookie of RED
	password = Your_Password//Leave blank if you are using Session Cookie of RED
	session_cookie = Working_Session_Cookie_for_Red
	data_dir = /download/directory/for/RED
	output_dir = /transcode/directory/for/RED
	torrent_dir = /watch/directory/for/RED
	formats = flac, v0, 320
	media = dat, cd, soundboard, web, sacd, vinyl, dvd, blu-ray
	24bit_behaviour = 0
	tracker = https://flacsfor.me/
	mode = both
	api = https://redacted.ch
	source = RED
	piece_length = 18

	[OPS]
	username = YOur_username//LEAVE black if you are using Session Cookie of OPS
	password = Your_Password//Leave blank if you are using Session Cookie of OPS
	session_cookie = Working_Session_Cookie_OF_OPS
	data_dir = /download/directory/for/OPS
	output_dir = /transcode/directory/for/OPS
	torrent_dir = /watch/directory/for/OPS
	formats = flac, v0
	media = sacd, vinyl, soundboard, web, blu-ray, dat, cd, dvd
	24bit_behaviour = 0
	tracker = https://home.opsfet.ch/
	mode = both
	api = https://orpheus.network
	source = OPS
	piece_length = 18


Alright! Now you're ready to use red_ops_better.

Usage
-----

```
usage: red_ops_better [-h] [-s] [-j THREADS] [--config CONFIG] [--cache CACHE]
                      [-U] [-E] [-m MODE] [-S] [-t TOTP] [-o SOURCE]
                      [-T TRACKER]
                      [release_urls [release_urls ...]]

positional arguments:
  release_urls          the URL where the release is located (default: None)

optional arguments:
  -h, --help            show this help message and exit
  -s, --single          only add one format per release (useful for getting
                        unique groups) (default: False)
  -j THREADS, --threads THREADS
                        number of threads to use when transcoding (default: 3)
  --config CONFIG       the location of the configuration file (default:
                        Directory/of/RED_OPS_Better/config)
  --cache CACHE         the location of the cache (default:
                        Directory/of/RED_OPS_Better/cache)
  -U, --no-upload       don't upload new torrents (in case you want to do it
                        manually) (default: False)
  -E, --no-24bit-edit   don't try to edit 24-bit torrents mistakenly labeled
                        as 16-bit (default: False)
  -m MODE, --mode MODE  mode to search for transcode candidates; snatched,
                        uploaded, both, seeding, or all (default: None)
  -S, --skip            treats a torrent as already processed (default: False)
  -t TOTP, --totp TOTP  time based one time password for 2FA (default: None)
  -o SOURCE, --source SOURCE
                        the value to put in the source flag in created
                        torrents (default: None)
  -T TRACKER, --tracker TRACKER
                        pick your tracker for transcoding Default is
                        RED(choose tracker OPS,RED) (default: RED)
```

Examples
--------

To transcode and upload every snatch you've ever downloaded along with all
your uploads (this may take a while):

For RED

    $ ./red_ops_better

For OPS

    $ ./red_ops_better -T OPS


To transcode and upload a specific release (provided you have already
downloaded the FLAC and it is located in your `data_dir`):

For RED:

    $ ./red_ops_better "https://redacted.ch/torrents.php?id=1391069&torrentid=3095859"

For OPS

    $ ./red_ops_better -T OPS "https://orpheus.network/torrents.php?id=137293&torrentid=269985"

Note that if you specify a particular release(s), red_ops_better will
ignore your configuration's media types and attempt to transcode the
releases you have specified regardless of their media type (so long as
they are lossless types).

Your first time running red_ops_better might take a while, but after it has
successfully gone through and checked everything, it'll go faster any
consecutive runs due to its caching method.



Note: 
1) On first run it will encrypt your config file. Please provide a strong Password and remember it.
If you forget the password delete the config.enc file from the directory. It will ask you to create config file again.

2) If you like to use this for OPS as default. Open red_ops_better and search for "default='RED'" and replace RED to OPS.

