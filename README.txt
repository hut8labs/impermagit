==========
Impermagit
==========

Impermagit makes it easier to test python programs that use /
integrate with git.  For example::

    from impermagit import fleeting_repo

    with fleeting_repo() as repo:
        # there is now a git repo created in a temporary dir
        # somewhere, allowing you to do things like:

        # add some files with some contents and commit them
        repo.commit([('test.txt', 'test contents\n'),
                     ('subdir/test2.txt', 'other contents\n')])

        # update contents
        repo.commit([('test.txt', 'new test contents\n')])

        # git rm files by passing None as contents
        repo.commit([('subdir/test.txt', None)])

        # run arbitrary git cmds in the repo when you don't care
        # about the output (will raise a GitExeException if it
        # fails)
        repo.do_git(["rm", "some-other-file.txt"])

        # run arbitrary git cmds in the repo and get back
        # stdout / stderr.
        with repo.yield_git(["log"]) as (out, err):
            print out.read()
            print err.read()

     # and here, the repo is gone and the temporary dir deleted.

You can also create a Repo object directly, if you'd prefer to control
the lifecycle more closely::

    from imperagit import Repo

    repo = Repo('/some/dir/you/manage')

By default, Imperagit uses "/usr/bin/env git" as the git executable,
but this can be overridden with the git_exe arg to both fleeting_repo
and Repo.

You can see more detailed docs with the interactive help::

    >>> import impermagit
    >>> help(impermagit)

Impermagit requires Python 2.6+, and has not been tested with Python
3.

----------
Installing
----------

Install with pip or easy_install from pypi, or "python setup.py
install" from source if you prefer.
