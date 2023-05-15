# Acquiring tokens interactively

## System Browser

MSAL supports acquiring tokens interactively on public clients through use of the system OS browser. MSAL will start the system browser as a separate process, and open the authorization URL. MSAL does have have control over this browser, but once the user finishes authentication, the web page is redirected in such a way such that MSAL can intercept the response from the authority. 

MSAL will listen on "http://localhost:port" and intercept the code that authority sends when the user is done authenticating. MSAL cannot detect if the user navigates away or simply closes the browser. Apps using this technique are encouraged to define a timeout. We recommend a timeout of at least a few minutes to take into account cases where the user is prompted to change password or perform 2FA. 

## How to use the default OS browser

During app registration, configure `http://localhost` as a redirect URI. In the case of B2C, you will need to register a specific port `http://locahost:port`.

```java
        PublicClientApplication publicClientApplication =
                PublicClientApplication
                        .builder(CLIENT_ID)
                        .authority(AUTHORITY)
                        .build();

        InteractiveRequestParameters parameters = InteractiveRequestParameters
                .builder(new URI("http://localhost"))
                .scopes(scope)
                .build();

        IAuthenticationResult result = publicClientApplication.acquireToken(parameters).join();

```

 ## Customizing the experience 

MSAL attempts to open the default system browser, if one is available, on the users machine. You can customize this logic by providing your own implementation of `OpenBrowserAction`

- Implement `OpenBrowserAction`
- Pass custom action to `SystemBrowserOptions`

```java
    class CustomOpenBrowserAction implements OpenBrowserAction {

        @Override
        public void openBrowser(URL url){
            //Custom logic to open URL 
        }
    }

        SystemBrowserOptions options =  
                SystemBrowserOptions
                        .builder()
                        .openBrowserAction(new CustomOpenBrowserAction())
                        .build();

```

You can also customize the HTML message that the user sees after authenticating or even provide a URL where you want the user to be redirected to after authenticating.

```java

        SystemBrowserOptions options =  
                SystemBrowserOptions
                        .builder()
                        .htmlMessageSuccess("CUSTOM_HTML_HERE")
                        .htmlMessageError("CUSTOM_HTML_HERE")
                        .build();
        // OR

        SystemBrowserOptions options =
                SystemBrowserOptions
                        .builder()
                        .browserRedirectSuccess(new URI("http://localhost:port"))
                        .browserRedirectError(new URI ("http://localhost:port"))
                        .build();

```




