=========================
Using Sphinx
=========================

Installing Sphinx into MacOSX 10.7 (Lion)
=========================================

Run following two commands to install.

.. code-block:: sh

  % sudo easy_install pip
  % sudo pip install Sphinx

That's all.

Installing Sphinx into Cygwin
=============================

Run following commands to install.

.. code-block:: sh

   % wget http://peak.telecommunity.com/dist/ez_setup.py
   % python ez_setup.py
   % sudo easy_install pip
   % pip install Sphinx

How to create tach.github.com
=============================

At first, create github repository at https://github.com/tach/tach.github.com
and clone it.

.. code-block:: sh

  % git clone git@github.com:tach/tach.github.com.git

Then, create ``src`` directory and run ``sphinx-quickstart``.
You may answer all questions as default.

.. code-block:: sh

  % cd tach.github.com
  % mkdir src
  % cd src
  % sphinx-quickstart

Install **phinxtogithub extension**
(https://github.com/michaeljones/sphinx-to-github)
into your exts directory.

.. code-block:: sh

  % mkdir exts
  % pushd exts
  % git clone git://github.com/michaeljones/sphinx-to-github.git
  % rsync -rlvHS --exclude=tests sphinx-to-github/sphinxtogithub ./
  % rm -rf sphinx-to-github
  % popd

Edit sys.path on your ``conf.py`` and Add extension settings.

.. code-block:: python

  sys.path.insert(0, os.path.abspath('exts'))
  extensions = [ "sphinxtogithub" ]

.. code-block:: python

  # options for sphinxtogithub
  sphinx_to_github = True
  sphinx_to_github_verbose = True

Edit your Makefile like https://github.com/tach/tach.github.com/blob/master/src/Makefile .

At last, run ``make publish`` to publish your pages.

.. code-block:: sh

  % make publish

References
==========

* `Sphinx Technical Information (in Japanese) <http://sphinx-users.jp/technical.html>`_
