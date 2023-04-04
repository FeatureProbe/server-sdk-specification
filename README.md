# FeatureProbe server side SDK Specification

We want FeatureProbe to be available for each popular programming languages, and still we want each of these SDKs to
have a consistent behavior.

This project defined expected result for a given FeatureProbe environment. And each server side SDK implementation
should adhere to these specs.

## File structure

Specs are located in /spec folder, in JSON format. Each language SDK implementation have to construct a test framework
to load and run these specs with their own language and test framework conventions.

## Spec description

A spec file has the following shape:

```json
{
  "tests": [
    {
      "scenario": "Name of this scenario",
      "fixture": {
        "segments": {},
        "toggles": {}
      },
      "cases": [
        {
          "name": "name of this test case.",
          "user": {
            "key": "user id",
            "customValues": [
              {
                "key": "email",
                "value": "name@mycompany.com"
              }
            ]
          },
          "function": {
            "name": "bool_value",
            "toggle": "toggle_to_test",
            "default": true
          },
          "expectResult": {
            "value": true,
            "noRuleIndex": true,
            "reason": "default"
          }
        }
      ]
    }
  ]
}
```

Fields description:

* __tests__ : list of all test scenarios in this spec file.
  * __scenario__ : each spec file contains several scenarios, this is the name of the scenario.
  * __fixture__ : each scenario share this fixture, fixture represent a FeatureProbe environment, which use the same
    format as SDK pulled from FeatureProbe server.
    * __segments__ : user segments in this environment.
    * __toggles__ : feature toggles in this environment.
  * __cases__ : test cases in this scenario.
    * __name__ : name of this case.
    * __user__ : use info in this struct to construct a FPUser.
      * __key__ : user id of FPUser.
      * __customValues__ : other key-value pairs to use with FPUser.with.
    * __function__ : FeatureProbe function to test.
      * __name__ : function name.
      * __toggle__ : toggle name, the first parameter to call FeatureProbe function.
      * __default__ : default value, the last parameter to call FeatureProbe function.
    * __expectResult__ : expected result of function call.
      * __value__ : returned variation value.
      * __reason__ : reason in FPDetail, if called _detail functions.
      * __ruleIndex__ : ruleIndex in FPDetail.
      * __noRuleIndex__ : should not return ruleIndex in FPDetail.
      * __version__ : version in FPDetail.
