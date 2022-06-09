# SedraCheck-iOS-sample
<p align="center">
  <img src="https://github.com/SedraPay/SedraCheck/blob/main/SedraCheck.png" alt="Icon"/>
</p>
<H1 align="center">SedraCheck</H1>

The new eKYC in simple way.

`SedraCheck` is between your hands to help you onboard your customer easily with almost no effort.


## Video

<a href="https://youtu.be/8oehz24fXI4" target="_blank"><img src="https://i.ytimg.com/vi/8oehz24fXI4/maxresdefault.jpg"
alt="SedraCheck Demo Video" width="480" height="360" border="10" /></a>



## Requirements
[![Platform iOS](https://img.shields.io/badge/Platform-iOS-blue.svg?style=fla)]()



## Installation
==========================

SedraCheck is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod 'SedraCheck'

#also add this
post_install do |installer_representation|
    installer_representation.pods_project.targets.each do |target|
        target.build_configurations.each do |config|
            config.build_settings['ONLY_ACTIVE_ARCH'] = 'NO'
            config.build_settings['BUILD_LIBRARY_FOR_DISTRIBUTION'] = 'YES'
        end
    end
end
```

Then install it in terminal using below lines:

pod install

-- OR --

pod install --repo-update


## Add below line into your Info.plist

```xml
<key>NSCameraUsageDescription</key>
<string>$(PRODUCT_NAME) {camera usage description and why the app needs to use it}.</string>
```


### Lets Start coding

## First step is required to have so you will create session and can use below steps

###### Create Journey ######

```swift
import SedraCheck

//Mandatory step to add

override func viewDidLoad(){
    super.viewDidLoad()
    
    //assign the delegate to your viewController
    SedraCheck.shared.delegate = self
    
/// Below function is required to continue using the framework, you have to enter all parameters to let the framework works fine
/// - Parameters:
///   - serverKey: this key will be from the portal
///   - serverURLString: the base url that sent to you by sales team
///   - needLog: this will show the errors in the debug
///   - journeyType: this is enum that contains 3 types (.unknown, .new, .update) to show the user is new or updating the profile.
///      If you are not intersted to check the type don't add it or set the type as .unknown

    SedraCheck.shared.setSettings(serverKey: "<YOUR_SERVER_KEY>", serverURLString: "<YOUR_GIVEN_SERVER>", true, .update) 
}

func getNationalities() {
/// Below function is not required, in case you need to get all the nationalities you can call it using:
 SedraCheck.shared.delegate = self
 
  SedraCheck.shared.getNationalities()


}

extension <YOUR_VIEW_CONTROLLER>: SedraCheckJourneyDelegate{
    func didFinishCreatingJourneyWithError(error: SedraCheckError){
        //do your own code as:
        //dismiss dialogs, loadings
        //recall the function
    }
    func didFinishCreatingJourneyWithSuccess(journeyId: String) {
        //do your own code as:
        //dismiss dialog, loadings
        //save the journey if needed as a reference to your server to check user from our protal
    }
    
    ///Optional if you call the getNationalities API
    func didGetNationalitiesWithSuccess(response:GetNationalities) {
       //do your own code as:
      //dismiss dialog, loadings
      // save the nationalities Response in your own array if needed : self.nationalitiesResponse = response
    }
    
    func didGetNationalitiesWithError(error: SedraCheckError) {
          //do your own code as:
        //dismiss dialogs, loadings
        //recall the function
    }
    
}
```
###### END OF CREATE JOURNEY ######


###### Sedra Check ######

If you need to let the user capture the document (id, passport), use below code:

```swift

//put this code when you need to capture the document.
@objc func myButtonAction(_ sender: UIButton){
    SedraCheck.documentsCheck.delegate = self

    /// Below function is for ocr the document and get the information of the user.
    /// - Parameters:
    
    ///   - nationality: this is an object of Nationality struct, you can make your own object, or use the result of the getNationalities API you hit after you create your journey, and choose the nationality you've selected.
    
    /// documentType:  this is an object of NationalityIDType struct, you can make your own object, or use the result of the getNationalities API you hit        after you create your journey, and choose the nationality type id you've selected.
    
    ///   - configuration: of type ConfigureScanDocumentsViews whitch contains 3 objects type will be declared down 

    SedraCheck.documentsCheck.captureDocuments(nationality:Nationality, documentType: NationalityIDType, configuration: ConfigureScanDocumentsViews)
}

extension <YOUR_VIEW_CONTROLLER>: SedraCheckDocumentsDelegate{
    func userDidCloseCamera(){
    
    }
    func userFinishCapturingDocument(documents: [SedraCheckDocument]){
    
    }
    func userFinishCapturingDocumentsWithResponse(documents: [SedraCheckDocument], response: SedraCheckDocumentVerificationResponse){
    
    }
    func userFinishCapturingDocumentsWithError(documents: [SedraCheckDocument], , error: SedraCheckError){
    
    }
    func didFinishWithError(error: SedraCheckError){

    }
}
```
###### END OF SEDRA CHECK ######

###### Sedra Configuration ######
If you need to configure the Documents Pages, use below code:
```swift

/// ConfigureScanDocumentsViews whitch contains 3 objects type will be declared down:

public struct ConfigureScanDocumentsViews {
    public var ConfigureDocumentsCameraPage: ConfigureDocumentsCameraPage? = nil
    public var ConfigureDocumentsEditPage: ConfigureDocumentsEditPage? = nil
    public var ConfigureDocumentsPreviewPage: ConfigureDocumentsPreviewPage? = nil
}

  ```

 ```swift
FIRST
/// ConfigureDocumentsCameraPage: this is the first object which configure all attributes in the Camera Page, use below code with default values:

public struct ConfigureDocumentsCameraPage {
    //Camera Page Attributes
    public var cameraViewBackgroundColor: UIColor? = .black
    public var topHintCameraLabelColor:UIColor? = .white
    public var topHintCameraLabelTitle:String? = NSLocalizedString("Please get close to the ID/Passport so it would fill the empty area", comment: "")
    public var topHintCameraIsHidden:Bool? = false
    public var topHintCameraLabelNumberOfLines:Int? = 0
    
    public var frontIDLabelColor:UIColor? = .white
    public var frontIDLabelTitle:String? =  NSLocalizedString("Scan your ID front face", comment: "")
    public var frontIDIsHidden:Bool? = false
    
    public var backIDLabelColor:UIColor? = .white
    public var backIDLabelTitle:String? =  NSLocalizedString("Scan your ID Back face", comment: "")
    public var backIDIsHidden:Bool? = false
    
    public var passportLabelColor:UIColor? = .white
    public var passportLabelTitle:String? =  NSLocalizedString("Scan your passport", comment: "")
    public var passportIsHidden:Bool? = false
    
    public var frontDrivingLicenseLabelColor:UIColor? = .white
    public var frontDrivingLicenseLabelTitle:String? =  NSLocalizedString("Scan your Driving front face", comment: "")
    public var frontDrivingLicenseIsHidden:Bool? = false
    
    public var backDrivingLicenseLabelColor:UIColor? = .white
    public var backDrivingLicenseLabelTitle:String? =  NSLocalizedString("Scan your Driving back face", comment: "")
    public var backDrivingLicenseIsHidden:Bool? = false
    
    public var documentTypeLabelNumberOfLines:Int? = 0
    public var fontNameAndSize: UIFont? = .systemFont(ofSize: 13)
    
    public var captureButtonImage: UIImage? = nil
    public var captureButtonImageURL: String? = ""
    public var captureButtonIsHidden:Bool? = false
    public var captureButtonTitle:String? = ""
    public var captureButtonColor:UIColor? = .clear
    public var captureButtonFontColor:UIColor? = .white
    public var captureButtonFontNameAndSize:UIFont? = .systemFont(ofSize: 13)
    public var captureButtonImageTintColor:UIColor? = .white
    
    public var closeButtonImageURL:String? = ""
    public var closeButtonImage:UIImage? = nil
    public var closeButtonIsHidden:Bool? = false
    public var closeButtonTitle:String? = ""
    public var closeButtonColor:UIColor? = .clear
    public var closeButtonFontColor:UIColor? = .white
    public var closeButtonFontNameAndSize:UIFont? = .systemFont(ofSize: 13)
    public var closeButtonImageTintColor:UIColor? = .white
    
    public var flashButtonImageURL:String? = ""
    public var flashButtonImage:UIImage? = nil
    public var flashButtonIsHidden:Bool? = false
    public var flashButtonTitle:String? = ""
    public var flashButtonColor:UIColor? = .clear
    public var flashButtonFontColor:UIColor? = .white
    public var flashButtonFontNameAndSize:UIFont? = .systemFont(ofSize: 13)
    public var flashButtonImageTintColor:UIColor? = .white
    }
 
 ```
 
 ```swift
SECOND
/// ConfigureDocumentsEditPage: this is the second object which configure all attributes in the edit Page, use below code with default values:

public struct ConfigureDocumentsEditPage {
    public var editPageBackgroundColor: UIColor? = .black
    public var outlinesCroppingColor:UIColor? = .red
    
    public var backButtonImage:UIImage? = nil
    public var backButtonImageURL:String? = ""
    public var backButtonTitle:String? = ""
    public var backButtonIsHidden:Bool? = false
    public var backButtonColor:UIColor? = .clear
    public var backButtonFontColor:UIColor? = .white
    public var backButtonFontNameAndSize:UIFont? = .systemFont(ofSize: 13)
    public var backButtonImageTintColor:UIColor? = .white
    
    public var cropButtonImage:UIImage? = nil
    public var cropButtonImageURL:String? = ""
    public var cropButtonTitle:String? = ""
    public var cropButtonIsHidden:Bool? = false
    public var cropButtonColor:UIColor? = .clear
    public var cropButtonFontColor:UIColor? = .white
    public var cropButtonFontNameAndSize:UIFont? = .systemFont(ofSize: 13)
    public var cropButtonImageTintColor:UIColor? = .white
    }
 ```
 
 ```swift
 THIRD
/// ConfigureDocumentsPreviewPage: this is the third object which configure all attributes in the preview Page, use below code with default values:
 
 public struct ConfigureDocumentsPreviewPage {
    public var previewPageBackgroundColor: UIColor? = .black
    
    public var reviewLabelTitle:String? = ""
    public var reviewLabelColor:UIColor = .white
    public var reviewLabelNumberOfLine:Int? = 0
    public var reviewLabelIsHidden:Bool? = false
    
    public var editScanButtonImage:UIImage? = nil
    public var editScanButtonImageURL:String? = ""
    public var editScanButtonTitle:String? = ""
    public var editScanButtonIsHidden:Bool? = false
    public var editScanButtonColor:UIColor? = .clear
    public var editButtonFontColor:UIColor? = .white
    public var editButtonFontNameAndSize:UIFont? = .systemFont(ofSize: 13)
    public var editScanButtonImageTintColor:UIColor? = .white
    
    public var confirmButtonImage:UIImage? = nil
    public var confirmButtonImageURL:String? = ""
    public var confirmButtonTitle:String? = ""
    public var confirmButtonIsHidden:Bool? = false
    public var confirmScanButtonColor:UIColor? = .clear
    public var confirmButtonFontColor:UIColor? = .white
    public var confirmButtonFontNameAndSize:UIFont? = .systemFont(ofSize: 13)
    public var confirmScanButtonImageTintColor:UIColor? = .white
    
    public var rotateButtonImage:UIImage? = nil
    public var rotateButtonImageURL:String? = ""
    public var rotateButtonTitle:String? = ""
    public var rotateButtonIsHidden:Bool? = false
    public var rotateScanButtonColor:UIColor? = .clear
    public var rotateButtonFontColor:UIColor? = .white
    public var rotateButtonFontNameAndSize:UIFont? = .systemFont(ofSize: 13)
    public var rotateScanButtonImageTintColor:UIColor? = .white
    }
```

###### END OF SEDRA CHECK ######

###### Sedra Liveness Check ######

If you need to check user liveness and take a selfie, use below code:

```swift

//put this code when you need to check liveness.
@objc func myButtonAction(_ sender: UIButton){
    SedraCheck.livenessCheck.delegate = self


    /// Below function is for checking the liveness of the user and take a photo for the user.
    /// - Parameters:
    ///   - viewController: current viewController

    SedraCheck.livenessCheck.checkLiveness(viewController: self)
}

extension <YOUR_VIEW_CONTROLLER>: SedraCheckLivenessCheckDelegate{
    func didPressCancel(){
    
    }
    func didGetImageSuccessfully(data: UIImage){
    
    }
    func didGetImageMatchingResponseSuccessfully(response: ImageMatchingResponse){
    
    }
    func didGetError(errorMessage: String){
    
    }
    func LivenessCheckPageError(error: SedraCheckError){
        
    }
}
```
###### END OF SEDRA LIVENESS CHECK ######


###### Sedra Comply ######


```swift

//put this code when you need to check your user in the world check.
@objc func myButtonAction(_ sender: UIButton){
    SedraCheck.comply.delegate = self

    /// Below function is for screening and checking the customer.
    /// - Parameters:
    ///   - firstName: enter the first name of the user <Required>
    ///   - secondName: enter the second name of the user <Optional>, leave empty string if not needed
    ///   - thirdName: enter the third name of the user <Optional>, leave empty string if not needed
    ///   - lastName: enter the last name of the user <Required>

    SedraCheck.comply.screenCustomer(firstName: "<FIRST_NAME_HERE>",
                                    secondName: "<SECOND_NAME_HERE>",
                                    thirdName: "<THIRD_NAME_HERE>",
                                    lastName: "<LAST_NAME_HERE>")
}

extension <YOUR_VIEW_CONTROLLER>: SedraComplyDelegate{
    func screeningFinishedWithSuccess(response: SedraCheckScreeningResponse){
        //do your code here
    }
    
    func screeningFinishedWithError(message: SedraCheckError){
        //do your code here
    }
}
```
###### END OF SEDRA COMPLY ######

###### Sedra Close Journey ######


```swift

//put this code when you need to close your journey.

@objc func myButtonAction(_ sender: UIButton){
     SedraCheck.closeJourney.delegate = self
              

    /// Below function is for closing the journey.
    /// - Parameters:
    ///   - customerId: this parameter to know your user id so you can compare it from our portal if needed

  SedraCheck.closeJourney.closeJourneyAPI(customerId: "<CUSTOMER_ID_HERE>" )
}

extension <YOUR_VIEW_CONTROLLER>: SedraCheckCloseJourneyDelegate{
    func didFinishCloseJourneyWithSuccess(){
        //do your code here
    }
    
    func didFinishCloseJourneyWithError(message: SedraCheckError){
        //do your code here
    }
}
```
###### END OF SEDRA CLOSE JOURNEY ######


Localization
==========================
Check localizable.string file in the project and translate it in the way you love.


Contact Us & Report a Bug
==========================

If you have any questions or you want to contact us, visit our website.

https://sedracheck.sedrapay.com/


--- OR ---

Contact us via email mob@sedrapay.com

