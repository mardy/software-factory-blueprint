:orphan:

Licensing
=========

Each source code file needs to mention which license applies and who owns
the copyright in the file header. The purpose of this is to make it
absolutely clear, for both humans and tooling, which license applies to
that particular file even when the file is moved to another project. In
order to achieve what is stated above, the header should include:

* A copyright statement
* A license header
* An SPDX tag

Examples of this setup for different programming languages can be found here:
https://github.com/Pelagicore/OpenSourceTemplates/tree/master/examples

The rationale behind using both a license header and an SPDX tag is to
make it readable for humans that do not know the abbreviations of
licenses as well as making it explicit enough to support compliance
tooling in a future proof way. As SPDX is the licensing standard that
compliance tooling will be using in the future, and indeed already is
used by tool makers such as WindRiver and Black Duck, they are included
both for support for tooling and future proofing the format.

Excluding incompatible licences
===============================

Business reasons sometimes impose restrictions on the licensing and require that the built image
contains no software distributed under a specific license. Yocto provides a simple way to enforce
this, by means of the ``INCOMPATIBLE_LICENCE`` variable which can be added to your image's recipe:

.. code-block:: none

    # A list of licenses, named according to the SPDX standard
    INCOMPATIBLE_LICENSE = "GPL-3.0 LGPL-3.0 AGPL-3.0 MPL-1.1"

This will exclude all recipes carrying software with these licenses; if other recipes depend on
these packages, they will also be excluded from the image. In case the image cannot be built due to
unsatisfied dependencies of some required feature or package group, you will get an error from
bitbake.

The problem can be solved either by explicitly removing the unsatisfied feature or package group,
like this:

.. code-block:: none

    IMAGE_INSTALL_remove = "packagegroup-bistro-utils"
    IMAGE_FEATURES_remove = "tools-debug"

or by adding a layer which would provide equivalent packages, licensed under a compatible license.
For replacing the packages using the GPL-3.0, Yocto offers a ``meta-gplv2`` [#metagplv2]_ layer with older versions
of many packages, still licensed under the GPL-2.0.

.. [#metagplv2] https://layers.openembedded.org/layerindex/branch/master/layer/meta-gplv2/

.. tags:: process
