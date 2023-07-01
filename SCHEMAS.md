# Overview

## System Definition

A system is a deployable and runnable core feather item. Our bread and butter. The system schema describes, in JSON, everything we need to know about a system and the steps. The schema looks as follows:

```
{
    "steps": [
        {
            "inputs": [
                {
                    "_name": "Name of this input parameter, as defined in python",
                    "component_type": "If this input is a component, this will specify the type of component",
                    "component_version": "If this input is a component, the version of it"
                    "id": "GUID identifying this input",
                    "schema": "JSON defining the structure of the input parameter",
                    "type": "The type of input, eg string, number or COMPONENT",
                },
            ],
            "name": "Name of the step, which must be equalt to the python function for this step",
            "outputs": [
            ]
        }
    ],
    "version": "SemVer Version for this system definition"
}
```