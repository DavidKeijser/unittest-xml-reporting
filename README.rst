unittest-xml-reporting
======================

unittest-xml-reporting is a unittest test runner that can save test results
to XML files that can be consumed by a wide range of tools, such as build
systems, IDEs and continuous integration servers.

:Author: Daniel Fernandes Martins <daniel@destaquenet.com>
:Company: `Destaquenet Technology Solutions`_


Requirements
------------

* `Python`_ 2.5+


Installation
------------

The easiest way to install unittest-xml-reporting is via `EasyInstall`_.
Follow `these instructions <http://pypi.python.org/pypi/setuptools>`_
to install EasyInstall if you don't have it already.

Then, execute the following command line to install the latest stable version
of unittest-xml-reporting::

    $ sudo easy_install unittest-xml-reporting

If you use Git and want to get the latest *development* version::

    $ git clone git://github.com/danielfm/unittest-xml-reporting.git
    $ cd unittest-xml-reporting
    $ sudo python setup.py install

Or get the latest *development* version as a tarball::

    $ wget http://github.com/danielfm/unittest-xml-reporting/tarball/master
    $ tar zxf danielfm-unittest-xml-reporting-XXXXXXXXXXXXXXXX.tar.gz
    $ cd danielfm-unittest-xml-reporting-XXXXXXXXXXXXXXXX
    $ sudo python setup.py install


Usage
-----

The script below, adapted from the `unittest`_ documentation, shows how to use
``XMLTestRunner`` in a very simple way. In fact, the only difference between
this script and the original one is the last line::

    import random
    import unittest
    import xmlrunner

    class TestSequenceFunctions(unittest.TestCase):
        def setUp(self):
            self.seq = range(10)

        def test_shuffle(self):
            # make sure the shuffled sequence does not lose any elements
            random.shuffle(self.seq)
            self.seq.sort()
            self.assertEqual(self.seq, range(10))

        def test_choice(self):
            element = random.choice(self.seq)
            self.assert_(element in self.seq)

        def test_sample(self):
            self.assertRaises(ValueError, random.sample, self.seq, 20)
            for element in random.sample(self.seq, 5):
                self.assert_(element in self.seq)

    if __name__ == '__main__':
        unittest.main(testRunner=xmlrunner.XMLTestRunner(output='test-reports'))


Django 1.2
``````````

In order to plug ``XMLTestRunner`` to a Django project, add the
following to your ``settings.py``::

    TEST_RUNNER = 'xmlrunner.extra.djangotestrunner.XMLTestRunner'


Also, the following settings are provided so you can fine tune the reports:

``TEST_OUTPUT_VERBOSE`` (Default: ``False``)
  Besides the XML reports generated by the test runner, a bunch of useful
  information is printed to the ``sys.stderr`` stream, just like the
  ``TextTestRunner`` does. Use this setting to choose between a verbose and a
  non-verbose output.

``TEST_OUTPUT_DESCRIPTIONS`` (Default: ``False``)
  If your test methods contains docstrings, you can display such docstrings
  instead of display the test name (ex: ``module.TestCase.test_method``). In
  order to use this feature, you have to enable verbose output by setting
  ``TEST_OUTPUT_VERBOSE = True``.

``TEST_OUTPUT_DIR`` (Default: ``"."``)
  Tells the test runner where to put the XML reports. If the directory
  couldn't be found, the test runner will try to create it before
  generate the XML files.


.. _Destaquenet Technology Solutions: http://www.destaquenet.com
.. _Python: http://python.org
.. _EasyInstall: http://peak.telecommunity.com/DevCenter/EasyInstall
.. _unittest: http://docs.python.org/library/unittest.html
