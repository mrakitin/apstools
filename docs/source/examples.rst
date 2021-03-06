
Examples
========

* :ref:`example_Excel_scan`
* :ref:`example_plan_catalog`
* :ref:`example_specfile`
* :ref:`example_nscan`
* :ref:`example_tuneaxis`
* :ref:`example_source`
* :ref:`examples_downloads`

.. index:: Excel scan, scan; Excel

.. _example_Excel_scan:

Example: *Excel scan*
+++++++++++++++++++++++++++

You can use an Excel spreadsheet as a multi-sample batch scan tool.  Follow
the example spreadsheet (in the 
:ref:`examples_downloads` section below)
and accompanying Jupyter notebook 
(https://github.com/BCDA-APS/apstools/blob/master/docs/source/resources/excel_scan.ipynb)
to write your own ``Excel_plan()``.

**SIMPLE**:  Your Excel spreadsheet could be rather simple...

.. figure:: resources/excel_simple.jpg
   :width: 95%
   
   Unformatted Excel spreadsheet for batch scans.

See :class:`ExcelDatabaseFileGeneric` for an example bluesky plan
that reads from this spreadsheet.

**FANCY**:  ... or contain much more information, including formatting.

.. _excel_plan_spreadsheet_screen:

.. figure:: resources/excel_plan_spreadsheet.jpg
   :width: 95%
   
   Example Excel spreadsheet for multi-sample batch scans.

The idea is that your table will start with column labels 
in **row 4** of the Excel spreadsheet.  One of the columns will be the name
of the action (in the example, it is ``Scan Type``).  The other columns will
be parameters or other information.  Each of the rows under the labels will
describe one type of action such as a scan.  Basically, whatever you  
handle in your ``Excel_plan()``.  
Any rows that you do not handle will be reported to the console during execution
but will not result in any action.
Grow (or shrink) the table as needed.

.. note::  For now, make sure there is no content in any of the spreadsheet
   cells outside (either below or to the right) of your table.  
   Such content will trigger a cryptic error
   about a numpy float that cannot be converted.  Instead, put that content 
   in a second spreadsheet page.
   
   .. see: https://github.com/BCDA-APS/apstools/issues/116

You'll need to have an action plan for every different action your spreadsheet
will specify.  Call these plans from your ``Excel_plan()`` within an ``elif`` block,
as shown in this example.  The example ``Excel_plan()`` converts the ``Scan Type`` 
into  lower case for simpler comparisons.  Your plan can be different if you choose.

::

        if scan_command == "step_scan":
            yield from step_scan(...)
        elif scan_command == "energy_scan":
            yield from scan_energy(...)
        elif scan_command == "radiograph":
            yield from AcquireImage(...)
        else:
            print(f"no handling for table row {i+1}: {row}")

The example plan saves all row parameters as metadata to the row's action.
This may be useful for diagnostic purposes.


.. _example_plan_catalog:

Example: ``plan_catalog()``
+++++++++++++++++++++++++++

The *apstools* package provides an executable that can be 
used to display a summary of all the scans in the database.  
The executable wraps the demo function: :func:`plan_catalog()`.
It is for demonstration purposes only (since it does not filter
the output to any specific subset of scans).

The output is a table, formatted as restructured text, with these columns:

:date/time:

   The date and time the scan was started.

:short_uid:

   The first characters of the scan's UUID (unique identifier).

:id:

   The scan number.  
   (User has control of this and could reset the counter for the next scan.)

:plan:

   Name of the plan that initiated this scan.

:args:

   Arguments to the plan that initiated this scan.


This is run as a linux console command::

   apstools_plan_catalog | tee out.txt

The :download:`full output <resources/plan_catalog.txt>`
is almost a thousand lines.  Here are the first few lines:

.. literalinclude:: resources/plan_catalog.txt
   :language: console
   :linenos:
   :lines: 1-10

.. _example_specfile:

Example: ``specfile_example()``
+++++++++++++++++++++++++++++++

We'll use a Jupyter notebook to demonstrate the ``specfile_example()`` that writes one or more scans to a SPEC data file.
Follow here: https://github.com/BCDA-APS/apstools/blob/master/docs/source/resources/demo_specfile_example.ipynb


.. _example_specfile_databroker:

Example: Create a SPEC file from databroker
+++++++++++++++++++++++++++++++++++++++++++

We'll use a Jupyter notebook to demonstrate the how to get a scan from the databroker and write it to a spec data file.
Follow here: https://github.com/BCDA-APS/apstools/blob/master/docs/source/resources/demo_specfile_databroker.ipynb


.. _example_nscan:

Example: ``nscan()``
++++++++++++++++++++

We'll use a Jupyter notebook to demonstrate the ``nscan()`` plan.  An *nscan* is used to scan two or more axes together,
such as a :math:`\theta`-:math:`2\theta` diffractometer scan.
Follow here: https://github.com/BCDA-APS/apstools/blob/master/docs/source/resources/demo_nscan.ipynb

.. _example_tuneaxis:

Example: ``TuneAxis()``
+++++++++++++++++++++++

We'll use a Jupyter notebook to demonstrate the ``TuneAxis()`` support that provides custom alignment
of a signal against an axis.
Follow here: https://github.com/BCDA-APS/apstools/blob/master/docs/source/resources/demo_tuneaxis.ipynb


.. _example_source:

Source Code Documentation
+++++++++++++++++++++++++

.. automodule:: apstools.examples
    :members:

.. _examples_downloads:

Downloads
+++++++++

The jupyter notebooks and files related to this section may be downloaded from the following table.

* :download:`plan_catalog.txt <resources/plan_catalog.txt>`
* jupyter notebook: :download:`demo_excel_scan <resources/excel_scan.ipynb>`

  * :download:`sample_example.xlsx <resources/sample_example.xlsx>`

* jupyter notebook: :download:`demo_nscan <resources/demo_nscan.ipynb>`
* jupyter notebook: :download:`demo_tuneaxis <resources/demo_tuneaxis.ipynb>`
* jupyter notebook: :download:`demo_specfile_example <resources/demo_specfile_example.ipynb>`

  * :download:`spec1.dat <resources/spec1.dat>`
  * :download:`spec2.dat <resources/spec2.dat>`
  * :download:`spec3.dat <resources/spec3.dat>`
  * :download:`spec_tunes.dat <resources/spec_tunes.dat>`
  * :download:`test_specdata.txt <resources/test_specdata.txt>`
