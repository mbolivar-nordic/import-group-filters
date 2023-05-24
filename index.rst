``import-group-filters`` discussion
###################################

This document contains various west manifest examples using
``import-group-filters: false``. The goal is to decide the desired
behavior.

The examples are not full manifest files. For example, project
``url`` values may be missing. This is for brevity; the meaning
of the manifests should still be clear.

Case 1
******

.. code-block:: yaml

   # manifest-repo/west.yml
   manifest:
     group-filter: [-disabled]
     import-group-filters: false
     projects:
       - name: project-1
         import: true

.. code-block:: yaml

   # project-1/west.yml
   manifest:
     group-filter: [-foo]
     import-group-filters: false
     projects:
       - name: project-2
         url: ignored
         groups: [disabled]
         import: true
       - name: project-3
         url: ignored
         groups: [foo]

============    ========================  ========  ==============================
Project         Defined in                Active?   Notes
============    ========================  ========  ==============================
project-1       manifest-repo/west.yml    Y         No groups
project-2       project-1/west.yml        N         Main manifest disables group
                                                    'disabled'
project-3       project-1/west.yml        TBD       decision needed
============    ========================  ========  ==============================
