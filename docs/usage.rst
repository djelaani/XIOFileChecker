.. _usage:


Generic usage
=============

.. note:: All the following arguments can be safely combined and add to the subcommand arguments.

Check the help
**************

.. code-block:: bash

    $> XIOFileChecker {-h,--help}

Check the version
*****************

.. code-block:: bash

    $> XIOFileChecker {-v,--version}

Debug mode
**********

You can switch to a more verbose mode displaying each step with useful additional information.

.. code-block:: bash

    $> XIOFileChecker --debug

.. warning::
The verbose mode is silently activated in the case of a logfile.

Use a logfile
*************

Information can be logged into a file named ``XIOFileChecker-YYYYMMDD-HHMMSS.log`` only if ``-l`` is submitted.
If not, the standard output is used following the verbose mode.
By default, the logfiles are stored in a ``logs`` folder created in your current working directory (if not exists).
It can be changed by adding an optional logfile directory to the flag.

.. code-block:: bash

    $> XIOFileChecker {-l,--log}
    $> XIOFileChecker -l /PATH/TO/LOGDIR/

Compare DR2XML files definition to XIOS output netCDF files
***********************************************************

Default is to submit a directory that includes both the XML files definition from DR2XML either with the netCDF
files produces by XIOS. Basically, this is the root directory of your simulation.

.. code-block:: bash

    $> XIOFileChecker /PATH/TO/MY/SIMULATION

A different directory can be submitted for the XML files definition from DR2XML:

.. code-block:: bash

    $> XIOFileChecker /PATH/TO/MY/SIMULATION --xml /PATH/TO/MY/FILEDEF

If you use the IPSL libIGCM framework, you can directly submit the directory of the ``run.card`` and ``config.card``
files to automatically deduce the DR2XML files to consider:

.. code-block:: bash

    $> XIOFileChecker /PATH/TO/MY/SIMULATION --card /PATH/TO/MY/CARDFILES

.. warning:: The XML directory will be recursively scan to get all ``dr2xml_*.xml`` files.

Print common entries between DR2XML and XIOS
********************************************

Default only shows the difference between XIOS input and output. You can add the common entries to the results
with:

.. code-block:: bash

    $> XIOFileChecker /PATH/TO/MY/SIMULATION --all

.. warning:: This would change the results totals in the table footer.

Filter entries by facets
************************

Default applies no filters and print all entries. The result can be filtered by any facet using an usual
regular expression:

.. code-block:: bash

    $> XIOFileChecker /PATH/TO/MY/SIMULATION --set-filter <FACEY_KEY>=<REGEX>

.. note:: Duplicate the argument to apply several filters for different facets.

.. warning:: Only one filter per facet key is allowed. If several filters refer to the same facet, only the latest one
will be considered.

.. warning:: This would change the results totals in the table footer.

Display all facets
******************

Default only displays the comparison result without the unchanged facets in order to make the result easier to read.
To not hide the facet with unchanged values among resulting entries:

.. code-block:: bash

    $> XIOFileChecker /PATH/TO/MY/SIMULATION --full-table

Use multiprocessing
*******************

``XIOFileChecker`` uses a multiprocessing interface. This is useful to process a large amount of XML files.
Set the number of maximal processes to simultaneously treat several files.
One process seems sequential processing. Set it -1 to use all available CPU processes
(as returned by ``multiprocessing.cpu_count()``). Default is set to 4 processes.

.. code-block:: bash

    $> XIOFileChecker /PATH/TO/MY/SIMULATION --max-processes 4

.. warning:: The number of maximal processes is limited to the maximum CPU count in any case.

Exit status
***********

 * Status = 0
    All the files have been successfully scanned and comparison went right.
 * Status = 1
    Errors may occur during file scanning.
 * Status = -1
    Argument parsing error.