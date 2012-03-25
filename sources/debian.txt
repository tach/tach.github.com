============================
Debian GNU/Linux Development
============================

Make package using svn-buildpackage
===================================

Import your repository to svn.debian.org
----------------------------------------

svn.debian.org (Alioth) has two different types of repositories.

* `deb-maint <http://anonscm.debian.org/viewvc/collab-maint/deb-maint/>`_ :
  Collaboration between Debian Developers

  * ``svn co svn+ssh://svn.debian.org/svn/collab-maint/deb-maint``

* `ext-maint <http://anonscm.debian.org/viewvc/collab-maint/ext-maint/>`_ :
  Collaboration between Debian Developers and others

  * ``svn co svn+ssh://svn.debian.org/svn/collab-maint/deb-maint``

Select repository which you should use.

Read http://wiki.debian.org/Alioth/CollabMaintImport at first.
Then, execute following commands to dump.

.. code-block:: sh

   SVNROOTPATH=/your/svn/root/path/of/repos
   PKGBASEDIR=debian
   PKG=your-package-name

   workdir=`mktemp -d` && pushd $workdir

   # dump your package
   dumpfile=$PKG.svndump
   svnadmin dump $SVNROOTPATH \
     | svndumpfilter --drop-empty-revs include $PKGBASEDIR/$PKG \
     1> $dumpfile 2> dumpfile.error
   perl -i.bak -npe "s|^(\S+-path: )$PKGBASEDIR/|\$1|" $dumpfile

   # restore test
   svnadmin create $workdir/collab-maint
   svn mkdir file://$workdir/collab-maint/deb-maint -m "Initial directory creation"
   svnadmin load $workdir/collab-maint --parent-dir deb-maint < $dumpfile \
     > load 2> load.error
   test ! -s load.error && echo "Succeeded"

   # check restored
   svn list file://$workdir/collab-maint/deb-maint
   svn log file://$workdir/collab-maint/deb-maint/$PKG

   # additional tests
   svn co file://$workdir/collab-maint/deb-maint/$PKG
   pushd $PKG/trunk && svn-buildpackage -uc -us
   popd

   # copy dumpfile to svn.debian.org
   scp $dumpfile svn.debian.org:

   # remove working dir
   popd && rm -rf $workdir

After that, you can restore dumped file into svn.debian.org.

.. code-block:: sh

   # log into svn.debian.org
   ssh svn.debian.org

   # check repository
   svn list file:///svn/collab-maint/deb-maint
   # or svn list file:///svn/collab-maint/ext-maint

   # load file to repos (replace dumpfile with real filename)
   svnadmin load /svn/collab-maint --parent-dir deb-maint < dumpfile > load 2> load.error
   test ! -s load.error && echo "Succeeded"

   # check restored (replace your-package-name with real package name)
   svn list 
   svn log file:///svn/collab-maint/deb-maint/your-package-name

At last, checkout your repository and build.

.. code-block:: sh

   svn co svn+ssh://svn.debian.org/svn/collab-maint/deb-maint/your-package-name
   cd your-package-name
   svn-buildpackage

See also

* `Alioth CollabMaintImport <http://wiki.debian.org/Alioth/CollabMaintImport>`_
* `Alioth PackagingProject <http://wiki.debian.org/Alioth/PackagingProject>`_
* `Useful tricks with SVN on Alioth <http://wiki.debian.org/Alioth/Svn>`_
