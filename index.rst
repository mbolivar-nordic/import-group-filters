``import-group-filters`` discussion
###################################

This document contains various west manifest examples using
``import-group-filters: false``. The goal is to decide the desired
behavior.

The examples are not full manifest files. For example, project ``url`` values
are missing. This is for brevity, and the meaning should be clear.

Assume ``manifest.group-filter`` is unset in all cases.

<template>
**********

.. code-block:: yaml
   :linenos:

   # A/west.yml
   manifest:
     import-group-filters: <true|false>
     group-filter: [-G1,-G2,-G3]
     projects:
       - name: N
         import: true
       - name: PA1
         groups: [G1]
       - name: PA2
         groups: [G5]
     self:
       import:
         - self-import-a.yml
         - self-import-b.yml

.. code-block:: yaml
   :linenos:

   # A/self-import-a.yml
   manifest:
     group-filter: [+G2,-GSA]
     projects:
       - name: PSA1
         groups: [G2]
       - name: PSA2
         groups: [G3]

.. code-block:: yaml
   :linenos:

   # A/self-import-b.yml
   manifest:
     projects:
       - name: PSB1
         groups: [GSA]

.. code-block:: yaml
   :linenos:

   # N/west.yml
   manifest:
     import-group-filters: <true|false>
     group-filter: [-GH,-G4]
     projects:
       - name: Z
         import: true
       - name: H
         groups: [GH]
       - name: PN1
         groups: [G2]
       - name: PN2
         groups: [G5]

.. code-block:: yaml
   :linenos:

   # Z/west.yml
   manifest:
     import-group-filters: true
     group-filter: [-G5]
     projects:
       - name: PZ1
         groups: [G3]
       - name: PZ2
         groups: [G4]
       - name: PZ3
         groups: [G5]

Bsim from ncs-example-application
*********************************

This example is meant to validate the above decisions using the real-world use
case of a user basing their sample off of ncs-example-application. ``B`` refers
to bsim.

.. code-block:: yaml
   :linenos:

   # A/west.yml
   manifest:
     projects:
       - name: N
         import: true

.. code-block:: yaml
   :linenos:

   # N/west.yml
   manifest:
     projects:
       - name: Z
         import: true

.. code-block:: yaml
   :linenos:

   # Z/west.yml
   manifest:
     import-group-filters: false
     group-filter: [-babblesim]
     projects:
       - name: bsim
         groups: [babblesim]
         import: true

.. code-block:: yaml
   :linenos:

   # bsim/west.yml
   manifest:
     projects:
       - name: babblesim_base
         groups: [babblesim]
       - name: babblesim_ext_2G4_libPhyComv1
         groups: [babblesim]
