[![](https://jitpack.io/v/VNPTSmartCA/android-sdk.svg)](https://jitpack.io/#VNPTSmartCA/android-sdk)
# android-sdk-example

This is android project example to connect VNPTSmartCA SDK using Java

## Requirements

At a minimum, this SDK is designed to work with Android SDK 16.

### Installation

To use the VNPT SmartCA Android SDK, add the compile dependency with the latest version.

***Step 1:*** Import SDK and add JitPack repository to your `build.gradle`

```gradle
allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}
```

Add the dependency:

```gradle
implementation 'com.github.VNPTSmartCA:android-sdk:1.0.3'
```

***Step 2:*** Config in AndroidManifest.xml file

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

***Step 3:*** Build layout Connect VNPT SmartCA Activity

```java
import com.vnpt.egov.vnptsmartcaandroidsdk.ParameterNameTransaction;
import com.vnpt.egov.vnptsmartcaandroidsdk.Transaction;

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_mapping);
    /*
    * You need choose environment to develop.
    * Have 2 choices: 
    * Transaction.ENVIRONMENT.DEVELOPMENT will connect to dev/test server
    * Transaction.*ENVIRONMENT.PRODUCTION will connect to production server
    * */
    Transaction.getInstance().setEnvironment(Transaction.ENVIRONMENT.PRODUCTION);
}
```

***Step 4:*** Init transaction required info as clientId and tranId, then connect to VNPT SmartCA App

```java

// You need pass your transaction ID replace for 'transactionId'.
private final String tranId = "transaction ID";
// You need pass your client ID replace for 'partnerSchemeId'. 
private final String clientId = "partner ID"; 

private void requestMapping() {
    Map<String, String> eventValue = new HashMap<>();
    //Client Required
    eventValue.put(ParameterNameTransaction.CLIENT_ID, clientId);
    eventValue.put(ParameterNameTransaction.TRAN_ID, tranId);
    Transaction.getInstance().requestVNPTSmartCACallback(this, eventValue);
}
```

***Step 5:*** Get callback from VNPT SmartCA app

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if(requestCode == Transaction.getInstance().REQUEST_CODE_VNPT_SMARTCA && resultCode == 0) {
        tvStatusCode.setText("Status Code: " + data.getExtras().getInt("status"));
        tvMessage.setText("Message: " + data.getExtras().getString("message"));
    } else {
        tvMessage.setText("message: " + this.getString(R.string.not_receive_info_err));
    }
}
```

***Request param***

| Param    | Description                                             |
|----------|---------------------------------------------------------|
| tranId   | ID' transaction                                         |
| clientId | Your ID in VNPTSmartCA' system. Provided by VNPTSmartCA |

***Response status code table***

| Code  | Description                                |
|-------|--------------------------------------------|
| 0     | Success                                    |
| 1     | User rejected                              |
| 60000 | Credential not exist                       |
| 60001 | Credential not match identity              |
| 60002 | Credential no result                       |
| 60003 | Credential status invalid                  |
| 61000 | Credential assign key failed               |
| 62000 | Signature transaction not found            |
| 62001 | Signature transaction not match identity   |
| 62002 | Signature transaction expired              |
| 62003 | Signature transaction not waiting          |
| 62010 | Signature data request invalid format      |
| 63000 | Credential sign signer authen failed       |
| 63001 | Credential sign init hash signer failed    |
| 63002 | Credential sign file upload failed         |
| 64000 | Credential sign file not support file type |
| 64001 | Credential acceptance generate file failed |
| 64002 | Credential acceptance transaction exist    |

## Author

VNPT SmartCA Development Team

## Copyright©

[Copyright (c) 2021 VNPTSmartCA](https://github.com/VNPTSmartCA/android-sdk-example/blob/master/LICENSE).

## Version

1.0.3

## Contact - Support

email: hoangdinhoi@vnpt.vn
