# This is documentation about kredensialku API

\
Kredensialku has two main services

- kredensialku
  kredensialku mainly for student credensial
- dokumenku
  dokumenku for any document

## I. Rest API

- [ ] Authentication

```YAML
  path: /auth
  method: POST
  input:
    - body:
      type: JSON
      data:
        - name: username
          type: string
          nullable: true
        - name: serviceCode
          type: string
          nullable: true
        - name: passphrase
          type: string
          nullable: false
  output:
    - code: 201
      type: JSON
      data:
        - name: token
          type: string
          nullable: false
        - name: expiredAt
          type: int64
          format: unixepoch
          nullable: false
    - code: 400
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 401
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 403
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 500
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
```

- [ ] Create Template

```YAML
  path: /template
  method: POST
  header:
    - name: Authorizations
      type: string
      format: refreshToken from auth
  input:
    - body:
      type: multipart/form-data
      data:
        - name: file
          type: file
          format: html-template
          nullable: false
  output:
    - code: 201
      type: JSON
      data:
        - name: template
          type: string
          format: url-encoded-base64
          nullable: false
        - name: hash
          type: string
          format: hexsha256
          nullable: false
    - code: 400
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 401
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 403
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 404
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 500
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
```

- [ ] Load One Template

```YAML
  path: /template/:hash
  method: Get
  header:
    - name: Authorizations
      type: string
      format: accessClaim from create access
  input:
    - path:
      name: hash
      type: string
      format: hexsha256
      nullable: false
  output:
    - code: 200
      type: JSON
      data:
        - name: template
          type: string
          format: base64
          nullable: false
        - name: hash
          type: string
          format: hexsha256
          nullable: false
        - name: context
          type: string
          nullable: false
        - name: timestamp
          type: string
          format: RFC3339
    - code: 400
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 401
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 403
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 404
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 500
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
```

- [ ] Load Template

```YAML
  path: /template
  method: Get
  header:
    - name: Authorizations
      type: string
      format: accessClaim from create access
  output:
    - code: 200
      type: JSON
      data:
        - type: array
          nullable: false
          values:
            - name: template
              type: string
              format: base64
              nullable: false
            - name: hash
              type: string
              format: hexsha256
              nullable: false
            - name: context
              type: string
              nullable: false
            - name: timestamp
              type: string
              format: RFC3339
    - code: 400
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 401
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 403
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 404
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 500
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
```

- [ ] Create/Update Document

```YAML
  path: /document
  method: POST
  header:
    - name: Authorizations
      type: string
      format: accessClaim from create access
  input:
    - body:
      type: JSON
      data:
        - name: ownerType
          type: string
          nullable: false
        - name: ownerId
          type: string
          nullable: false
        - name: documentId
          type: string
          nullable: true
          description: If not provided random UUIDV4 will be generated, to update document this field must be provided to update the old one
        - name: data
          type: JSON
          nullable: false
        - name: template
          type: string
          format: hexsha256
          nullable: false
        - name: email
          type: string
          format: email
          nullable: true
        - name: phoneNumber
          type: string
          format: phone number
          nullable: true
        - name: whatsapp
          type: string
          format: phone number
          nullable: true
  output:
    - code: 201
      type: JSON
      data:
        - name: ownerType
          type: string
          nullable: false
          description: Indicator owner id types, eg NIM, NIK, passport, etc.
        - name: ownerId
          type: string
          nullable: false
        - name: documentId
          type: string
          nullable: false
          description: Please save this value to update document in future.
        - name: hash
          type: string
          format: hexsha256
          nullable: false
          description: Should change everytime submitted to preserve uniqueness across document version.
        - name: code
          type: string
          format: alphanumeric 8 digits, length can change in future
          nullable: false
          description: Will not change if document is updated.
        - name: pdf
          type: string
          format: base64
          nullable: false
        - name: email
          type: string
          format: email
          nullable: true
        - name: phoneNumber
          type: string
          format: phone number
          nullable: true
        - name: whatsapp
          type: string
          format: phone number
          nullable: true
        - name: timestamp
          type: string
          format: RFC3339
    - code: 400
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 401
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 403
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 404
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 500
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
```

- [ ] Load One Document

```YAML
  path: /document/:documentId
  method: GET
  header:
    - name: Authorizations
      type: string
      format: accessClaim from create access
  input:
    - path:
      name: documentId
      type: string
      nullable: false
  output:
    - code: 200
      type: JSON
      data:
        - name: ownerType
          type: string
          nullable: false
          description: Indicator owner id types, eg NIM, NIK, passport, etc.
        - name: ownerId
          type: string
          nullable: false
        - name: documentId
          type: string
          nullable: false
          description: Please save this value to update document in future.
        - name: hash
          type: string
          format: hexsha256
          nullable: false
          description: Should change everytime submitted to preserve uniqueness across document version.
        - name: code
          type: string
          format: alphanumeric 8 digits, length can change in future
          nullable: false
          description: Will not change if document is updated.
        - name: pdf
          type: string
          format: base64
          nullable: false
        - name: email
          type: string
          format: email
          nullable: true
        - name: phoneNumber
          type: string
          format: phone number
          nullable: true
        - name: whatsapp
          type: string
          format: phone number
          nullable: true
        - name: timestamp
          type: string
          format: RFC3339
    - code: 401
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 403
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 404
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 500
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
```

- [ ] Load Document

```YAML
  path: /document
  method: GET
  header:
    - name: Authorizations
      type: string
      format: accessClaim from create access
  output:
    - code: 200
      type: JSON
      data:
        - type: array
          nullable: false
          values:
            - name: ownerType
              type: string
              nullable: false
              description: Indicator owner id types, eg NIM, NIK, passport, etc.
            - name: ownerId
              type: string
              nullable: false
            - name: documentId
              type: string
              nullable: false
              description: Please save this value to update document in future.
            - name: hash
              type: string
              format: hexsha256
              nullable: false
              description: Should change everytime submitted to preserve uniqueness across document version.
            - name: code
              type: string
              format: alphanumeric 8 digits, length can change in future
              nullable: false
              description: Will not change if document is updated.
            - name: email
              type: string
              format: email
              nullable: true
            - name: phoneNumber
              type: string
              format: phone number
              nullable: true
            - name: whatsapp
              type: string
              format: phone number
              nullable: true
            - name: timestamp
              type: string
              format: RFC3339
    - code: 401
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 403
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 404
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 500
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
```

- [ ] Load Document History

```YAML
  path: /document/:documentId/history
  method: GET
  header:
    - name: Authorizations
      type: string
      format: accessClaim from create access
  output:
    - code: 200
      type: JSON
      data:
        - type: array
          nullable: false
          values:
            - name: ownerType
              type: string
              nullable: false
              description: Indicator owner id types, eg NIM, NIK, passport, etc.
            - name: ownerId
              type: string
              nullable: false
            - name: documentId
              type: string
              nullable: false
              description: Please save this value to update document in future.
            - name: hash
              type: string
              format: hexsha256
              nullable: false
              description: Should change everytime submitted to preserve uniqueness across document version.
            - name: code
              type: string
              format: alphanumeric 8 digits, length can change in future
              nullable: false
              description: Will not change if document is updated.
            - name: email
              type: string
              format: email
              nullable: true
            - name: phoneNumber
              type: string
              format: phone number
              nullable: true
            - name: whatsapp
              type: string
              format: phone number
              nullable: true
            - name: timestamp
              type: string
              format: RFC3339
    - code: 401
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 403
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 404
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 500
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
```
