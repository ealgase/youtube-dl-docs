.. youtube-dl documentation master file, created by
   sphinx-quickstart on Sun Jun 16 00:22:43 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to youtube-dl's documentation!
======================================

This page contains basic information about youtube-dl.

.. toctree::
   :hidden:
   :maxdepth: 3
   :glob:

   Usage/formatselect.rst
   Usage/output.rst
   Usage/videoselect.rst
   Usage/configuration.rst
   Usage/options.rst

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
.. code:: bash

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
