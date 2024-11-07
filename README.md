# Backend

The backend is responsible for storing and updating the visitor count, utilizing serverless architecture to maintain a low-cost, scalable API service.

## Backend Components

### API Gateway
- **Purpose**: Acts as the public-facing REST API for the visitor counter, handling HTTP requests from the frontend.
- **Configuration**: Configured to route requests to the AWS Lambda function.
- **CORS**: Enabled to allow requests from the frontend domain, ensuring secure cross-origin interactions.

### Lambda Function
- **Purpose**: The core logic for the visitor counter resides in this function. Written in Python, the function increments and retrieves the visitor count.
- **Flow**:
  - **GET Request**: Retrieves the current visitor count from DynamoDB and returns it as a JSON response.
  - **POST Request**: Increments the count in DynamoDB and returns the updated value.
- **Error Handling**: Includes error handling to ensure that database failures are gracefully managed, sending meaningful error responses back to the API Gateway.
- **Python Code**:
  - `lambda_function.py` contains the function’s core logic.
  - Includes data validation, response formatting, and error management.

### DynamoDB
- **Purpose**: Serves as a NoSQL database to store the visitor count.
- **Schema**: Simple key-value schema where the visitor count is stored under a single item, minimizing read and write costs.
- **Access**: The Lambda function has permission to read and write to this table, configured through IAM roles to ensure secure access.
- **Table Structure**:
  - **Primary Key**: `visitorCount` (fixed key)
  - **Value**: Count of visitors (integer, incremented on each visit)

## CI/CD for Backend

### GitHub Actions Workflow
The CI/CD pipeline automates testing and deployment of the backend code.

- **Tests**: Runs unit tests for the Lambda function to validate logic and ensure stability on every code change.
- **Deployment**: If tests pass, the pipeline packages the Lambda code and deploys it to AWS automatically.
- **Terraform Deployment**: For updates to infrastructure (DynamoDB, API Gateway), a separate workflow automatically applies these changes, keeping backend resources in sync with the code.
