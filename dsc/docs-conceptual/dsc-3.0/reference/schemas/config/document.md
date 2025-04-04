---
description: JSON schema reference for a Desired State Configuration document.
ms.date:     05/09/2024
ms.topic:    reference
title:       DSC Configuration document schema reference
---

# DSC Configuration document schema reference

## Synopsis

The YAML or JSON file that defines a DSC Configuration.

## Metadata

```yaml
SchemaDialect: https://json-schema.org/draft/2020-12/schema
SchemaID:      https://raw.githubusercontent.com/PowerShell/DSC/main/schemas/2024/04/config/document.json
Type:          object
```

## Description

DSC Configurations enable users to define state by combining different DSC Resources. A
configuration document uses parameters and variables to pass to a set of one or more resources that
define a desired state.

A configuration document can be defined as either YAML or JSON. For ease of authoring, Microsoft
recommends drafting configuration documents in YAML.

For DSC's authoring tools to recognize a file as a DSC Configuration document, the filename must
end with `.dsc.config.json`, `.dsc.config.yml`, or `.dsc.config.yaml`.

You can use configuration document functions to dynamically determine values in the document at
runtime. For more information, see [DSC Configuration document functions reference][01]

<!-- For more information, see [DSC Configurations overview][01]. -->

The rest of this document describes the schema DSC uses to validation configuration documents.

## Examples

<!-- To-Do -->

## Required Properties

Every configuration document must include these properties:

- [$schema]
- [resources]

## Properties

### $schema

The `$schema` property indicates the canonical URL of the version of this schema that the document
adheres to. DSC uses this property when validating the configuration document before any
configuration operations.

There are currently 3 published versions of the schema, compatible with different versions of DSC:

- `2024/04` is the latest version of the schema, compatible with DSC version 3.0.0-preview.7 and
  later.
- `2023/10` is the previous version of the schema, compatible with DSC versions `3.0.0-alpha.4` and
  `3.0.0-alpha.5`.
- `2023/08` is the first version of the schema, compatible with DSC versions `3.0.0-alpha.1` through
  `3.0.0-alpha.3`.

This documentation is for the latest version of the schema. You should update your configuration
documents and resource manifests to the latest version of the schema. Prior versions don't work
with new releases of DSC. The schemas remain published, but won't get any updates.

For every version of the schema, there are three valid urls:

- `.../config/document.json`

  The URL to the canonical non-bundled schema. When it's used for validation, the validating client
  needs to retrieve this schema and every schema it references.

- `.../bundled/config/document.json`

  The URL to the bundled schema. When it's used for validation, the validating client only needs to
  retrieve this schema.

  This schema uses the bundling model introduced for JSON Schema 2020-12. While DSC can still
  validate the document when it uses this schema, other tools might error or behave in unexpected
  ways.

- `.../bundled/config/document.vscode.json`

  The URL to the enhanced authoring schema. This schema is much larger than the other schemas, as
  it includes additional definitions that provide contextual help and snippets that the others
  don't include.

  This schema uses keywords that are only recognized by VS Code. While DSC can still validate the
  document when it uses this schema, other tools might error or behave in unexpected ways.

```yaml
Type:        string
Required:    true
Format:      URI
ValidValues: [
               https://raw.githubusercontent.com/PowerShell/DSC/main/schemas/2024/04/config/document.json
               https://raw.githubusercontent.com/PowerShell/DSC/main/schemas/2024/04/bundled/config/document.json
               https://raw.githubusercontent.com/PowerShell/DSC/main/schemas/2024/04/bundled/config/document.vscode.json
               https://raw.githubusercontent.com/PowerShell/DSC/main/schemas/2023/10/config/document.json
               https://raw.githubusercontent.com/PowerShell/DSC/main/schemas/2023/10/bundled/config/document.json
               https://raw.githubusercontent.com/PowerShell/DSC/main/schemas/2023/10/bundled/config/document.vscode.json
               https://raw.githubusercontent.com/PowerShell/DSC/main/schemas/2023/08/config/document.json
               https://raw.githubusercontent.com/PowerShell/DSC/main/schemas/2023/08/bundled/config/document.json
               https://raw.githubusercontent.com/PowerShell/DSC/main/schemas/2023/08/bundled/config/document.vscode.json
             ]
```

### metadata

The `metadata` property defines a set of key-value pairs as annotations for the configuration. DSC
doesn't validate the metadata. A configuration can include any arbitrary information in this
property.

```yaml
Type:     object
Required: false
```

### parameters

The `parameters` property defines a set of runtime options for the configuration. Each parameter is
defined as key-value pair. The key for each pair defines the name of the parameter. The value for
each pair must be an object that defines the `type` keyword to indicate how DSC should process the
parameter.

You can override parameters at run-time, enabling re-use of the same configuration document for
different contexts.

For more information about defining parameters in a configuration, see
[DSC Configuration document parameter schema][02].

<!-- For more information about using parameters in a configuration, see
[DSC Configuration parameters][03] -->

```yaml
Type:                object
Required:            false
ValidPropertySchema: https://raw.githubusercontent.com/PowerShell/DSC/main/schemas/2024/04/config/document.parameter.json
```

### variables

The `variables` property defines a set of reusable values for the resources in the document as
key-value pairs. The key for each pair defines the name of the variable. Resources that reference
the variable by name can access the variable's value.

This can help reduce the amount of copied values and options for resources in the configuration,
which makes the document easier to read and maintain. Unlike parameters, variables can only be
defined in the configuration and can't be overridden at run-time.

<!-- For more information about using variables in a configuration, see
[DSC Configuration variables][04]. -->

```yaml
Type:     object
Required: false
```

### resources

The `resources` property defines a list of DSC resource instances that the configuration document
manages. Every instance in the list must be unique, but instances might share the same DSC resource
type.

For more information about defining a valid resource instance in a configuration, see
[DSC Configuration document resource schema][05].

<!-- For more information about how DSC uses resources in a configuration, see
[DSC Configuration resources][06] and [DSC Configuration resource groups][07]. -->

```yaml
Type:             array
Required:         true
MinimumItemCount: 1
ValidItemSchema:  https://raw.githubusercontent.com/PowerShell/DSC/main/schemas/2024/04/config/document.resource.json
```

<!-- Link reference definitions -->
[01]: functions/resourceId.md
<!-- [01]: ../../../configurations/overview.md -->
[02]: parameter.md
<!-- [03]: ../../../configurations/parameters.md -->
<!-- [04]: ../../../configurations/variables.md -->
[05]: resource.md
<!-- [06]: ../../../configurations/resources.md -->
<!-- [07]: ../../../configurations/resource-groups.md -->
