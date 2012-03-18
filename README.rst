===============
tach.github.com
===============

This repository is for managing http://tach.github.com/ .

How to edit tach.github.com
===========================

1. Install sphinx on your client
   http://sphinx.pocoo.org/
2. Clone this repository
   ``git clone git@github.com:tach/tach.github.com.git``
3. Edit your documents
   ``cd src`` ``vi index.rst``
4. Build and check the contents
   ``make html && open ../_build/html/index.html``
5. Publish to github
   ``make publish``
