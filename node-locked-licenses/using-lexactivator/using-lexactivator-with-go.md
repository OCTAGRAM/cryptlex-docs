# Using LexActivator with Go

First of all, login to your Cryptlex account and download LexActivator library for Windows, MacOS or Linux:

* ​[Download LexActivator for Windows](https://app.cryptlex.com/downloads)​
* ​[Download LexActivator for MacOS](https://app.cryptlex.com/downloads)
* ​[Download LexActivator for Linux](https://app.cryptlex.com/downloads)​

The above download package contains the library which you will be using to add licensing to your app.

## Adding licensing to your app <a id="adding-licensing-to-your-app"></a>

After you've added a product for your app in the dashboard, go to the product page of the product you will be adding licensing to. You will need to do two things:

* Note the product id for the product.
* Download the Product.dat for the product.
* Download the example project from [Github](https://github.com/cryptlex/lexactivator-go).

Product.dat contains product data which is used by LexActivator. Product id is the identifier of your product which is to be used in the code.

### Adding library to your app <a id="adding-library-to-your-app"></a>

LexActivator example project for Go contains **LexActivator.h** file. You will need to add this file to your Go  project. It contains all the LexActivator API functions needed to add licensing to your app. Depending on the OS you are targeting you need to copy the respective LexActivator.dll, libLexActivator.so or libLexActivator.dylib to your project.

### Setting product.dat file and product Id <a id="setting-product.dat-file-and-product-id"></a>

The first thing you need to do is either embed the Product.dat file in your app using `SetProductData()` function or set the absolute path of the file using `SetProductFile()` function.

The next thing you need to do is to set the product id of your application in your code using `SetProductId()` function. It sets the id of the product you will be adding licensing to.

```go
C.SetProductData(C.CString("PASTE_CONTENT_OF_PRODUCT.DAT_FILE"))
C.SetProductId(C.CString("PASTE_PRODUCT_ID", C.LA_USER))
```

If your app requires admin \(root\) privileges to run \(e.g. services, daemons etc.\), instead of passing   `C.LA_USER` flag, you need to pass `C.LA_SYSTEM` flag.

### License activation <a id="license-activation"></a>

To activate the license in your app using the license key, you will use `ActivateLicense()` LexActivator API function. It invokes the `/v3/activations` Cryptlex Web API endpoint, verifies the encrypted and digitally signed response to validate the license.

```go
func activate() {
	var status C.int
	status = C.SetLicenseKey(C.CString("PASTE_LICENCE_KEY"))
	if C.LA_OK != status {
		fmt.Println("Error Code:", status)
		os.Exit(1)
	}

	status = C.SetActivationMetadata(C.CString("key1"), C.CString("value1"))
	if C.LA_OK != status {
		fmt.Println("Error Code:", status)
		os.Exit(1)
	}

	status = C.ActivateLicense()
	if C.LA_OK == status || C.LA_EXPIRED == status || C.LA_SUSPENDED == status {
		fmt.Println("License activated successfully:", status)
	} else {
		fmt.Println("License activation failed:", status)
	}
}
```

The above code should be executed at the time of license activation.

### Verifying license activation <a id="verifying-license-activation"></a>

Each time, your app starts, you need to verify whether your license is already activated or not. This verification should occur locally by verifying the cryptographic digital signature of activation. Ideally, it should also asynchronously contact Cryptlex servers to validate and sync the license activation periodically. For this you need to use `IsLicenseGenuine()` LexActivator API function.

```go
func main() {
	var status C.int
	status = C.SetProductData(C.CString("PASTE_CONTENT_OF_PRODUCT.DAT_FILE"))
	if C.LA_OK != status {
		fmt.Println("Error Code:", status)
		os.Exit(1)
	}
	status = C.SetProductId(C.CString("PASTE_PRODUCT_ID"), C.LA_USER)
	if C.LA_OK != status {
		fmt.Println("Error Code:", status)
		os.Exit(1)
	}
	status = C.SetAppVersion(C.CString("PASTE_YOUR_APP_VERION"))
	if C.LA_OK != status {
		fmt.Println("Error Code:", status)
		os.Exit(1)
	}
	status = C.IsLicenseGenuine()
	if C.LA_OK == status {
		fmt.Println("License is genuinely activated!")
	} else if C.LA_EXPIRED == status {
		fmt.Println("License is genuinely activated but has expired!")
	} else if C.LA_SUSPENDED == status {
		fmt.Println("License is genuinely activated but has been suspended!")
	} else if C.LA_GRACE_PERIOD_OVER == status {
		fmt.Println("License is genuinely activated but grace period is over!")
	} else {
		 fmt.Println("License is not activated:", status)
	}
}
```

The above code should be executed every time user starts the app. After verifying locally, it schedules a periodic server check in a separate thread.

## Need more help <a id="need-more-help"></a>

In case you need more help for adding LexActivator to your app, we'll be glad to help you make the integration. You can either post your questions on our [support forum](https://cryptlex.com/forums) or can contact us through [email](mailto:support@cryptlex.com?Subject=Using%20LexActivator).

