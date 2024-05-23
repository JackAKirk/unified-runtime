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
Instead of using a fixed set of kernel enqueue arguments, the approach is to introduce the
`exp_kernel_launch_desc_t` descriptor and associated properties that enables a more flexible approach.
The `exp_kernel_launch_desc_t` descriptor points to a linked-list of specific kernel launch properties.
One new function is introduced: `urEnqueueKernelLaunchCustomExp`. A
`exp_kernel_launch_desc_t` pointer can be passed to  `urEnqueueKernelLaunchCustomExp` as an argument,
which then launches a native kernel using the list of launch properties associated with the descriptor.
`urEnqueueKernelLaunchCustomExp` corresponds closely to the CUDA Driver API
`cuLaunchKernelEx`.

Many kernel lauch properties can be supported, such as cooperative kernel launches. As such,
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
    ${X}_STRUCTURE_TYPE_EXP_KERNEL_LAUNCH_DESC


Types
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* ${x}_exp_launch_properties_cluster_dims_t
* ${x}_exp_launch_properties_cooperative_t
* ${x}_exp_kernel_launch_desc_t

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
