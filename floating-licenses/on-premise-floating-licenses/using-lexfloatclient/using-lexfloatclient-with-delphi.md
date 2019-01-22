# Using LexFloatClient with Delphi

First of all, login to your Cryptlex account and download LexFloatClient library for Windows:

* [Download LexFloatClient for Windows](https://app.cryptlex.com/downloads)

The above download package contains the library which you will be using to add licensing to your app.

## Adding licensing to your app

After you've added a product for your app in the dashboard, go to the product page of the product you will be adding licensing to. You will need to do two things:

* Note the product id for the product.
* Download the example project from [Github](https://github.com/cryptlex/lexfloatclient-delphi)

Product id is the identifier of your product which is to be used in the code. The product id of the LexFloatServer and LexFloatClient must match.

### Adding library to your app

LexFloatClient example project for Delphi \(7 or newer\) contains the **LexFloatClient.pas** unit file. In addition to that it includes **LexFloatClient.DelphiFeatures.pas** unit file used internally.

You need to add these files to your app in order to use LexFloatClient API in your app. Both units must be added, but only LexFloatClient unit must be added to uses list.

### Setting product id

The first LexFloatClient API procedure you need to use in your code is `LexFloatClient.SetHostProductId`. It sets the product id of the product you will be adding licensing to. 


```delphi
SetHostProductId('PASTE_PRODUCT_ID');
```

### Requesting license lease

To receive a floating license, you will use `LexFloatClient.SetHostUrl`, `LexFloatClient.SetFloatingLicenseCallback` and `LexFloatClient.RequestFloatingLicense` LexFloatClient API methods. It sets LexFloatServer address, callback for status notifications, contacts the server and receives the floating license.

```delphi
var
  Step: string;
begin
  try
    Step := 'SetHostProductId';
    SetHostProductId('PASTE_PRODUCT_ID');
    Step := 'SetHostUrl'; SetHostUrl('http://localhost:8090');
    Step := 'SetFloatingLicenseCallback';
    // console application has no message loop, thus Synchronized is False
    SetFloatingLicenseCallback(LicenseRenewCallback, False);
    try
      Step := 'RequestFloatingLicense'; RequestFloatingLicense;
      try
        WriteLn;
        Write('Success! License Acquired. Press Enter to continue...'); ReadLn;
        //
        // do some work here
        //
        WriteLn;
        Write('Press Enter to drop the license...'); ReadLn;
        Step := 'DropFloatingLicense';
      finally
        DropFloatingLicense; // drop acquired license
      end;
      WriteLn;
      WriteLn('Success! License dropped.');
      Step := 'ResetFloatingLicenseCallback';
    finally
      ResetFloatingLicenseCallback; // remove callback
    end;
    Write('Press Enter to continue...'); ReadLn;
  except
    on E: Exception do
    begin
      WriteLn;
      WriteLn('Exception from ', Step, ': ', ScopedClassName(E.ClassType));
      WriteLn(E.Message);
      raise;
    end;
  end;
end;
```

The above code can be executed every time user starts the app or needs a new license.

The second argument of `LexFloatClient.SetFloatingLicenseCallback` is False because console applications have no message loop. In GUI applications `Synchronized` is recommended to be `True` to enforce execution in main thread.

### Renewing license lease

License lease automatically renews itself in a background thread. When something goes wrong, Callback is invoked \(from background thread\). Callback can be either procedure, object method, class method or a closure \(also known as anonymous function\). Note that GUI applications cannot safely interact with GUI elements from a thread other than GUI one. `System.Classes.TThread.Synchronize` will be used if `Synchronized` was `True` when callback was set. On another hand, `Synchronized` cannot work without message loop in main thread (e.g. in console applications), so synchronization must be performed in another way then.

```delphi
procedure LicenseRenewCallback(const Error: Exception);
begin
  // No synchronization, write everything to console
  WriteLn;
  if Assigned(Error) then
  begin
    WriteLn('Asynchronous exception: ',
      ScopedClassName(Error.ClassType));
    WriteLn(Error.Message);
  end else begin
    WriteLn('Asynchronous success');
  end;
end;
```

You would ideally request for a new license if Callback gets invoked.

### Dropping license lease

When your user is done using the app, the app should send a request to free the license, thereby making it available for other users. If the app doesn't, the license becomes \(zombie\) useless until lease time is over.

GUI \(e.g. VCL\) applications are supposed to divide the code into initialization and finalization. `LexFloatClient.ResetFloatingLicenseCallback` should be called before callback is going to become invalid. E.g. form method is invalid after form is destroyed, so putting `ResetFloatingLicenseCallback` in `TForm.OnDestroy` is a proper place. `LexFloatClient.DropFloatingLicense` should also be invoked, but beware of exceptions it can raise. Sample code:

```delphi
procedure TForm1.OnDestroy;
begin
  try
    DropFloatingLicense;
  except
    // write to log, display warning dialog, ignore or handle any other way
  end;
  ResetFloatingLicenseCallback;
end;
```

The above code should be executed every time user closes the app.

## Need more help

In case you need more help for adding LexActivator to your app, we'll be glad to help you make the integration. You can either post your questions on our [support forum](https://forums.cryptlex.com) or can contact us through [email](mailto:support@cryptlex.com?Subject=Using%20LexFloatClient).
