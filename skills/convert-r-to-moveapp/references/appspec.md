## appspec.json:
The appspec.json file is used to define the specifications of an App within MoveApps.
Read the [App Specification](https://docs.moveapps.org/#/appspec?id=appspecjson) and then check the template here: [Template_R_Function_App/appspec.json](https://github.com/movestore/Template_R_Function_App/blob/master/appspec.json)
- `version` — copy from the live fetched template, don't invent it.
Currently, the following specifications can/need to be added:

1- [Settings](https://docs.moveapps.org/#/appspec/current/settings/README). 
- Every setting needs `id` (matches the R argument name), `name` (short label), `description` (plain language), and `defaultValue`. Choose
`type` based on what the argument represents, like the [example](https://docs.moveapps.org/#/appspec/current/settings/README?id=example).
- There are different types of settings. Read all the links:
  - [Text](https://docs.moveapps.org/#/appspec/current/settings/string) and [Example](https://docs.moveapps.org/#/appspec/current/settings/string?id=example).
  - [Integer numbers](https://docs.moveapps.org/#/appspec/current/settings/integer) and [Example](https://docs.moveapps.org/#/appspec/current/settings/integer?id=example).
  - [Real numbers](https://docs.moveapps.org/#/appspec/current/settings/double) and [Example](https://docs.moveapps.org/#/appspec/current/settings/double?id=example).
  - [Date selection](https://docs.moveapps.org/#/appspec/current/settings/timestamp) and [Example](https://docs.moveapps.org/#/appspec/current/settings/timestamp?id=example).
  - [Radiobuttons](https://docs.moveapps.org/#/appspec/current/settings/radiobuttons) and [Example](https://docs.moveapps.org/#/appspec/current/settings/radiobuttons?id=example).
  - [Checkboxes](https://docs.moveapps.org/#/appspec/current/settings/checkbox) and [Example](https://docs.moveapps.org/#/appspec/current/settings/checkbox?id=example).
  - [Dropdown](https://docs.moveapps.org/#/appspec/current/settings/dropdown) and [Example](https://docs.moveapps.org/#/appspec/current/settings/dropdown?id=example).
  - [Passwords](https://docs.moveapps.org/#/appspec/current/settings/secret) and [Example](https://docs.moveapps.org/#/appspec/current/settings/secret?id=example).
  - [Auxiliary/user files](https://docs.moveapps.org/#/appspec/current/settings/user_file).


2- [Dependencies](https://docs.moveapps.org/#/appspec/current/dependencies_appspec) 
  - All libraries on which the App needs for its construction and/or runtime.
  - Do not include any base library in the appspecs.json file.
  - For a CRAN package, just`{"name": "pkgname"}`. For a package that comes from somewhere else (GitHub, GitLab, etc.), add a [`"remotes"`](https://remotes.r-lib.org/reference/index.html).
  - Read the [Examples](https://docs.moveapps.org/#/appspec/current/dependencies_appspec?id=example).

3- [License](https://docs.moveapps.org/#/appspec/current/license_appspec)
- Check the license options from which the license key has to be entered: [List of license keys](https://docs.moveapps.org/#/appspec/current/license_appspec?id=list-of-license-keys) and then check the links of those 4 options:
  
    1- [GPL-3.0-or-later](https://spdx.org/licenses/GPL-3.0-or-later.html#licenseText)
  
    2- [MIT](https://spdx.org/licenses/MIT.html#licenseText)
  
    3- [AGPL-3.0-or-later](https://spdx.org/licenses/AGPL-3.0-or-later.html#licenseText)
  
    4- [BSD-3-Clause](https://spdx.org/licenses/BSD-3-Clause.html#licenseText)
  
- Check the [example](https://docs.moveapps.org/#/appspec/current/license_appspec?id=example)
- Ask user about the license agreement. Show them the options and briefly explain about the options before asking.
  
4- [Language](https://docs.moveapps.org/#/appspec/current/language_appspec)
  - Check the [example](https://docs.moveapps.org/#/appspec/current/language_appspec?id=example).
  
5- [Keywords](https://docs.moveapps.org/#/appspec/current/keywords_appspec)
- Check the [example](https://docs.moveapps.org/#/appspec/current/keywords_appspec?id=example).
- provide some keywords to user and ask if they want to add more.
  
6- [People](https://docs.moveapps.org/#/appspec/current/people_appspec)
- ask user about all the items for the people that are in this [example](https://docs.moveapps.org/#/appspec/current/people_appspec?id=example):
  "firstName", "middleInitials", "lastName", "email", "roles",  "orcid": null, "affiliation", "affiliationRor"
- For roles check the [List of roles](https://docs.moveapps.org/#/appspec/current/people_appspec?id=list-of-roles) and then show the options of roles.
- Ask user about the people and their role in this way:
enter the first name,  then the next person....
- list the people like the [example](https://docs.moveapps.org/#/appspec/current/people_appspec?id=example)

7- [Funding](https://docs.moveapps.org/#/appspec/current/funding_appspec) 
- The funding statement is not mandatory.
- see the [example](https://docs.moveapps.org/#/appspec/current/funding_appspec?id=example).
  
8- [References](https://docs.moveapps.org/#/appspec/current/references_appspec)
  - Check the [Reference types](https://docs.moveapps.org/#/appspec/current/references_appspec?id=reference-types)
  - see the [example](https://docs.moveapps.org/#/appspec/current/references_appspec?id=examples) and the Note after that.
  - If the App is based mainly on a single library, to acknowledge them add it as a [refrence](https://docs.moveapps.org/#/best_practices_coding?id=acknowledgements-and-references).

## Validation
- Write all the items above like an example in appspec.json file.
- The finished `appspec.json` can be checked at the MoveApps [Settings Editor ](https://www.moveapps.org/apps/settingseditor)
