paths:
  /users/{id}:  # Endpoint URL
    get:  # HTTP Method
      summary: Get user details
      description: Retrieves the details of a specific user.
      parameters:
        - name: id
          in: path
          required: true
          description: User ID
          schema:
            type: integer
      responses:
        "200": # HTTP Status Code
          description: Successful operation
          content:
            application/json:
              schema:  # Response Schema (JSON)
                type: object
                properties:
                  id:
                    type: integer
                  name:
                    type: string
                  email:
                    type: string
        "404": # HTTP Status Code
          description: User not found
          content:
            application/json:
              schema:
                type: object
                properties:
                  error:
                    type: string
paths:
  /calculate:
    post:
      summary: Calculate the result of a mathematical expression.
      description: Accepts a mathematical expression string and returns the result.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                expression:
                  type: string
                  description: Mathematical expression to calculate.
                  example: "2 + 3 * 4"
              required:
                - expression
      responses:
        "200":
          description: Successful calculation.
          content:
            application/json:
              schema:
                type: object
                properties:
                  result:
                    type: number
                    description: Result of the calculation.
                    example: 14
        "400":
          description: Invalid expression provided.
          content:
            application/json:
              schema:
                type: object
                properties:
                  error:
                    type: string
                    description: Error message.
                    example: "Invalid expression format"
  /operation/{op}:
    get:
      summary: Perform a specific operation.
      description: Accepts an operation type and two numbers to perform a calculation.
      parameters:
        - name: op
          in: path
          required: true
          description: Operation type (e.g., add, subtract, multiply, divide).
          schema:
            type: string
            enum: [ "add", "subtract", "multiply", "divide" ]
        - name: num1
          in: query
          required: true
          description: First number.
          schema:
            type: number
        - name: num2
          in: query
          required: true
          description: Second number.
          schema:
            type: number
      responses:
        "200":
          description: Successful calculation.
          content:
            application/json:
              schema:
                type: object
                properties:
                  result:
                    type: number
                    description: Result of the calculation.
                    example: 7
        "400":
          description: Invalid operation type, or invalid number format.
          content:
            application/json:
              schema:
                type: object
                properties:
                  error:
                    type: string
                    description: Error message.
                    example: "Invalid operation type"
