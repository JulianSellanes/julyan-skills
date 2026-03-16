# AWS Guide for any AI reading this

version = 1

## Http API

When you create an http api resource, make sure the syntax is similar to the following examples:

```
#Http API

HttpApi:
  Type: AWS::Serverless::HttpApi
  Properties:
    Name: !Sub "${AWS::StackName}-HttpApi"
    Description: !Sub "[${AWS::StackName}] HttpApi"
    CorsConfiguration:
      AllowMethods:
        - GET
        - POST
        - OPTIONS
        - DELETE
      AllowHeaders:
        - content-type
      AllowOrigins:
      AllowOrigins: !If
        - IsProd
        - [ "https://prod-domain.com", "https://www.prod-domain.com" ]
        - [ "http://localhost:5173", !Sub "https://${FrontendDistribution.DomainName}" ]
      ExposeHeaders:
        - content-type
      AllowCredentials: false
      MaxAge: 0
      DefaultRouteSettings:
        ThrottlingBurstLimit: 100
        ThrottlingRateLimit: 50
      #If the web needs Auth
      Auth:
        DefaultAuthorizer: CognitoAuthorizer #All api calls need auth token to call this HttpApi
        Authorizers:
          CognitoAuthorizer:
            IdentitySource: $request.header.Authorization
            JwtConfiguration:
              issuer: !Sub https://cognito-idp.${AWS::Region}.amazonaws.com/${CognitoUserPool}
              audience:
                - !Ref CognitoUserPoolClient
```

## Tables

When you create a dynamodb table resource, make sure the syntax is similar to the following examples:

```
#Tables

ProfilesTable:
  Type: AWS::DynamoDB::Table
  DeletionPolicy: Retain
  UpdateReplacePolicy: Retain
  Properties:
    TableName: !Sub ${AWS::StackName}-Profiles 
    KeySchema:
      - AttributeName: userId
        KeyType: HASH
    AttributeDefinitions:
      - AttributeName: userId
        AttributeType: S
      - AttributeName: lb
        AttributeType: S
      - AttributeName: trophies
        AttributeType: N
    GlobalSecondaryIndexes:
      - IndexName: byTrophies
        KeySchema:
          - AttributeName: lb
            KeyType: HASH
          - AttributeName: trophies
            KeyType: RANGE
        Projection:
          ProjectionType: ALL
    BillingMode: PAY_PER_REQUEST
    StreamSpecification:
      StreamViewType: NEW_AND_OLD_IMAGES
    SSESpecification:
      SSEEnabled: true

RoomsTable:
  Type: AWS::DynamoDB::Table
  DeletionPolicy: Retain
  UpdateReplacePolicy: Retain
  Properties:
    TableName: !Sub ${AWS::StackName}-Rooms
    KeySchema:
      - AttributeName: roomId
        KeyType: HASH
    AttributeDefinitions:
      - AttributeName: roomId
        AttributeType: S
    BillingMode: PAY_PER_REQUEST
    StreamSpecification:
      StreamViewType: NEW_AND_OLD_IMAGES
    SSESpecification:
      SSEEnabled: true
```

## Lambdas

When you create a lambda resource, make sure the syntax is similar to the following examples:

```
#Lambdas

GetHello:
  Type: AWS::Serverless::Function
  Properties:
    FunctionName: !Sub ${AWS::StackName}-GetHello
    Description: !Sub [${AWS::StackName}] GetHello
    CodeUri: ./src/lambdas/GetHello
    Handler: index.handler
    Runtime: nodejs22.x
    MemorySize: 128
    EphemeralStorage:
      Size: 512
    Timeout: 30
    Tracing: Disabled
    Environment:
      Variables:
        PROFILES_TABLE: !Ref ProfilesTable
    Policies:
      - DynamoDBCrudPolicy:
          TableName: !Ref ProfilesTable
    Events:
      #If lambda needs to call an api
      ApiTrigger:
        Type: HttpApi
        Properties:
          Path: /hello
          Method: GET
          ApiId: !Ref HttpApi
      #If lambda needs to call cognito
      UserConfirmation:
        Type: Cognito
        Properties:
          UserPool: !Ref CognitoUserPool
          Trigger: PostConfirmation
      #If lambda needs to call stripe event
      StripeEvent:
        Type: EventBridgeRule
        Properties:
          EventBusName: aws.partner/stripe.com/ed_test_61...
          Pattern:
            source:
              - prefix: aws.partner/stripe.com
            detail-type:
              - checkout.session.completed

GetHelloLogGroup:
  Type: AWS::Logs::LogGroup
  DeletionPolicy: Delete
  UpdateReplacePolicy: Delete
  Properties:
    LogGroupName: !Sub /aws/lambda/${GetHello}
    RetentionInDays: 7

LambdaWithLayer:
  Type: AWS::Serverless::Function
  Properties:
    FunctionName: !Sub ${AWS::StackName}-LambdaWithLayer
    Description: !Sub [${AWS::StackName}] LambdaWithLayer
    CodeUri: ./src/lambdas/LambdaWithLayer
    Handler: index.handler
    Runtime: python3.13
    MemorySize: 128
    EphemeralStorage:
      Size: 512
    Timeout: 30
    Tracing: Disabled
    Environment: {}
    Layers:
      - !Ref TestLayer

LambdaWithLayerLogGroup:
  Type: AWS::Logs::LogGroup
  DeletionPolicy: Delete
  UpdateReplacePolicy: Delete
  Properties:
    LogGroupName: !Sub /aws/lambda/${LambdaWithLayer}
    RetentionInDays: 7

TestLayer:
  Type: AWS::Serverless::LayerVersion
  Properties:
    LayerName: RequestsLayer
    Description: Layer containing 'requests' package
    ContentUri: src/layers/TestLayer
    RetentionPolicy: Delete
    CompatibleRuntimes:
      - python3.13
```

1) All lambda functions must have their own LogGroup resource
2) If the lambda needs a layer, remember to create its LayerVersion resource
3) Create the lambda folder inside backend/src/lambdas (the folder must have the same name as the lambda)
4) The lambda folder must have:

index.js:
```
export const handler = async (event) => {
    return { statusCode: 200 };
}
```

package.json:
```
{
    "name": "get-hello",
    "version": "1.0.0",
    "type": "module",
    "main": "index.js",
    "keywords": [],
    "author": "",
    "license": "ISC",
    "description": "Some description",
    "dependencies": {}
}
```

5) If the lambda has dependencies, from the root folder, use `pnpm install` to generate the node_modules folder