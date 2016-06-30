Transfer Documentation
======================

Installation
------------

We use Sphinx to build our documentation.

Before you start, be sure to install our version of Sphinx:

```
pip install git+https://github.com/valisj/sphinx
pip install git+https://github.com/valisj/sphinxcontrib-fulltoc
```

This requires `pip` to be installed on your system. To install `pip`, follow the [installation instructions](https://pip.pypa.io/en/stable/installing/) on their official documentation.

Building
--------

Run the following command to build the documentation in HTML format:

```
make html
```

Then, open `_build/html/index.html` in your browser.
