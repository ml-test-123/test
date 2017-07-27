openapi: '3.0.*' 
info: 
  title: DATEVconnect online/accounting-documents 
  version: '1.0.1' 
servers: 
  - url: https://api.datev.de/{basepath} 
    description: Production 
    variables: 
      basepath: 
        default: accounting/v1 
  - url: http://sandbox-api.datev.de/ 
    description: Test 
    # TODO: variables 
    
security: 
  datev_connect_online_oidc: 
    - accounting:clients:read 
    - accounting:documents 
  my_basic: 
  
components: 
  securitySchemes: 
    datev_connect_online_oidc: 
      type: oauth2 
      flows: 
        authorizationCode: 
          authorizationUrl: https://login.datev.de/openid/authorize 
          tokenUrl: https://api.datev.de/token 
          scopes: 
            'accounting:clients:read': get client data 
            'accounting:documents': transfer accounting documents for financial accounting 
      # openIdConnectUrl: https://login.datev.de/openid/.well-known/openid-configuration 
    my_basic: 
      type: http 
      scheme: basic 
  schemas: 
    Client: 
      type: object 
   
paths: 
  /clients: 
    get: 
      security: 
        datev_connect_online_oidc: 
          - 'accounting:clients:read' 
      responses: 
        200: 
          description: List of clients 
          content: 
            application/json: 
              schema: 
                type: array 
                items: 
                  $ref: '#/components/schemas/Client' 
  /test: 
    get: 
      responses: 
        200: 
          description: OK 
          content: 
            application/json: 
              type: object 
      security: 
        my_basic: 
