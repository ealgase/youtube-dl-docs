.. youtube-dl documentation master file, created by
   sphinx-quickstart on Sun Jun 16 00:22:43 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to youtube-dl's documentation!
======================================

This page contains basic information about youtube-dl (installation, format selection, etc).

.. toctree::
   :maxdepth: 2
   :caption: Contents:



What is youtube-dl?
-------------------

youtube-dl is a command-line program written in Python to download online videos from websites including YouTube, Vimeo, Soundcloud, and many others.

Installation
------------

youtube-dl can be installed in many ways.  The preferred way to install is using pip: ``pip install youtube-dl``.  

youtube-dl can also be installed via a package manager (e.g. ``sudo apt install youtube-dl``), but these versions are almost always outdated, due to the fast pace that websites are updated.

FFmpeg is necessary for merging video and audio and downloading HLS and DASH.  Instructions for installing FFmpeg can be found at https://ffmpeg.org/download.html.  avconv can also be used, but it is not recommended.

PhantomJS is necessary for extracting videos from Openload and PornHub.  Instructions for installing PhantomJS can be found at http://phantomjs.org/download.html.

Basic usage
-----------

To download a video, simply run ``youtube-dl videourl``.  The video will be downloaded in the highest quality available, unless FFmpeg is not installed, in which case it will be downloaded in the highest quality containing both video and audio in one file.

Example
^^^^^^^^
::

	$ youtube-dl https://www.youtube.com/watch?v=BaW_jenozKc
	[youtube] BaW_jenozKc: Downloading webpage
	[youtube] BaW_jenozKc: Downloading video info webpage
	[download] Destination: youtube-dl test video ''_ä↭𝕐-BaW_jenozKc.f137.mp4
	[download] 100% of 2.11MiB in 00:00
	[download] Destination: youtube-dl test video ''_ä↭𝕐-BaW_jenozKc.f140.m4a
	[download] 100% of 154.06KiB in 00:00
	[ffmpeg] Merging formats into "youtube-dl test video ''_ä↭𝕐-BaW_jenozKc.mp4"
	Deleting original file youtube-dl test video ''_ä↭𝕐-BaW_jenozKc.f137.mp4 (pass -k to keep)
	Deleting original file youtube-dl test video ''_ä↭𝕐-BaW_jenozKc.f140.m4a (pass -k to keep)

FFmpeg is installed, so the highest quality video and audio are downloaded and merged.

Format selection
----------------
By default, youtube-dl downloads the highest quality of a video, and no format selection is required.

However, you may want to download in a different format, for example when you are on a slow or intermittent connection. The key mechanism for achieving this is so-called format selection based on which you can explicitly specify desired format, select formats based on some criterion or criteria, setup precedence and much more.

The general syntax for format selection is ``--format FORMAT`` or shorter ``-f FORMAT`` where FORMAT is a *selector* expression, i.e. an expression that describes format or formats you would like to download.

**tl;dr:** jump to the :ref:`format-selection-examples`

The simplest case is requesting a specific format, for example with ``-f 22`` you can download the format with format code equal to 22. You can get the list of available format codes for particular video using ``--list-formats`` or ``-F``. Note that these format codes are extractor specific.

You can also use a file extension (currently ``3gp``, ``aac``, ``flv``, ``m4a``, ``mp3``, ``mp4``, ``ogg``, ``wav``, ``webm`` are supported) to download the best quality format of a particular file extension served as a single file, e.g. ``-f webm`` will download the best quality format with the webm extension served as a single file.

You can also use special names to select particular edge case formats:

* ``best``: Select the best quality format represented by a single file with video and audio.
* ``worst``: Select the worst quality format represented by a single file with video and audio.
* ``bestvideo``: Select the best quality video-only format (e.g. DASH video). May not be available.
* ``worstvideo``: Select the worst quality video-only format. May not be available.
* ``bestaudio``: Select the best quality audio only-format. May not be available.
* ``worstaudio``: Select the worst quality audio only-format. May not be available.

For example, to download the worst quality video-only format, use ``-f worstvideo``.

If you want to download multiple videos and they don't have the same formats available, you can specify the order of preference using slashes. Note that slash is left-associative, i.e. formats on the left hand side are preferred, for example ``-f 22/17/18`` will download format 22 if it's available, otherwise it will download format 17 if it's available, otherwise it will download format 18 if it's available, otherwise it will report that no suitable formats are available for download.

If you want to download several formats of the same video use a comma as a separator, e.g. ``-f 22,17,18`` will download all these three formats, of course if they are available. Or a more sophisticated example combined with the precedence feature: ``-f 136/137/mp4/bestvideo,140/m4a/bestaudio``.

You can also filter the video formats by putting a condition in brackets, as in ``-f "best[height=720]"`` (or ``-f "[filesize>10M]"``).

The following numeric meta fields can be used with comparisons ``<``, ``<=``, ``>``, ``>=``, ``=`` (equals), ``!=`` (not equals):

* ``filesize``: The number of bytes, if known in advance
* ``width``: Width of the video, if known
* ``height``: Height of the video, if known
* ``tbr``: Average bitrate of audio and video in KBit/s
* ``abr``: Average audio bitrate in KBit/s
* ``vbr``: Average video bitrate in KBit/s
* ``asr``: Audio sampling rate in Hertz
* ``fps``: Frame rate

Filtering also works with operators ``=`` (equals), ``^=`` (starts with), ``$=`` (ends with), ``*=`` (contains) on the following string meta fields:

* ``ext``: File extension
* ``acodec``: Name of the audio codec in use
* ``vcodec``: Name of the video codec in use
* ``container``: Name of the container format
* ``protocol``: The protocol that will be used for the actual download, lower-case (``http``, ``https``, ``rtsp``, ``rtmp``, ``rtmpe``, ``mms``, ``f4m``, ``ism``, ``http_dash_segments``, ``m3u8``, or ``m3u8_native``)
* ``format_id``: A short description of the format

Any string comparison may be prefixed with negation ``!`` in order to produce an opposite comparison, e.g. ``!*=`` (does not contain).

Note that none of the aforementioned meta fields are guaranteed to be present since this solely depends on the metadata obtained by particular extractor, i.e. the metadata offered by the video hoster.

Formats for which the value is not known are excluded unless you put a question mark (``?``) after the operator. You can combine format filters, so ``-f "[height <=? 720][tbr>500]"`` selects up to 720p videos (or videos where the height is not known) with a bitrate of at least 500 KBit/s.

You can merge the video and audio of two formats into a single file using ``-f <video-format>+<audio-format>`` (requires ffmpeg or avconv installed), for example ``-f bestvideo+bestaudio`` will download the best video-only format, the best audio-only format and mux them together with ffmpeg/avconv.

Format selectors can also be grouped using parentheses, for example if you want to download the best mp4 and webm formats with a height lower than 480 you can use ``-f '(mp4,webm)[height<480]'``.

.. _format-selection-examples:

Examples
^^^^^^^^

::

	# Download best mp4 format available or any other best if no mp4 available
	$ youtube-dl -f 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best'
	
	# Download best format available but no better than 480p
	$ youtube-dl -f 'bestvideo[height<=480]+bestaudio/best[height<=480]'
	
	# Download best video only format but no bigger than 50 MB
	$ youtube-dl -f 'best[filesize<50M]'
	
	# Download best format available via direct link over HTTP/HTTPS protocol
	$ youtube-dl -f '(bestvideo+bestaudio/best)[protocol^=http]'
	
	# Download the best video format and the best audio format without merging them
	$ youtube-dl -f 'bestvideo,bestaudio' -o '%(title)s.f%(format_id)s.%(ext)s'

**Note:** in the last example, an output template is recommended, as bestvideo and bestaudio may have the same file name.