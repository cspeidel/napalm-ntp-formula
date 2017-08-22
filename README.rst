==================
napalm-ntp-formula
==================

Salt formula to manage the NTP configuration on network devices, managed via NAPALM.

Available states
================

.. contents::
    :local:

``netconfig``
-------------

Generate the configuration using Jinja templates and load the rendered configuration on the network device. The
templates are pre-written for several operating systems:

- Junos
- IOS-XR
- EOS

If you have a different operating system not covered yet, please submit a PR to add it.

Pillar
======

The pillar has the same structure in both cases, following the hierarchy of the
`openconfig-system YANG model<http://ops.openconfig.net/branches/master/openconfig-system.html>`_, e.g.:

.. code-block:: yaml

    openconfig-system:
      system:
        ntp:
          servers:
            server:
              172.17.19.1:
                config:
                  association_type: SERVER
                  prefer: true
              172.17.19.2:
                config:
                  association_type: PEER

Usage
=====

After configuring the pillar data (and refresh it to the minions, i.e. ``$ sudo salt '*' saltutil.refresh_pillar``),
you can run this formula:

.. code-block:: bash

    $ sudo salt '*' state.sls ntp.netconfig

Output Example:

.. code-block:: bash

    $ sudo salt vmx1 state.sls ntp.netconfig
    vmx1:
    ----------
              ID: oc_ntp_netconfig
        Function: netconfig.managed
          Result: True
         Comment: Configuration changed!
         Started: 14:43:55.454470
        Duration: 3884.221 ms
         Changes:
                  ----------
                  diff:
                      [edit system]
                      +   ntp {
                      +       server 172.17.19.1;
                      +       peer 172.17.19.2;
                      +   }

    Summary for vmx1
    ------------
    Succeeded: 1 (changed=1)
    Failed:    0
    ------------
    Total states run:     1
    Total run time:   3.884 s

