## LAMBDA-BACKED CUSTOM RESOURCE

When implementing the lambda function, of a Lambda-backed Custom Resource, there are two main considerations to address. First, if the function errored out with exception, it wont response to the cloudformation creation process. As a result the creation process will hang and the Service Catalog consumer will wait for about an hour until the product provisioning will eventually fail - bad UX.  Second, after created, the cloudformation stack may be updated or deleted, thus the custom resource lambda function must handle the whole stack (custom resource) life cycle. I.e., “Update” and “Delete” actions.
To mitigate the first risk, the main function should be designed to catch and handle any exception, and always response to cloudformation. Moreover, the exception message should be included in the response, in order for the Service Catalog consumer to get a clue about the failure reason.