## Example : 
### **tools**

- **test-helper.ts**: This file contains a class that is used to make testing easier. It’s a utility intended for testing purposes and not directly related to the core application logic. It falls under **tools** because it’s assisting in the testing and development process.

### **utils**

- **logger.ts**: This is a utility used throughout the application for logging purposes. It provides a simple logging mechanism to track and log events and errors. Since it's a common helper function used across your app, it's categorized as a **utility**.
- **phoneTools.ts**: Contains helper functions to convert phone numbers to E164 format and validate them. This is general-purpose code that can be used throughout the application, making it a **utility**.
- **format.ts**: Contains helper functions for formatting, such as masking emails and phone numbers. These small, reusable functions make it a **utility**.

### **lib**

- **http-response-builder.ts**: This file builds HTTP responses and customizes them depending on the environment (e.g., adding debug information in development). Since it directly handles core application functionality, it’s categorized under **lib**. It's more substantial than a simple utility, as it’s deeply integrated into the application’s behavior.
- **api-connector.ts**: Configures and provides functions to create API connectors using Axios, which is essential for interacting with external APIs. This is a core module of your application, so it falls under **lib**. It encapsulates the logic of making API calls, and the logic can be reused across your application.
- **password.ts**: Manages password verification using multiple algorithms and provides a random password generation function. It’s a core piece of functionality for handling user authentication, so it belongs under **lib**. This module is likely central to your authentication system.

### Summary of Categorization:

- **tools**:
    
    - test-helper.ts
- **utils**:
    
    - logger.ts
    - phoneTools.ts
    - format.ts
- **lib**:
    
    - http-response-builder.ts
    - api-connector.ts
    - password.ts