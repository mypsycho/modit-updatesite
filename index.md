# Feedback: Building a DSL at i-BP
## Overview
### Context

Project to convert Rational Rose Extensions into Eclipse plugin 

- By translating UML stereotypes in DSL

Targeted DSL was important : 300+ classes, 350+ properties

- A lot of Stereotype packags were migrated in 4 Emf Packages

Edition features
- Few types of diagrams expected (4)  
- but all properties were edited through EEF and dispatched in 4 tabs

### Presented points
- Structural organization (in-short)
- Eclipse plugins development


## Structural organization: In-short
### Chosen factory
Maven/tycho, gerrit, Jenkins, Oomph, Jira/confluence (Model-Driven agile!)

### Notable elements
- A replicable structure for POM as the project were divided in 3 Gits
- Layout files to keep consistency between oomph descriptors and Maven/Typho TPD
- Reproducing Eclipse Nature philosophy in POMs using Pom parent and profile
  - For example: Xtend or projecting including code generation

## Development : Limits of model-driven approach
### Genmodel
A lot a boiler plates in code and hard to identify specific from mundane

Poor handling of multi-inheritance.

Customization of ItemProviderAdapter is painful.
- Even with EmfLoopHole, each sub-class needed to be rewritten.

Editing properties file was difficult with this number of Emf elements.
- Renaming/refactoring adds pollution

### Writting Sirius Model
(Un)Fortunately so repetitive 
- Possibility (Compelled) to dev of a creation wizard.

As one-shoot strategy, wizard lacks flexibility when modifying Meta-Model.

**Resulting ODesign is huge !**

## Retrospective analysis
### Needs ? (Beside using Xtend)
Extends meta-model in seamless way with the implementation

No generation for customization an Ecore Element

Modifications of meta-model should be measured by compilation error

### Constraints

Different aspects should be attached in a readable unit of code. 

No massive switch forcing functional rules to be separated

- In fact, developer should be able to choose between grouping 
  - by feature (validation, edition, ...)
  - by semantic (package or EClass) 

For example, display, validation and specific actions of 1 element sould be in 1 place

## Proposition
Extending meta-model by code
- Readable syntax (no eAnnotation)
- Real typed-approach (no symbolic code like plugin.xml extensions)





## How to use
Project [https://github.com/mypsycho/modit](https://github.com/mypsycho/modit)

### Available API 

A singleton of EmfStrecher works on a group of EPackage, registers specificities and provides inheritance mechanism.

EmfContribution provides a factory to initialize extensions of EPackage into the singleton.

Each major function (edit, validated, etc) must be supported by an engine based on EmfContribution and running with an EmfStrecher.

### Example for Sirius

Definition of singleton : EqxModelExtensions

Contribution of model : EquinoxeCoreContrib, EquinoxeComposantsMetierContrib, …

[code sample](https://github.com/mypsycho/ModIt/tree/master/tests/reversit-tests/src-gen/fr/ibp/odv/xad2/rcp/model)

Engine of Sirius (limited to EEF part) : 1 simple class (<500 lines)

[SiriusGenerator](https://github.com/mypsycho/ModIt/blob/master/tests/reversit-tests/src/org/mypsycho/emf/modit/reverit/test/SiriusGenerator.xtend)

## Complements

### To reverse engineer of pivot model (genmodel or sirius)
EReversIt can generate Xtend class matching Emf model

It eases detection of pattern in models.

Use case: Round-trip with Sirius
- Edit in run mode
- Reverse to code
- Update engine accordingly

### I18n in edit plugin is messy

Xtend syntax leads to a Class-based implementation 

Typed approach (not only String)

Using Xtend template instead of tricky pseudo MessageFormat

## Possible complements

### In progress : Create an ItemProviderAdapterFactory

Bevahior can be customize endlessly (not limited to genmodel)

Each function have a default behavior which can be overridden

### Fields of interest

Validation, actions

### Existing POC

EEF in Sirius


