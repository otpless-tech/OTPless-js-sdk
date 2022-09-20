# otpless-js-sdk




Otpless-js-sdk is a platform agnostic JS SDK used for authenticating users using WhatsApp.



## Features
- Get WhatsApp intent for redirecting users to WhatsApp.
- Validate OTPless token 
- Perform healthcheck

## Getting Started

Create an at www.otpless.com using your business email. Your appId and appSecret will be sent to you on your email.

1. Install the otpless-js-sdk from the npm repository.

    ```sh
    npm i otpless-js-sdk
    ```
2. Initialize the sdk
    ```sh
    import SDK from "otpless-js-sdk";
    const otplessSdk = new SDK({
    appId: YOUR_APP_ID,
    url: "https://api.otpless.app",});
    ```
3. Create a function using `getIntent` method and call it in the onclick of your log into WhatsApp button. This function returns a promise which resolves to a WhatsApp intent. Redirect the user to the intent when onClick event is triggered on your button 
    ```sh
    coonst getIntent=otplessSdk.getIntent({
    redirectionURL: YOUR_WEBSITE_URL, });
    //calling getIntent() would return a promise that resolves to a WhatsApp intent 
    return(
    <div>
    <button onClick=()=> {
    getIntent().then((intent)=> {
        Location.assign(intent);
            })
        }>
        Log in using WhatsApp
    </button>
    </div>)
    ```
    RedirectionURL is an optional parameter
    
4. Once the authentication process is complete, the user would be redirection to your RedirectionURL or the default url set up on the OTPless dashboard. The token would be passed into the queryParam when the redirection happens. 
    1. Clients can use function `startValidation` to extract token from queryParam and validate it. The function returns a promise which resolve to the following object. 
    ```sh
        {
            isValidated:true,
            token:YOUR_TOKEN
        }
    ```
    
    ```sh
    otplessSdk.startValidation().then(({ isValidated, token }) => {
    console.log(isValidated, token);
    if (isValidated) {
      console.log(isValidated, token);
    }});
    ```
    2. Clients can also extract the token on their own and pass it to `validateToken`. This takes in two parameters `token` and `state`. The state is randomly generated by the SDK and can be extracted using the function `getState`. This flow can be used when the Client is using the SDK in Node.js (Server side applications would have to follow this flow). 
    
    ```sh
      otplessSdk.validateToken({"TOKEN_FROM_THE QUERY_PARAM","STATE"}).then(({ isValidated, token }) => {
    if (isValidated) {
      console.log(isValidated, token);
            }
        }
    );
    ```
    
5. The token generated in thelast step can now be used to fetch user data. However user data is only provided on the server side. So the client would have to send the token to their server and fetch the data using function `getUserData` on the server side. The function takes in two params `appSecret` and `token`. Make sure not to share the appSecret in the client side.
    ```sh
    const data = await getUserData({appSecret:YOUR_APP_SECRET,token:YOUR_APP_TOKEN})
    ```


## License

MIT

**Free Software, Hell Yeah!**