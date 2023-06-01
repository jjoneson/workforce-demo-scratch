<spec title="Order API">
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
</spec>