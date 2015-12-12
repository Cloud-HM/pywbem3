pywbem3
=======

This is a fork of python3-pywbem for Python 3.5+ that will, in the future, try to remain 100% compatible with PyWBEM as a drop-in replacement.

This project started off originally from deejross's https://github.com/deejross/python3-wbem project. We tried to use the library to connect to our CIM server, but couldn't due to a veriety of reasons. We tried both Python 2.7 and Python 3.5. The main reasons were:

1) Python 2.7 version of python3-pywbem has a dependency on M2Crypto, and since we develop on Mac OS X El Capitan, getting it to install properly was difficult. (See: http://stackoverflow.com/questions/33005354/trouble-installing-m2crypto-with-pip-on-el-capitan. We also ran into a bunch of other issues.)

2) Python 3.5 version wouldn't run as well because of SSL and some errors related to `ValueError: check_hostname requires server_hostname`.

**TL;DR:** it was just too difficult. Let's start over!

So we made a few decisions. They were:
-------
* It's year 2016, we should **only** use Python 3
* Cleaning up all code related to 2.7
* Use idiomatic Python 3 SSL code

License
-------
LGPL 2.1 (original PyWBEM license)


Motivation
----------
When working with WBEM/CIM and Python, PyWBEM is the only library available. Even libraries like PowerCIM use PyWBEM under the hood. Unfortunately, PyWBEM only supports the Python 2.x series and carries a heavy dependency, M2Crypto, that is difficult to install on platforms other than Linux. Worse yet, this dependency is also restricted to the Python 2.x series. There have been some efforts to update M2Crypto for Python 3, but none seem to be in a fully working state.

This fork introduces Python 3.4+ support by dropping M2Crypto in favor of Python's newly updated, built-in `ssl` module. This module improves Python's SSL security enough that M2Crypto is no longer required. However, Python 2.6+ still requires M2Crypto. Compatibility with the 2.x series should make the transition to Python 3.4+ seamless.

Dependencies
------------
With Python 3.5+:
 * six
 

Installation
------------
There is no PyPI package. For now, make a `pywbem` folder in your site-packages folder and put the contents of this repo inside it.

Documentation
-------------
See the official documentation: http://pywbem.sf.net/

Examples
--------
The following is copied from the official examples (see documentation) for convenience:

```python
import pywbem

# Make connection
conn = pywbem.WBEMConnection('https://server',     # url
                             ('root', 'penguin'))  # credentials

# Enumerate CIM_Process instance names and instances

instance_names = conn.EnumerateInstanceNames('CIM_Process')
instances = conn.EnumerateInstance('CIM_Process')

print '%d CIM_Process names returned' % len(instance_names)
print '%d CIM_Process instances returned' % len(instance_names)

# Get all CIM_OperatingSystem instances

names = conn.EnumerateInstanceNames('CIM_OperatingSystem')

# Call GetInstance on returned instances

for n in names:
    os = conn.GetInstance(n)
    for key, value in os.items():
        print '%s = %s' % (key, value)
```