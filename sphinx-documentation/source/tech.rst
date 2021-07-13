Technology Stack
================


Stack Diagram
-------------

click nodes to visit links in new tabs!!

.. graphviz::

   digraph techstack {

      node [shape = "component"];

      caller [label = "Scheduled API calls", href="https://www.digitalocean.com/community/tutorials/how-to-use-web-apis-in-python-3", target="_blank"];
      redditAPIRules [label="Reddit API Rules", href="https://github.com/reddit-archive/reddit/wiki/API" target="_blank"];
      production [label="Production"];
      testing [label="Testing", href="https://docs.djangoproject.com/en/3.2/topics/testing/", target="_blank"];
      processor [label = "Process API data"];

      node [label = "technology", style="filled" fillcolor="#1f0477", shape="cylinder", fontcolor="white"];

      reddit [label="Reddit API", href="https://www.reddit.com/dev/api" target="_blank" alt="Reddit API documentation"];
      django [label="Django", href="https://docs.djangoproject.com/en/3.2/", target="_blank"];
      sqlite [label="SQLite", href="https://www.sqlite.org/index.html", target="_blank" fill="blue"];
      apache [label="Apache", href="https://httpd.apache.org/" target="_blank"];
      modwsgi [label="mod_wsgi", href="https://modwsgi.readthedocs.io/en/develop/" target="_blank"];
      autodoc [label="sphinx-autodoc", href="https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html", target="_blank"];
      sphinx [label="sphinx documentation", href="https://www.sphinx-doc.org",  target="_blank"];
      html [label="HTML/CSS"];
      github [label="github pages"];

      node [shape="ellipse", style="filled", fillcolor="#e5e6ce", fontcolor="black"];

      models [label="Models", href="https://docs.djangoproject.com/en/3.2/topics/db/models/", target="_blank"];
      database [label="Database"];
      view [label="View", href="https://docs.djangoproject.com/en/3.2/#the-view-layer", target="_blank"];
      template [label="Template", href="https://docs.djangoproject.com/en/3.2/topics/templates/", target="_blank"];

      codebase [label="Code Base", href="https://www.github.com/unhm-programming-team" target="_blank"];

      reddit -> redditAPIRules;

      database -> models [label="populates"];
      database -> sqlite;
      template -> html;
      django -> view
      django -> models
      django -> database
      django -> template

      view -> template [label = "serves"];
      template -> html [label = "generates"];
      models -> template [label = "populates"];
      caller -> reddit [label="queries"];
      caller -> processor [label="processes"];
      processor -> database [label="fills"];

      codebase -> autodoc;
      autodoc -> sphinx [label="made with"];
      production -> apache [label="backend"];
      production -> modwsgi [label="python/apache"];
      codebase -> production;
      codebase -> testing;
      codebase -> django [label="mostly written using"];

      sphinx -> github [label="hosted statically"];

   }






.. graphviz::

   digraph legend {
      fillcolor="gray";
      rankdir=UD;

      label = "Legend";
      shape = "rectangle";
      color = "black";

      technology [label = "technology", style="filled" fillcolor="#1f0477", shape="cylinder", fontcolor="white"];
      ourcode [label="our code", shape="ellipse", style="filled", fillcolor="#e5e6ce"];
      process [shape="component", label="process"];
   }


Description
-----------

`Django <https://www.djangoproject.com/start/>`_ is proposed to be the main driver of this web-application. Most of our members are familiar with Python which is why we should try a Python based stack. `SQLite <https://www.sqlite.org/index.html>`_ is proposed to be the database because it runs out of the box with Django, though Django offers support for other dbs as well, including `PostgreSQL <https://www.postgresql.org/>`_ through `psycopg <https://www.psycopg.org/>`_. `Sphinx <https://www.sphinx-doc.org/en/master/>`_ makes decent documentation. Django has `some built in testing capabilities <https://docs.djangoproject.com/en/3.2/topics/testing/>`_.

`Hosting with Apache and mod_wsgi <https://docs.djangoproject.com/en/3.2/howto/deployment/wsgi/modwsgi/>`_ is a tried and tested deployment strategy.