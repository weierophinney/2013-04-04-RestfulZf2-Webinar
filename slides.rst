.. role:: white

Build RESTful ZF2 Applications
==============================

.. class:: titleslideinfo

    Matthew Weier O'Phinney

    @mwop

    http://www.mwop.net/

.. Change to a standard page
.. raw:: pdf

    PageBreak twoColumn

Who I am
========

Just this guy:

* Project Lead, Zend Framework
* Open Source enthusiast
* Coffee lover
* Chocolate lover
* Beer lover

.. image:: images/mwop.png
    :align: center
    :width: 94%

.. raw:: pdf

    PageBreak titlePage

.. class:: centredtitle

What do I mean by "REST"?

.. raw:: pdf

    PageBreak standardPage

Richardson Maturity Model
=========================

http://martinfowler.com/articles/richardsonMaturityModel.html

Level 0
=======

**HTTP to tunnel RPC**

.. image:: images/rmm-level0.png

Level 1
=======

**Resources** (multiple endpoints)

.. image:: images/rmm-level1.png


Level 2
=======

**HTTP verbs**

.. image:: images/rmm-level2.png

Level 3
=======

**Hypermedia Controls**

Level 3: Hypermedia Types: Representations
==========================================

.. code-block:: javascript

    // application/vnd.recipes+json
    {
        "id": "identifier",
        "name": "Recipe name",
        "ingredients": [
            // ingredient objects
        ],
        "directions": "Directions for cooking"
    }

Level 3: Linking
================

.. code-block:: javascript

    {
        "_links": {
            "self": {
                "href": "http://example.com/api/recipes/1234"
            },
            "describedby": {
                "href": "http://example.com/api/resources/recipe"
            }
        }
        // ...
    }

.. Talk about other link use cases: pagination, linking to related resources,
.. etc.

Level 3: Embedding
==================

.. code-block:: javascript

    {
        "_embedded": {
            "addresses": [
                {
                    "_links": {"self": {
                        "href": "http://example.com/api/addresses/5678"
                    }},
                    // a representation
                }
            ]
        }
        // ... 
    }

.. raw:: pdf

    PageBreak titlePage

.. class:: centredtitle

Aside: Hypermedia Application Language

.. raw:: pdf

    PageBreak standardPage

Which media type should I use?
==============================

* Vendor-specific? *(e.g., application/vnd.myorg.recipe+json)*
* Fully generic? *(e.g., application/json)*

A happy medium: HAL
===================

**application/hal+json**

* Describes hypermedia links
* Describes how to embed resources, either as parts of other resources or parts
  of collections
* Otherwise retains your object structure

HAL: Resource
=============

.. code-block:: javascript

    {
        "_links": {
            "self": {
                "href": "http://example.com/api/recipes/cacio-e-pepe"
            }
        },
        "id": "cacio-e-pepe",
        "name": "Cacio e Pepe Pasta"
    }

.. Talk about self link as being required

HAL: Embedded resource
======================

.. code-block:: javascript

    {
        "_links": {"self": {"href": "..."}},
        "_embedded": {
            "author": {
                "_links": {"self": {"href": "..."}},
                "id": "mario",
                "name": "Mario Mario"
            }
        }
        // ...
    }

.. Note that the style is name => object, and that embedded resources
.. also have links.


HAL: Collections
================

.. code-block:: javascript

    {
        "_links": {
            "self": {"href": "..."},
            "next": {"href": "..."},
            "prev": {"href": "..."},
            "first": {"href": "..."},
            "last": {"href": "..."}
        },
        // ...
    }

.. Talk about relational links!

HAL: Collections
================

.. code-block:: javascript

    {
        // ...
        "_embedded": {
            "recipes": [
                {
                    "_links": { "self": { "href": "..." } },
                    "id": "cacio-e-pepe",
                    "name": "Cacio e Pepe Pasta"
                },
                // ...
            ]
        },
        "and-other-properties": "if desired"
    }

.. Discuss that this is the same format to use in a single resource as well,
.. when an "object" is actually a collection of objects, such as addresses.

.. raw:: pdf

    PageBreak titlePage

.. class:: centredtitle

Translating the Richardson Maturity Model to ZF2

.. raw:: pdf

    PageBreak standardPage

Areas of Concern
================

* Routing (unique URLs per resource, link generation)
* ``AbstractRestfulController`` (HTTP method negotiation)
* View Models and Renderers (media-type negotiation and resource representations)


