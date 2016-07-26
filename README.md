# What

# Why

# How

## Pre-requisites

### Create an Okta org

### Create an Apigee Edge account

## Setting up Okta

### Create OIDC app

-   Create app
-   Assign to user

### Testing that it works

## Setting up Apigee

### Log in to Apigee

1.  After logging in to your Apigee account, you will see a
    dashboard listing "API Management", "API Baas", "API Consoles",
    and "Insights"
2.  Click on "Launch" in the API Management area.
    If it is your first using Apigee, you will need to create an
    Apigee organization first:
    -   Click on "Activate" in the API Management area.
    -   If you are prompted, click "Activate" to create
        your Apigee organization.
    -   After activation, you will have to wait a few minutes for your
        Apigee organization to be created.
3.  You will see a dashboard for the API Management area with a menu
    along the top of the screen.

### Create a cache

-   Create a cache in "prod"

    1.  From the "APIs" menu, select the "Environment Configuration" option.
    2.  You should be in the "Caches" section
    3.  Click on the "Edit" button on the right of the "Caches" section
    4.  Click the "+ Cache" button that will appear where the "Edit"
        button was.
    5.  Enter `jwk_cache` into "Name" box.
    6.  Enter `Cache for JWKs responses` in the "Description" box.
    7.  Leave the defaults for "Expiration Type" and "Expiration".
    8.  Click the "Save" button.

-   Create a cache in "test"

    1.  From the "APIs" menu, select the "Environment Configuration" option.
    2.  You should be in the "Caches" section
    3.  You should see a menu icon next to the "prod" text, to the
        right of the "Environment Configuration" text.
    4.  Click on the "prod" menu described above, then select "test"
    5.  Click on the "Edit" button on the right of the "Caches" section
    6.  Click the "+ Cache" button that will appear where the "Edit"
        button was.
    7.  Enter `jwk_cache` into "Name" box.
    8.  Enter `Cache for JWKs responses` in the "Description" box.
    9.  Leave the defaults for "Expiration Type" and "Expiration".
    10. Click the "Save" button.

### Create an "Okta OIDC" Product

1.  From the "Publish" menu, select "Products"
2.  Click the "+ Product" button found on the right hand side of
    the screen.
3.  Set the following:
    -   Name: Set to: "`Okta OIDC`"
    -   Display Name: Set to: "`Okta OIDC`"
    -   Environment: Select both "test" and "prod"
    -   Access: Set to "Internal Only"
    -   In the "API Proxies" section, click the "+ API Proxy" button,
        then select "okta-oidc-jwt-bearer"
    -   Leave all other settings to their defaults.
4.  Click the "Save" button.

### Create a developer account

1.  From the "Publish" menu, select "Developer"
2.  Click the "+ Developer" button.
3.  Set the "First Name", "Last Name", "Email", and "Username"
    fields to your name, email address, and preferred
    username.
4.  Click the "Save" button.

### Create an "Okta App"

1.  From the "Publish" menu, select "Developer Apps".
2.  Click the "+ Developer App" button.
3.  Set the Name to "Okta App"
4.  Set the Developer to the developer account you created above.
5.  In the Products section, select the "Okta OIDC" product.
6.  Click the "Save" button.

### Get the OAuth `client_id` and `client_secret`

1.  Click on the "Okta App" text in the "Developer Apps" section.
2.  In the Products section, you should see empty "Consumer Key" and
    "Consumer Secret" fields, with a "Show" button next to each.
3.  Click the "Show" button next to the empty "Consumer Key" and
    "Consumer Secret" fields.
4.  Copy down the "Consumer Key" and "Consumer Secret" values,
    you will need them soon.

### Create an API Proxy bundle

1.  Clone this repository to your machine:
    
        git clone git@github.com:jpf/okta-apigee-oidc.git
2.  Change to the `okta-apigee-oidc` directory:
    
        cd okta-apigee-oidc
3.  Create the `okta-oidc-jwt-bearer-apiproxy.zip` file:
    
        zip -r okta-oidc-jwt-bearer-apiproxy.zip apiproxy/

### Upload the example API Proxy bundle to Apigee

1.  Find the "APIs" menu, and select "API Proxy" from the menu.
2.  On the right hand side of the screen, click the "+ API Proxy" button.
3.  You will be presented with a list of options for creating the
    API proxy. Select "Proxy Bundle"
4.  After you select "Proxy Bundle", click the "Next" button.
5.  You will be prompted to "Specify the proxy details"
6.  Click the "Choose File" button and select the
    `okta-oidc-jwt-bearer.zip` file you just created.
7.  Leave the "Proxy Name" as "okta-oidc-jwt-bearer"
8.  Click the "Next" button.
9.  Verify your settings, then click the "Build" button.
10. You should see a green dialog saying "✓ Uploaded Proxy"
11. Click on the link in the text that reads: "View okta-oidc-jwt-bearer proxy in the editor"

### Modify the uploaded API Proxy bundle

1.  From the "APIs" menu, select "API Proxies"
2.  Click on the blue text for the "okta-oidc-jwt-bearer" API proxy
3.  Click on the "Develop" tab, located to the right of the "Overview" tab.
4.  Find the "Policies" area on the left hand side of the screen.
5.  In the Policies area, find and click on the "Configure OAuth" Policy.
6.  Using the Consumer Key (`client_id`) and Consumer Secret
    (`client_secret`) you created earlier, replace the "client\_id"
    and "client\_secret" values in the XML.
7.  Replace the text `ADD YOUR CLIENT ID HERE` with the "Consumer
    Key" you created earlier.
8.  Replace the text `ADD YOUR CLIENT SECRET HERE` with the "Consumer
    Secret" you created earlier.
9.  Click the "Save" button on the upper left hand side of the
    screen.

### Deploy the API Proxy to the "test" environment

1.  From the "APIs" menu, select "API Proxies"
2.  Click on the blue text for the "okta-oidc-jwt-bearer" API proxy
3.  Click on the "Develop" tab, located to the right of the "Overview" tab.
4.  In the "Deployment" menu in the middle of the screen, select "test"
5.  You will be prompted to "Deploy API Proxy."
6.  Click the "Deploy" button.

### Try it out

1.  From the "APIs" menu, select "API Proxies"
2.  Click on the blue text for the "okta-oidc-jwt-bearer" API proxy
3.  You should see the dashboard for the "okta-oidc-jwt-bearer" API
    proxy
4.  In the "Deployments" section of the dashboard, find the URL
    for the API proxy that you created, this URL should end with
    `-test.apigee.net/jwt-bearer` take note of the full domain for
    this URL, you will be using it below.
5.  Run the command below, replacing the domain in the URL with the
    domain copied from the URL in the step above. 
    
        curl -d assertion=test -d grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer http://example-test.apigee.net/jwt-bearer/oauth/accesstoken
    
    Normally, we would set the `assertion` POST parameter to the
    value of an `id_token`, however we are setting this to the
    invalid JWT value of "test" to make sure that we get an error
    back from Apigee. The error that we get back should look like this:
    
        {"fault":{"faultstring":"Execution of Get-Key-ID-and-Issuer failed with error: Exception thrown from JavaScript : Error: Invalid id_token (Get_Key_ID_and_Issuer_js#75)","detail":{"errorcode":"steps.javascript.ScriptExecutionFailed"}}}
6.  Re-run the command again, but with a valid value for the
    `assertion` parameter:
    
    The first thing that we'll want to do is fetch a valid `id_token`
    for our domain. You can do this using a tool like the [Okta
    Sign-In Widget](http://developer.okta.com/docs/guides/okta_sign-in_widget) or the `get_id_token.sh` shell script per below:
    
        get_id_token.sh -b "https://example.oktapreview.com" -c "aBCdEf0GhiJkLMno1pq2" -u "example.user" -p "Abcdefgh0" -o "https://example.com"
    
    Once you have a valid `id_token`, use it in the `curl` command
    again to exchange the `id_token` for an Apigee access token:
    
        curl -d assertion=$id_token -d grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer http://example-test.apigee.net/jwt-bearer/oauth/accesstoken
    
    If everything is configured correctly, you will get a response
    from Apigee that looks like the below:
    
        {
          "issued_at" : "1469142055119",
          "application_name" : "01abc234-d567-8901-2345-e67890123f45",
          "scope" : "",
          "status" : "approved",
          "api_product_list" : "[Okta OIDC]",
          "expires_in" : "3599",
          "developer.email" : "okta.developer@example.com",
          "token_type" : "BearerToken",
          "client_id" : "aBcDefGHijKlmnopqrStUVwXYZabcDE0",
          "access_token" : "AbCD0efGhIJKlMNoPqrSTUvWXyZa",
          "organization_name" : "example",
          "refresh_token_expires_in" : "0",
          "refresh_count" : "0"
        }