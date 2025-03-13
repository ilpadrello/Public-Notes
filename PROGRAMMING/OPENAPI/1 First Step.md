The root object in any OpenAPI description is the OpenAPI Object, and only two of its fields are mandatory: `openapi` and `info`. Additionally, at least one of `paths`, `components` and `webhooks` is required.

- `openapi` (**string**): This indicates the version of the OAS this OAD is using, e.g. “3.1.0”. Using this field tools can check that the description correctly adheres to the specification.
- `info` ([Info Object](https://spec.openapis.org/oas/v3.1.0#info-object)): This provides general information about the API (like its description, author and contact information) but the only mandatory fields are `title` and `version`
- 