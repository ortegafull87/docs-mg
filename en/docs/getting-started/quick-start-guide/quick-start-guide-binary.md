# Quick Start Guide - Binary

**Let's host your first API on WSO2 API Microgateway using the WSO2 Microgateway runtime.**

-   [Before you begin...](#QuickStartGuide-Binary-Beforeyoubegin...)
-   [Step 1 - Generate an executable using WSO2 API Microgateway Toolkit](#QuickStartGuide-Binary-Step1-GenerateanexecutableusingWSO2APIMicrogatewayToolkit)
-   [Step 2 - Run the API Microgateway](#QuickStartGuide-Binary-Step2-RuntheAPIMicrogateway)
-   [Step 3 - Invoke the sample API](#QuickStartGuide-Binary-Step3-InvokethesampleAPI)

### Before you begin...

Make sure to install and set up all the [installation prerequisites](_Install_on_VM_) .

### Step 1 - Generate an executable using WSO2 API Microgateway Toolkit

##### Step 1.1 - Initialize a project

1.  Navigate to a preferred workspace folder using the command line. This is the location that is used to store the Microgateway project.
2.  Run the following command to create a project named "petstore" . This will create the folder structure for the artifacts to be included.  Use the -a option to include the API definition to the project as follows.

    **Format**

    ``` java
        micro-gw init petstore -a <api definition path>
    ```

        Let's use the [Petstore sample open API definition](https://petstore.swagger.io/)

        **Example**

    ``` java
        micro-gw init petstore -a https://petstore.swagger.io/v2/swagger.json
    ```

        3.  The project is now initialized. A directory by the name "petstore" has been created within the directory where you executed the init command.

        !!! note
        The folder structure is similar to the following.
    ``` java
        petstore
        ├── api_definitions
        └── swagger.json
        ├── conf
        │   └── deployment-config.toml
        ├── extensions
        │   ├── extension_filter.bal
        │   ├── startup_extension.bal
        │   └── token_revocation_extension.bal
        ├── grpc_definitions
        ├── interceptors
        ├── lib
        ├── policies.yaml
    ```

!!! info
    More Information
    -   For more information on the MGW project directory that gets created, see [Project Directory](_Project_Directory_) .
    -   Check out the troubleshooting guide if you run into an issue on [Microgateway project duplication](Troubleshooting_141247127.html#Troubleshooting-proj-duplication) .

##### Step 1.2 - Build the project

1.  Use your command-line tool to navigate to where the project directory ("petstore") was created.  Execute the following command to build the project.
    An executable file ( `            /petstore/target/petstore.jar           ` ) is created to expose the API via WSO2 API Microgateway.

    ``` java
        micro-gw build petstore
    ```

        !!! info
        More information
        Here are some FAQs on [Building a Microgateway project](FAQs_141247125.html#FAQs-BuildingaMicrogatewayproject) .

### Step 2 - Run the API Microgateway

The executable file ( `         .jar        ` ), which includes the API artifacts of the project, is used as input to the WSO2 API Microgateway runtime component in order to expose the APIs.

Follow the steps below to expose the APIs via WSO2 API Microgateway.

1.  Navigate to the  &lt;MGW\_HOME&gt;/bin directory.  &lt;MGW\_HOME&gt; is the installation directory of the WSO2 API Microgateway runtime

    ``` java
        cd <MGW_HOME>/bin
    ```

        2.  Execute the following command to start WSO2 API Microgateway.

        -   [**Format**](#format-store-key-value)
        -   [**Example**](#example-store-key-value)
        -   [**Response**](#response-store-key-value)

        **Format**

    ``` java
        gateway <path-to-MGW-executable-file>
    ```

        **Example**

    ``` java
        ./gateway /Users/kim/petstore-project/target/petstore-.jar
    ```

        When WSO2 Microgateway starts successfully, the following logs are printed on the console.

    ``` java
        ballerina: HTTP access log enabled
        [ballerina/http] started HTTPS/WSS endpoint 0.0.0.0:9096
        [ballerina/http] started HTTPS/WSS endpoint 0.0.0.0:9095
        [ballerina/http] started HTTP/WS endpoint 0.0.0.0:9090
        2019-05-30 18:09:32,540 INFO  [wso2/gateway] - HTTPS listener is active on port 9095 
        2019-05-30 18:09:32,541 INFO  [wso2/gateway] - HTTP listener is active on port 9090 
    ```

        ### Step 3 - Invoke the sample API

        ##### Step 3.1 - Obtain a token

        After the APIs are exposed via WSO2 API Microgateway, you can invoke an API with a valid token(JWT or opaque access token) or an API key.  Let's use WSO2 API Microgateway's API key endpoint to obtain an API key in order to access the API.

        Below command will retrieve a token and set it to the shell variable TOKEN

        **JWT Token**

``` java
    TOKEN=$(curl -X get "https://localhost:9095/apikey" -H "Authorization:Basic YWRtaW46YWRtaW4=" -k)
```

    !!! info
    More information
    -   You can obtain a JWT token from any third-party secure token service or via the WSO2 API Manager.
    -   You can obtain an API Key easily from WSO2 API Microgateway. Follow the documentation to [Obtain an API Key](https://docs.wso2.com/display/MG310/API+Key+Security+Token+Service) .
    -   Alternatively, you can also use an opaque token to invoke the API.
    For more information, see the FAQs on [Working with Tokens](FAQs_141247125.html#FAQs-WorkingwithTokens) .

##### Step 3.2 - Invoke the API

Execute the following command to Invoke the API using the API key. You can now invoke the API running on the Microgateway using cURL as below

**Format**

``` java
    curl -X GET "<MGW-runtime-hostname>:<MGW-runtime-port>/<API-context>/<API-resource>" -H "accept: application/xml" -H "api_key:$TOKEN" -k
```

    **Examples**

``` java
    curl -X GET "https://localhost:9095/v2/pet/1" -H "accept: application/xml" -H "api_key:$TOKEN" -k
```

    !!! note
    You were able to invoke the API resource pet/{petId} using an API Key in api\_key header because the resource is secured with API Key in <https://petstore.swagger.io/v2/swagger.json> API definition as follows. For more information, please refer to the documentation on [API Key Authentication](https://docs.wso2.com/display/MG310/API+Key+Authentication) .
``` java
    paths:
    '/pet/{petId}':
    get:
    security:
    - api_key: []
    securityDefinitions:
    api_key:
    type: apiKey
    name: api_key
    in: header
```

