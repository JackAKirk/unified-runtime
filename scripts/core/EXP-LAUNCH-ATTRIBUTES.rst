<%
    OneApi=tags['$OneApi']
    x=tags['$x']
    X=x.upper()
%>

.. _experimental-launch-properties:

================================================================================
LAUNCH Properties
================================================================================

.. warning::

    Experimental features:

    *   May be replaced, updated, or removed at any time.
    *   Do not require maintaining API/ABI stability of their own additions over
        time.
    *   Do not require conformance testing of their own additions.


Terminology
--------------------------------------------------------------------------------
"Launch Properties" is used to indicate optional kernel launch properties that can
be specified at the time of a kernel launch. Such properties can be used to
enable hardware specific kernel features.

Motivation
--------------------------------------------------------------------------------
Advances in hardware sometimes require new kernel properties. One example is
distributed shared memory as used by Nvidia Hopper GPUs. Launching a kernel
that supports distributed shared memory requires specifying a thread cluster
dimension over which the shared memory is accessible. Additionally some
applications require specification of kernel properties at launch-time.

This extension is a future-proof and portable solution that supports these two requirements.
Instead of using a fixed set of kernel enqueue arguments, the approach is to introduce new
property types, such as `ur_exp_properties_dim3_t` which can be used to specify
the cluster dimensions,
that can be passed via a descriptor to a new custom kernel enqueue function
`urEnqueueKernelLaunchCustomExp`. `urEnqueueKernelLaunchCustomExp` launches
a kernel using the list of launch properties associated with the descriptor.
`urEnqueueKernelLaunchCustomExp` corresponds closely to the CUDA Driver API
`cuLaunchKernelEx`.

Many kernel launch properties can be supported, such as cooperative kernel launches. As such,
eventually this extension should be able to replace the cooperative kernels
UR extension.

API
--------------------------------------------------------------------------------

Macros
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* ${X}_LAUNCH_PROPERTIES_EXTENSION_STRING_EXP

Enums
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* ${x}_structure_type_t
    ${X}_STRUCTURE_TYPE_EXP_LAUNCH_PROPERTIES_CLUSTER_DIMS
    ${X}_STRUCTURE_TYPE_EXP_LAUNCH_PROPERTIES_COOPERATIVE
    ${X}_STRUCTURE_TYPE_EXP_BASE_DESC


Types
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* ${x}_exp_properties_dim3_t
* ${x}_exp_properties_bool_t

Functions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* ${x}EnqueueKernelLaunchCustomExp

Support
--------------------------------------------------------------------------------

Adapters which support this experimental feature *must* return the valid string
defined in ``${X}_LAUNCH_PROPERTIES_EXTENSION_STRING_EXP`` as one of the options from
${x}DeviceGetInfo when querying for ${X}_DEVICE_INFO_EXTENSIONS.

Changelog
--------------------------------------------------------------------------------

+-----------+---------------------------------------------+
| Revision  | Changes                                     |
+===========+=============================================+
| 1.0       | Initial Draft                               |
+-----------+---------------------------------------------+

Contributors
--------------------------------------------------------------------------------

* JackAKirk `jack.kirk@codeplay.com <jack.kirk@codeplay.com>`_
