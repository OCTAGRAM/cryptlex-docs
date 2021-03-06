# Using LexActivator with Node.js

First of all, login to your Cryptlex account and download LexActivator library for Windows, MacOS or Linux:

* ​[Download LexActivator for Windows](https://app.cryptlex.com/downloads)​
* ​[Download LexActivator for MacOS](https://app.cryptlex.com/downloads)
* ​[Download LexActivator for Linux](https://app.cryptlex.com/downloads)​

The above download package contains the library which you will be using to add licensing to your app.

## Adding licensing to your app <a id="adding-licensing-to-your-app"></a>

After you've added a product for your app in the dashboard, go to the product page of the product you will be adding licensing to. You will need to do two things:

* Note the product id for the product.
* Download the Product.dat for the product.
* Download the example project from [Github](https://github.com/cryptlex/lexactivator-nodejs).

Product.dat contains product data which is used by LexActivator. Product id is the identifier of your product which is to be used in the code.

### Adding library to your app <a id="adding-library-to-your-app"></a>

LexActivator example project for Node.js contains **LexActivator.js** file. You will need to add this file to your Node.js project. It contains all the LexActivator API functions needed to add licensing to your app. Depending on the OS you are targeting you need to copy the respective LexActivator.dll, libLexActivator.so or libLexActivator.dylib to your project.

### Setting product.dat file and product Id <a id="setting-product.dat-file-and-product-id"></a>

The first thing you need to do is either embed the Product.dat file in your app using `SetProductData()` function or set the absolute path of the file using `SetProductFile()` function.

The next thing you need to do is to set the product id of your application in your code using `SetProductId()` function. It sets the id of the product you will be adding licensing to.

```csharp
LexActivator.SetProductData("PASTE_CONTENT_OF_PRODUCT.DAT_FILE");
LexActivator.SetProductId("PASTE_PRODUCT_ID", PermissionFlags.LA_USER);
```

If your app requires admin \(root\) privileges to run \(e.g. services, daemons etc.\), instead of passing   `PermissionFlags.LA_USER` flag, you need to pass `PermissionFlags.LA_SYSTEM` flag.

### License activation <a id="license-activation"></a>

To activate the license in your app using the license key, you will use `ActivateLicense()` LexActivator API function. It invokes the `/v3/activations` Cryptlex Web API endpoint, verifies the encrypted and digitally signed response to validate the license.

```javascript
function activate() {
    let status;
    status = LexActivator.SetLicenseKey("PASTE_LICENCE_KEY");
    if (LexStatusCodes.LA_OK != status) {
        console.log("Error Code:", status);
        process.exit(status);
    }

    status = LexActivator.SetActivationMetadata("key1", "value1");
    if (LexStatusCodes.LA_OK != status) {
        console.log("Error Code:", status);
        process.exit(status);
    }

    status = LexActivator.ActivateLicense();
    if (LexStatusCodes.LA_OK == status || LexStatusCodes.LA_EXPIRED == status || LexStatusCodes.LA_SUSPENDED == status) {
        console.log("License activated successfully:", status);
    } else {
        console.log("License activation failed:", status);
    }
}
```

The above code should be executed at the time of license activation.

### Verifying license activation <a id="verifying-license-activation"></a>

Each time, your app starts, you need to verify whether your license is already activated or not. This verification should occur locally by verifying the cryptographic digital signature of activation. Ideally, it should also asynchronously contact Cryptlex servers to validate and sync the license activation periodically. For this you need to use `IsLicenseGenuine()` LexActivator API function.

```javascript
function main() {
    let status;
    status = LexActivator.SetProductData("PASTE_CONTENT_OF_PRODUCT.DAT_FILE");
    if (LexStatusCodes.LA_OK != status) {
        console.log("Error Code:", status);
        process.exit(status);
    }
    status = LexActivator.SetProductId("PASTE_PRODUCT_ID", PermissionFlags.LA_USER);
    if (LexStatusCodes.LA_OK != status) {
        console.log("Error Code:", status);
        process.exit(status);
    }
    status = LexActivator.IsLicenseGenuine();
    if (LexStatusCodes.LA_OK == status) {
        console.log("License is genuinely activated!");
    } else if (LexStatusCodes.LA_EXPIRED == status) {
        console.log("License is genuinely activated but has expired!");
    } else if (LexStatusCodes.LA_SUSPENDED == status) {
        console.log("License is genuinely activated but has been suspended!");
    } else if (LexStatusCodes.LA_GRACE_PERIOD_OVER == status) {
        console.log("License is genuinely activated but grace period is over!");
    } else {
         console.log("License is not activated:", status);
    }
}
```

The above code should be executed every time user starts the app. After verifying locally, it schedules a periodic server check in a separate thread.

## Need more help <a id="need-more-help"></a>

In case you need more help for adding LexActivator to your app, we'll be glad to help you make the integration. You can either post your questions on our [support forum](https://forums.cryptlex.com) or can contact us through [email](mailto:support@cryptlex.com?Subject=Using%20LexActivator).

