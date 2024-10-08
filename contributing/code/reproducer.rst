Creating a Bug Reproducer
=========================

The main Symfony code repository receives thousands of issues reports per year.
Some of those issues are easy to understand and can
be fixed without any other information. However, other issues are much harder to
understand because developers can't reproduce them in their computers. That's
when we'll ask you to create a "bug reproducer", which is the minimum amount of
code needed to make the bug appear when executed.

Reproducing Simple Bugs
-----------------------

If you are reporting a bug related to some Symfony component used outside the
Symfony framework, it's enough to share a small PHP script that when executed
shows the bug::

    // First, run "composer require symfony/validator"
    // Then, execute this file:
    <?php
    require_once __DIR__.'/vendor/autoload.php';
    use Symfony\Component\Validator\Constraints;

    $wrongUrl = 'http://example.com/exploit.html?<script>alert(1);</script>';
    $urlValidator = new Constraints\UrlValidator();
    $urlConstraint = new Constraints\Url();

    // The URL is wrong, so var_dump() should display an error, but it displays
    // "null" instead because there is no context to build a validator violation
    var_dump($urlValidator->validate($wrongUrl, $urlConstraint));

Reproducing Complex Bugs
------------------------

If the bug is related to the Symfony Framework or if it's too complex to create
a PHP script, it's better to reproduce the bug by creating a new project. To do so:

#. Create a new project:

.. code-block:: terminal

    $ composer create-project symfony/skeleton bug_app

#. Add and commit the changes generated by Symfony.
#. Now you must add the minimum amount of code to reproduce the bug. This is the
   trickiest part and it's explained a bit more later.
#. Add and commit your changes.
#. Create a `new repository`_ on GitHub (give it any name).
#. Follow the instructions on GitHub to add the ``origin`` remote to your local project
   and push it.
#. Add a comment in your original issue report to share the URL of your forked
   project (e.g. ``https://github.com/YOUR-GITHUB-USERNAME/symfony_issue_23567``)
   and, if necessary, explain the steps to reproduce (e.g. "browse this URL",
   "fill in this data in the form and submit it", etc.)

Adding the Minimum Amount of Code Possible
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The key to create a bug reproducer is to solely focus on the feature that you
suspect is failing. For example, imagine that you suspect that the bug is related
to a route definition. Then, after creating your project:

#. Don't edit any of the default Symfony configuration options.
#. Don't copy your original application code and don't use the same structure
   of controllers, actions, etc. as in your original application.
#. Create a small controller and add your routing definition that shows the bug.
#. Don't create or modify any other file.
#. Install the :doc:`local web server </setup/symfony_server>` provided by Symfony
   and use the ``symfony server:start`` command to browse to the new route and
   see if the bug appears or not.
#. If you can see the bug, you're done and you can already share the code with us.
#. If you can't see the bug, you must keep making small changes. For example, if
   your original route was defined using XML, forget about the previous route
   and define the route using XML instead. Or maybe your application
   registers some event listeners and that's where the real bug is. In that case,
   add an event listener that's similar to your real app to see if you can find
   the bug.

In short, the idea is to keep adding small and incremental changes to a new project
until you can reproduce the bug.

.. _`new repository`: https://github.com/new
