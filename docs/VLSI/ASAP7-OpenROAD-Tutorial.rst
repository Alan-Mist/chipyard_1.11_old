.. _asap7-openroad-tutorial:

ASAP7 + OpenROAD Tutorial
=========================

This tutorial provides an open-source-friendly VLSI flow in Chipyard 1.11.0 by combining:

* the built-in ASAP7 technology plugin (7nm, <=45nm as requested), and
* the OpenROAD + Yosys tool plugins.

The goal is to mirror the Sky130 + OpenROAD teaching flow while keeping a smaller, reproducible setup for students who want synthesis and place-and-route QoR (timing/area/power-style report) data.

Files Added for this Tutorial
-----------------------------

This flow is selected with ``tutorial=asap7-openroad`` and uses:

* ``vlsi/tutorial.mk`` (new tutorial target)
* ``vlsi/example-asap7.yml`` (technology + baseline physical constraints)
* ``vlsi/example-openroad.yml`` (open-source tool plugin selection)
* ``vlsi/example-designs/asap7-openroad.yml`` (OpenROAD-focused design overrides)

Prerequisites
-------------

* Python 3.9+
* Chipyard 1.11.0 dependencies and conda environment
* ASAP7 PDK files (`ASAP7 plugin README <https://github.com/ucb-bar/hammer/blob/master/hammer/technology/asap7>`__)
* OpenROAD and Yosys plugins (fetched by ``init-vlsi.sh``)

Initial Setup
-------------

In the Chipyard root:

.. code-block:: shell

    ./scripts/init-vlsi.sh asap7 openroad

Then go to the VLSI directory:

.. code-block:: shell

    cd vlsi

Set your ASAP7 path in ``example-asap7.yml``:

.. code-block:: yaml

    technology.asap7.tarball_dir: "/absolute/path/to/asap7"

If needed, also set:

.. code-block:: yaml

    technology.asap7.pdk_install_dir: "/path/to/asap7PDK_r1p7"
    technology.asap7.stdcell_install_dir: "/path/to/asap7sc7p5t_27"

Run the Flow
------------

This tutorial supports the exact command pattern used by other Chipyard tutorials.

Generate build targets:

.. code-block:: shell

    make buildfile tutorial=asap7-openroad CONFIG=TinyRocketConfig

Run synthesis:

.. code-block:: shell

    make syn tutorial=asap7-openroad CONFIG=TinyRocketConfig

Run place-and-route:

.. code-block:: shell

    make par tutorial=asap7-openroad CONFIG=TinyRocketConfig

Where to Read PPA/QoR Data
--------------------------

After ``syn``:

* ``build-asap7-openroad/.../syn-rundir/reports`` for synthesis timing/area reports.

After ``par``:

* ``build-asap7-openroad/.../par-rundir/reports`` for OpenROAD timing and physical QoR summaries.
* ``build-asap7-openroad/.../par-rundir`` for DEF/GDS/netlist outputs and step-by-step logs.

Notes for Students
------------------

* The ``example-designs/asap7-openroad.yml`` file intentionally relaxes clock target and adjusts OpenROAD knobs for better convergence on commodity machines.
* This teaching flow focuses on open-source synthesis/PnR. DRC/LVS in ASAP7 generally requires additional foundry/commercial collateral beyond basic OpenROAD use.
* If you switch between tutorial presets, run ``make buildfile -B ...`` to force regeneration of Hammer build targets.
