<story title="Create Order API">
As an Order API, I want to create an order, so that I can fulfill the order.

# Acceptance Criteria
- [ ] I can create an order
- [ ] I can get an order
- [ ] I can update an order
- [ ] I can delete an order

# Dependencies
- [ ] Warehouse API
- [ ] Email API
</story>

<story title="Create Warehouse API">
As a Warehouse API, I want to fulfill an order, so that I can ship the order.

# Acceptance Criteria
- [ ] I can fulfill an order
</story>

<apiDesign title="Order API">
openapi: 3.0.0
      info:
        title: Order API
        description: Manages Orders.
        version: 1.0.0
      servers:
          - url: http://localhost:8080
      paths:
          /orders:
              post:
                  summary: Create Order
                  description: Creates an order.
                  requestBody:
                      required: true
                      content:
                          application/json:
                              schema:
                                  $ref: '#/components/schemas/Order'
                  responses:
                      '201':
                          description: Order Created
                          content:
                              application/json:
                                  schema:
                                      $ref: '#/components/schemas/Order'
      components:
          schemas:
              Order:
                  type: object
                  properties:
                      order_id:
                          type: integer
                          description: The order id.
                      customer_id:
                          type: integer
                          description: The customer id.
                      order_date:
                          type: string
                          format: date-time
                          description: The order date.
                      order_total:
                          type: number
                          format: double
                          description: The order total.
                      order_status:
                          type: string
                          description: The order status.
</apiDesign>