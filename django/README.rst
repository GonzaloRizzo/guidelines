Django
======

**`⬅ back to index <./>`__**

Table of Contents
-----------------

1. `Read <#Read>`__
2. `Files <#Files>`__
3. `Templates <#Templates>`__
4. `Signals <#Signals>`__
5. `Books <#Books>`__

Read
----

`Django Two Scoops <http://twoscoopspress.org/>`__, Now.

Files
-----

.. code:: yaml

    py: lowercase_with_underscores.py
    html: lowercase_with_underscores.html
    javascript: lowercase-with-dashes.js
    images: lowercase-with-dashes.*
    css: lowercase-with-dashes.*
    scss: lowercase-with-dashes.*
    scss (partials): _lowercase-with-dashes.*

**`⬆ back to top <#table-of-contents>`__**

Templates
---------

-  Name blocks with lowercase and underscores.

.. code:: html

    {% block lowercase_with_underscores %}
    {% endbblock lowercase_with_underscore %}

-  If the block/endblock is inline you should avoid the name on the
   endblock.

-  In endblocks, add the name of the block they close.

.. code:: html

    {% block foo_bar %}
        ...
    {% endblock foo_bar %}

-  Indent everything within template tags.

.. code:: html

    {% block foo_bar %}
        <html-tag></html-tag>
        {% if foo %}
            <html-tag></html-tag>
        {% else %}
            <html-tag></html-tag>
        {% endif %}
    {% endblock foo_bar %}

**`⬆ back to top <#table-of-contents>`__**

Signals
-------

.. code:: bash

    app/signals/__init__.py # Define new signals
    app/signals/handlers.py # Define handlers

.. code:: python

    from django.apps import AppConfig as BaseAppConfig


    class AppConfig(BaseAppConfig):

        ...

        def ready(self):
            import app.signals.handlers  # noqa

**`⬆ back to top <#table-of-contents>`__**

Books
-----

-  `Django Two Scoops <http://twoscoopspress.org/>`__

**`⬆ back to top <#table-of-contents>`__**
