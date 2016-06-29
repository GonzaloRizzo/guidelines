Table of Contents
=================

1. `Books`_
2. `Files`_
3. `Templates`_
4. `Signals`_


Books
=====

#. `Django Two Scoops <http://twoscoopspress.org/>`__


Files
=====

.. code:: yaml

    py: lowercase_with_underscores.py
    html: lowercase_with_underscores.html
    javascript: lowercase-with-dashes.js
    images: lowercase-with-dashes.*
    css: lowercase-with-dashes.*
    scss: lowercase-with-dashes.*
    scss (partials): _lowercase-with-dashes.*


Templates
=========

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


Signals
=======

.. code:: bash

    app/signals/__init__.py # Define new signals
    app/signals/handlers.py # Define handlers

.. code:: python

    from django.apps import AppConfig as BaseAppConfig


    class AppConfig(BaseAppConfig):

        ...

        def ready(self):


Tests
=====

TDD - Introduction
==================

First off, suppose you were required to create an app that should register user
activities and then show if the activity was done on the current week.

In order to do that you decide to create a new model. Where should you start?
well you could start by creating the activity class, defining its fields and
methods, documenting it, integrate it with the rest of the app, clicking around to
make sure everything works and then write tests as an after thought.

There are a couple of problems with that workflow, the main one is that creating
the tests after the functionality will make you adapt your tests to the functionality
and not the other way around, so the tests, instead of describing the requirements
will describe the already implemented functionality (which can be wrong).

Also, by writing tests firsts, you'll have a clear definition of the required public
interface, the client requirements and a clear ending point of the development process.
Once your test suit passes, you've successfully implemented the requirements, of course
this doesn't necessarily means you are done, refactor is a key element in the development
of any kind of software.


TDD - Unit tests
================

How do we use TDD in Python?

First off, we will start by designing a simple class that will do simple stuff
just by way of example.


.. code:: python


    from django.db import models
    from django.contrib.auth.models import User


    class Activity(models.Model):

        user = models.ForeignKey(User)
        start_date = models.DateField()
        end_date = models.DateField()


        def is_next_week(self):
            return True


This method called :code:`is_next_week`, is a method of activity because as
an example, we had the requirement to list all the activities that were next
week. Now, this is the test class that tests this method and its base scenarios.


.. code:: python

    import datetime

    from django.test import TestCase
    from django.contrib.auth.models import User

    from activities.models import Activity

    class ActivityTestCase(TestCase):

        @classmethod
        def setUp(self):
            super().setUpClass()
            self.user = User.objects.create_user(
                'admin',
                'admin@example.com',
                'examplepass'
            )
            self.this_week = Activity.objects.create(
                user=User.objects.first(),
                start_date=datetime.date.today(),
                end_date=(datetime.date.today() + datetime.timedelta(days=1))
            )

        def test_is_next_week(self):
            self.assertTrue(True)

        def test_is_next_week_passed_next_week(self):
            self.assertTrue(True)

        def test_is_next_week_this_week(self):
            self.assertTrue(True)


The base scenarios are as you can see if it starts in fact next week, if its
this week, or if it is passed next week.

The :code:`setUp` method is a method that we build just because when using
Django, it automatically builds an independent db for the tests which is empty,
so we will have to populate it with something; due to how our models are made,
this is the minimum data we will need to test the method, an activity and a
User.

Now, as this test passes because it is just asserting True to True, we can make
this test case richer.


.. code:: python

    # Rest of the code stays the same.
    def test_is_next_week(self):
        activity = self.this_week
        self.assertTrue(activity.is_next_week())

    # The two other tests change exactly as this one

This test case is richer because its mostly finished, because from now on its
changes will be pretty simple for this example. After making sure this passes by
running the tests, it is time to get to the code, and do it the simplest way we
can. This will be:


.. code:: python

    import datetime

    # (...) rest of code stays the same

    def is_next_week(self):
        # we need to figure out which is the next monday
        next_monday = datetime.date.today()
        while next_monday.weekday() != 0:
            next_monday += datetime.timedelta(1)
        return self.start_date >= next_monday and \
               self.start_date <= (next_monday + datetime.timedelta(7))


The simplest way to see if an activity starts on next week, is by finding out
which is the next monday, and after that, check if the start day is between next
monday and next sunday, if that is true, then the activity starts next week. Now
if you run the test, they will fail, because of the data we entered, and so we
will need to modify the data that we entered in order to make this three test
cases useful, and also the methods to call the correct activity:


.. code:: python

    @classmethod
    def setUp(self):
        super().setUpClass()
        self.user = User.objects.create_user(
            'admin',
            'admin@example.com',
             'examplepass'
        )
        today = datetime.date.today()
        if today.weekday() == 0:
            today += datetime.timedelta(7)
        else:
            today += datetime.timedelta(6)
        self.next_week = Activity.objects.create(
            user=User.objects.first(),
            start_date=today,
            end_date=(today + datetime.timedelta(days=1))
        )
        self.passed_next_week = Activity.objects.create(
            user=User.objects.first(),
            start_date=datetime.date.today() + datetime.timedelta(15),
            end_date=datetime.date.today() + datetime.timedelta(16)
        )
        self.this_week = Activity.objects.create(
            user=User.objects.first(),
            start_date=datetime.date.today(),
            end_date=(datetime.date.today() + datetime.timedelta(days=1))
        )


    def test_is_next_week(self):
        activity = self.next_week
        self.assertTrue(activity.is_next_week())

    def test_is_next_week_passed_next_week(self):
        activity = self.passed_next_week
        self.assertFalse(activity.is_next_week())

    def test_is_next_week_this_week(self):
        activity = self.this_week
        self.assertFalse(activity.is_next_week())


Note: there is still one scenario we are not contemplating, and that would be if
you run this tests on Monday, because it will find next Monday as todays, which
is a validation that follows the same process that we have just described.

This way, the three tests pass and we have ended the round of tdd testing.
What comes next? We assumed that this dates came with the right format, etc. Now
we will need to make sure that happens, but as this is just an example, that is
left for the reader as an exercise.

If you want to see how we do tests, please click here_.

.. _here: https://github.com/sophilabs/guidelines/tree/master/python#tdd-unit-tests
