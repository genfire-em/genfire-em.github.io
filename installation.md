# Installation

## Table of contents

- [Get the Source Code](#get-the-source-code)
- [Mac OS X](#mac-os-x)
- [Add your own content](#add-your-own-content)
- [Last important thing: YAML front matter ("parameters" for a page)](#last-important-thing-yaml-front-matter-parameters-for-a-page)
- [Features](#features)
- [Creating a User Page vs a Project Page](#creating-a-user-page-vs-a-project-page)
- [Showcased users (success stories!)](#showcased-users-success-stories)
- [Advanced: local development](#advanced-local-development-using-docker)
- [Credits and contributions](#credits)


## Get the Source Code

To install GENFIRE as a python package, first 
[download the source code](www.github.com/genfire-em). Then follow the procedure for 
your operating system.

## Mac OS X

Python is preinstalled on Mac OS X, but it is generally a bad idea to alter the system
python in /usr/bin as some programs depend on it. I would highly recommend using the
package manager homebrew to create a python environment for you. The following
commands will install homebrew and python3

~~~
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
$ brew install python3
~~~

Once that's done, navigate to the top source code directory. That's the same
folder as requirements.txt and setup.py

~~~
$ cd /path/to/GENFIRE
~~~

Install (most of the) dependencies using pip3, you may need to prepend the command with sudo depending on your system

~~~
$ pip install --upgrade pip
$ pip3 install -r requirements.txt
~~~

You will also need sip and PyQt5		

~~~
$ brew install sip
$ brew install pyqt
~~~

You should now have all the dependencies necessary to run GENFIRE. To install it, make
sure you are in the directory with setup.py and enter the follow. Again, you may need sudo

~~~
$ python3 setup.py install
~~~

If everything worked, you can now launch the gui from the command line with

~~~
$ genfire
~~~

You can also use the code as any other python package. Here's an example of launching the gui from within python

~~~
$ python3
~~~

~~~ python
$ import GENFIRE
$ GENFIRE.gui.launch.main()
~~~

### (Optional) Turbo-charge GENFIRE with FFTW

GENFIRE can make use of[pyFFTW](https://pypi.python.org/pypi/pyFFTW), which wraps the [FFTW](http://www.fftw.org/) library. I have tested a number of
FFT routines including those in NumPy, SciPy, and pyFFTW, and found this to be the fastest one by a factor of 2-3.

*The following are details for installing FFTW from source, but recently pip has begun to support the package, so you should first try the easy way with*

~~~
pip install pyfftw
~~~

In order for GENFIRE to use pyFFTW you must install [the FFTW libraries](http://www.fftw.org/download.html). Download the source code, decompress it, and navigate into the unpacked directory
from your terminal. PyFFTW needs FFTW to be compiled for all precision types, so you have to compile it three times.
Use the following commands

~~~
$ ./configure --enable-threads --enable-shared
$ make
$ sudo make install
~~~