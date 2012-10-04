## Summary

This is the iOS SDK of AdjustIo. You can read more about it at [adjust.io](http://www.adjust.io).

## Basic Installation

These are the minimal steps required to integrate the AdjustIo SDK into your iOS project.

### 1. Get the SDK
by downloading the most recent version from our [tags page](https://github.com/adeven/adjust_sdk/tags).

### 2. Add it to your project
by dragging the AdjustIo subdirectory into the "Supporting Files" group in your Xcode project navigator (or any other group of your choice). A dialog box appears for you to "choose options for adding these files". Make sure the checkbox is checked and the upper radio button is selected before you finish.

### 3. Integrate AdjustIo into your app
by adding some code to your AppDelegate.m file. 
* Import the SDK by adding the line `#import "AdjustIo.h"` at the top of the file. 
* Start AdjustIo by adding the line `[AdjustIo appDidLaunch:@"<appId>"];` to your `application:didFinishLaunchingWithOptions:` or your `applicationDidFinishLaunching:` method's body. 
* replace `<appId>` with the Apple ID that is provided by iTunes Connect for your app (Log into iTunes Connect, select "Manage Your Applications", select your app, see the "Apple ID" entry in the App Infomration tab).
* If you want to track the deviceId, add the line `[AdjustIo trackDeviceId];` as well.
* If you want to see debug logs, call `[AdjustIo setLoggingEnabled:YES]`.

### 4. Build your app
* If the build succeeds, you successfully integrated AjdustIo into your app.
* If your project was already using AFNetworking, your build failed because of many duplicate symbols. Just remove the AdjustIO/AFNetworking group to fix this issue.
* If your project uses automatic reference counting, your build failed because of many ARC restriction errors. Fix this by disabling ARC on all AdjustIo files in the target's Build Phases: Expand the "Compile Sources" group, select all AdjustIo files (AjustIo, AI..., ...+AIAdditions, AF..., ...+AFNetworking) and change the "Compilec Flags" to `-fno-objc-arc` (press the return key to change all at once).

## Additional features

Once you integrated the AdjustIo SDK into your project, you can take advantage of the following features wherever you see fit.

### Add tracking of custom events
by adding some code to the methods that get called when something interesting happens. If you want to track how many users press a certain button or reach a particular view, you would ask us for an eventId, like "abc123". In your `buttonDown` or `viewDidAppear` method you would then add the line `[AdjustIo trackEvent:@"abc123"];`. If you register a callback URL for that eventId, like `http://www.adeven.com/callback`, we will send a request there every time the event gets triggered. Additionally, you can provide a dictionary of parameters that we will send back to your callback. All keys and values are expected to be strings.

For example, if you call `[AdjustIo trackEvent:@"abc123" withParameters:@{@"key":@"value",@"foo":@"bar"}];`, we will track the event and call `http://www.adeven/com/callback?key=value&foo=bar`. If you're using Xcode 4.3 or earlier, literal dictionaries won't work and you will have to go with the following version to achieve the same effect:
`[AdjustIo trackEvent:@"abc123" withParameters:[NSDictionary dictionaryWithObjectsAndKeys:@"value",@"key",@"bar",@"foo",nil]]`

Again, you have to import `AdjustIo` to make it work by adding the line `#import "AdjustIo.h"` at the top of the file.

### Add tracking of revenue
by adding some code to the methods that get called after some revenue has been generated. Suppose you want to track an $0.99 In-App Purchase in your app. Every time a purchase gets completed, the `paymentQueue:updatedTransactions:` method of your `SKPaymentTransactionObserver` gets called where the transaction's `transactionState` changed to `SKPaymentTransactionStatePurchased`. In that case you would add `[AdjustIo userGeneratedRevenue:99.0]` to track the generated revenue of 99 cents. Note that the parameter gets rounded to one decimal point. Within that limitation you're free to interpret the value in any currency you like. Again, don't forget to `#include "AdjustIo.h"`.

If you want to differentiate between different kinds of revenue, you can provide an eventId: `[AdjustIo userGeneratedRevenue:99.0 forEvent:@"abc123"];` If that eventId is associated with a callback URL, you can also provide parameters to be forwarded again:
`[AdjustIo userGeneratedRevenue:99.0 forEvent:@"abc123" withParameters:@{@"key":@"value",@"foo":@"bar"}];`

## License

The adjust-sdk is licensed under the MIT License.

Copyright (c) 2012 adeven GmbH, 
http://www.adeven.com

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
