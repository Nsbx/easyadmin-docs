---
title: How to Manage Configuration for Complex Backends
---

The recommended way to start configuring your backend is to use the
`config/packages/easy_admin.yaml` file and put your configuration under the
`easy_admin` key. However, for medium-sized and large backends this
configuration can be very long and hard to maintain.

## Splitting Configuration into Several Files

If your application keeps growing, it's better to split the configuration into
different files.

Consider an application which defines the following configuration:

``` yaml
# config/packages/easy_admin.yaml
easy_admin:
    site_name: '...'
    # ...
    design:
        # ...
    entities:
        Product:
            # ...
        User:
            # ...
        Category:
            # ...
        # ...
```

This configuration is going to be divided into four different files:

- `design.yaml` for design related configuration;
- `product.yaml` for the configuration related to `Product` entity;
- `user.yaml` for the configuration related to `User` entity;
- `basic.yaml` for the rest of the configuration, including any entity
  different from `Product` and `User`.

First, create a new `config/packages/easy_admin/` directory to store the new
files so they don't mess with the other Symfony configuration files. Then,
create the four files with these contents:

``` yaml
# config/packages/easy_admin/basic.yaml
easy_admin:
    site_name: '...'
    # ...

# config/packages/easy_admin/design.yaml
easy_admin:
    design:
        # ...

# config/packages/easy_admin/product.yaml
easy_admin:
    entities:
        Product:
            # ...

# config/packages/easy_admin/user.yaml
easy_admin:
    entities:
        User:
            # ...
```

.. note::

    Beware that each configuration file must define its contents under
    the `easy_admin` key. Otherwise, Symfony won't be able to merge
    the different configurations.

Lastly, update the contents of the main `easy_admin.yaml` file to import all
these new files:

``` yaml
# config/packages/easy_admin.yaml
imports:
    - { resource: easy_admin/ }
```

