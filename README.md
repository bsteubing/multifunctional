# Multifunctional

Handling multifunctional activities in the Brightway LCA software framework.

Multifunctional activities can lead to linear algebra problems which don't have exactly one solution. Therefore, we commonly need to apply a handling function to either partition such activities, or otherwise manipulate their data such that they allow for the creation of a non-singular, square technosphere matrix.

This library is designed around the following workflow:

1. A multifunctional activity is created and saved to the database by a user. A multifunctional activity is any activity with multiple functional flows, either outputs (e.g. products) or input (e.g. wastes).
1. The user provides a handling function of this activity, such as substitution, allocation, etc.
1. When this database is processed, this library will apply the handling function, and create a square matrix for the database.

More functionality is planned; see [limitations](#limitations).

## How does it work?

1. Users label functional exchanges when creating them: ``exchange_instance['functional'] = True``.
1. Saving a functional flow exchange will automatically create a new `product` node in the supply chain graph (if necessary).
1. Users give multifunctional activities a handler function label: ``activity_instance['handler'] = 'some_handler_function_label'``. Handler function labels are strings.
1. Handler functions are mapped in `multifunctional.handler_mapping`. This library provides a default set of handling functions (see below); users may also add custom handlers.
1. When the database is processed, the handler functions are executed, and if necessary, virtual activities are created. The processed array will create a square matrix.

## Limitations

* This library currently only supports one handler for multifunctional activities. Support for variable handlers will be added in a later release.

* This library is not yet integrated with `bw2data` - therefore, multifunctional activities must be manually converted:

```python
import brightway2 as bw
import multifunctional as mf

mf.convert_multifunctional_activity(
    activity=bw.get_activity(("some", "activity")),
    handler="some_handling_function_label",
)
```

* This library current only works with the default SQlite backend

## Installation

Install via conda or pip.

## Contributing

Your contribution is welcome! Please follow the [pull request workflow](https://guides.github.com/introduction/flow/), even for minor changes.

When contributing to this repository with a major change, please first discuss the change you wish to make via issue, email, or any other method with the owners of this repository.

Please note we have a [code of conduct](https://github.com/brightway-lca/multifunctional/blob/master/CODE_OF_CONDUCT.md), please follow it in all your interactions with the project.

### Documentation and coding standards

* [Black formatting](https://black.readthedocs.io/en/stable/)
* [Semantic versioning](http://semver.org/)

## Maintainers

* [Chris Mutel](https://github.com/cmutel/)

## License

[BSD-3-Clause](https://github.com/brightway-lca/multifunctional/blob/master/LICENSE). Copyright 2020 Chris Mutel.
