==============================================
PyUNLocBoX: Optimization by Proximal Splitting
==============================================

The PyUNLocBoX is a Python package which uses
`proximal splitting methods <https://en.wikipedia.org/wiki/Proximal_gradient_method>`_
to solve non-differentiable convex optimization problems.
The documentation is available on
`Read the Docs <https://pyunlocbox.readthedocs.io>`_
and development takes place on
`GitHub <https://github.com/epfl-lts2/pyunlocbox>`_.
A (mostly unmaintained) `Matlab version <https://epfl-lts2.github.io/unlocbox-html>`_ exists.

+-----------------------------------+
| |doc|  |pypi|  |conda|  |binder|  |
+-----------------------------------+
| |zenodo|  |license|  |pyversions| |
+-----------------------------------+
| |travis|  |coveralls|  |github|   |
+-----------------------------------+

.. |doc| image:: https://readthedocs.org/projects/pyunlocbox/badge/?version=latest
   :target: https://pyunlocbox.readthedocs.io
.. |pypi| image:: https://img.shields.io/pypi/v/pyunlocbox.svg
   :target: https://pypi.org/project/pyunlocbox
.. |zenodo| image:: https://zenodo.org/badge/DOI/10.5281/zenodo.1199081.svg
   :target: https://doi.org/10.5281/zenodo.1199081
.. |license| image:: https://img.shields.io/pypi/l/pyunlocbox.svg
   :target: https://github.com/epfl-lts2/pyunlocbox/blob/master/LICENSE.txt
.. |pyversions| image:: https://img.shields.io/pypi/pyversions/pyunlocbox.svg
   :target: https://pypi.org/project/pyunlocbox
.. |travis| image:: https://img.shields.io/travis/com/epfl-lts2/pyunlocbox.svg
   :target: https://app.travis-ci.com/github/epfl-lts2/pyunlocbox
.. |coveralls| image:: https://img.shields.io/coveralls/github/epfl-lts2/pyunlocbox.svg
   :target: https://coveralls.io/github/epfl-lts2/pyunlocbox
.. |github| image:: https://img.shields.io/github/stars/epfl-lts2/pyunlocbox.svg?style=social
   :target: https://github.com/epfl-lts2/pyunlocbox
.. |binder| image:: https://static.mybinder.org/badge_logo.svg
   :target: https://mybinder.org/v2/gh/epfl-lts2/pyunlocbox/master?urlpath=lab/tree/examples/playground.ipynb
.. |conda| image:: https://img.shields.io/conda/vn/conda-forge/pyunlocbox.svg
   :target: https://anaconda.org/conda-forge/pyunlocbox

The package is designed to be easy to use while allowing any advanced tasks. It
is not meant to be a black-box optimization tool. You'll have to carefully
design your solver. In exchange you'll get full control of what the package
does for you, without the pain of rewriting the proximity operators and the
solvers and with the added benefit of tested algorithms. With this package, you
can focus on your problem and the best way to solve it rather that the details
of the algorithms.

Content
-------

The following solvers are included:

* Gradient descent
* Forward-backward proximal splitting (FISTA and ISTA)
* Generalized forward-backward proximal splitting
* Douglas-Rachford proximal splitting
* Monotone+Lipschitz forward-backward-forward primal-dual
* Projection-based primal-dual

The following acceleration schemes are included:

* Backtracking acceleration based on a quadratic approximation of the objective
* FISTA acceleration for forward-backward solvers
* FISTA acceleration with backtracking for forward-backward solvers
* Regularized nonlinear acceleration (RNA) for gradient descent

To compose your objective, the following functions are included:

* L1-norm (eval, prox)
* L2-norm (eval, prox, grad)
* Nuclear-norm (eval, prox)
* TV-norm (eval, prox)
* Projection on the positive octant (eval, prox)
* Projection on the L2-ball (eval, prox)
* Structured sparsity (eval, prox)

Alternatively, you can easily define a custom function by implementing an
evaluation method and a proximal operator or gradient method:

>>> from pyunlocbox import functions
>>> class myfunc(functions.func):
...     def _eval(self, x):
...         return 0  # Function evaluated at x.
...     def _grad(self, x):
...         return x  # Gradient evaluated at x, if available.
...     def _prox(self, x, T):
...         return x  # Proximal operator evaluated at x, if available.

Likewise, custom solvers are defined by inheriting from ``solvers.solver``
and implementing ``_pre``, ``_algo``, and ``_post``.
Custom acceleration schemes are defined by inheriting from
``acceleration.accel`` and implementing ``_pre``, ``_update_step``,
``_update_sol``, and ``_post``.

Usage
-----

Following is a typical usage example that solves an optimization problem
composed by the sum of two convex functions. The functions and solver objects
are first instantiated with the desired parameters. The problem is then solved
by a call to the solving function.

>>> from pyunlocbox import functions, solvers
>>> f1 = functions.norm_l2(y=[4, 5, 6, 7])
>>> f2 = functions.dummy()
>>> solver = solvers.forward_backward()
>>> ret = solvers.solve([f1, f2], [0., 0, 0, 0], solver, atol=1e-5)
Solution found after 9 iterations:
    objective function f(sol) = 6.714385e-08
    stopping criterion: ATOL
>>> ret['sol']
array([3.99990766, 4.99988458, 5.99986149, 6.99983841])

You can
`try it online <https://mybinder.org/v2/gh/epfl-lts2/pyunlocbox/master?urlpath=lab/tree/examples/playground.ipynb>`_,
look at the
`tutorials <https://pyunlocbox.readthedocs.io/en/stable/tutorials/index.html>`_
to learn how to use it, or look at the
`reference guide <https://pyunlocbox.readthedocs.io/en/stable/reference/index.html>`_
for an exhaustive documentation of the API. Enjoy!

Installation
------------

The PyUNLocBoX is available on PyPI::

    $ pip install pyunlocbox

The PyUNLocBoX is available on `conda-forge <https://github.com/conda-forge/pyunlocbox-feedstock>`_::

    $ conda install -c conda-forge pyunlocbox

Contributing
------------

See the guidelines for contributing in ``CONTRIBUTING.rst``.

Acknowledgments
---------------

The PyUNLocBoX was started in 2014 as an academic open-source project for
research purpose at the `EPFL LTS2 laboratory <https://lts2.epfl.ch>`_.

It is released under the terms of the BSD 3-Clause license.

If you are using the library for your research, for the sake of
reproducibility, please cite the version you used as indexed by
`Zenodo <https://doi.org/10.5281/zenodo.1199081>`_.
Or cite the generic concept as::

    @misc{pyunlocbox,
      title = {PyUNLocBoX: Optimization by Proximal Splitting},
      author = {Defferrard, Micha\"el and Pena, Rodrigo and Perraudin, Nathana\"el},
      doi = {10.5281/zenodo.1199081},
      url = {https://github.com/epfl-lts2/pyunlocbox/},
    }
