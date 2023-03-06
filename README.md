# Applylar iOS SDK Integration

Appylar framework for iOS is a lightweight and easy-to-use Ad integration SDK provided by Appylar. The SDK enables developers to integrate Appylar Ads in any type of iOS application.

Appylar provides several types of Ads and enables you to place Ads wherever you want in the application.

The Ads provided by Appylar are:
 - Banners
 - Interstitials
 
## General Requirements
 - Minimum targeted version must be iOS 12.0 and later.
 
# Step 1: Add Appylar to your project bundle

Make sure `Pods` is available in your project's directory.
Then import the class in your UIViewController Class

```swift
import UIKit
import Appylar
```


# Step 2: Setup the configuration for your App and Listeners


1. Create an extension of UIViewController(), override its viewDidLoad() method, and implement the `AdEventListener` protocol. You can, of course, use your Application subclass if you already have one in your project.

```swift
import UIKit
import Appylar

class ViewController: UIViewController{
      	Override func viewDidLoad(){
            	super.viewDidLoad()  
            	Appylar.adeventlistener = self  //Attach callback listeners for SDK before initialization
            	//Here ‘adeventlister’ is a variable of AdEventListener
            	//Initialization
            	……
         }
}
extension ViewController: AdEventListener {
    	func onInitialized(token: String) {
              	//Callback for successful initialization
      	}

     	func onError(description: String) {
                //Callback for error thrown by SDK
        }
}
```

2. Initialize SDK with configuration:
```swift
import UIKit
import Appylar

class ViewController: UIViewController{
      	Override func viewDidLoad(){
		super.viewDidLoad()  
            	Appylar.adeventlistener = self //Attach callback listeners for SDK before initialization
            	//Here ‘adeventlister’ is a variable of AdEventListener
           	//Initialization
           	Appylar.initializeWithApiKey(testmode: true // ‘True’ for development and ‘False’ for production,                         
          		app_Key: "<YOUR_APP_KEY>"?? “”, //APP KEY provided by console for Development use    ["OwDmESooYtY2kNPotIuhiQ"]
           		app_id: "", 
 			orientations: [Orientation.PORTRAIT, Orientation.LANDSCAPE], 	//Supported orientations for Ads 
		        Adtypes: [AdType.BANNER, AdType.INTERSTITIAL]	//Types of Ads to integrate 
			)
          }
}

```

3. Another method is available for parameter customization:
```swift
Override func viewDidLoad(){
   	super.viewDidLoad()  
     	Appylar.adeventlistener = self //Attach callback listeners for SDK before initialization
      	//Here ‘adeventlister’ is a variable of AdEventListener
	//Initialization
     	Appylar.setParameters(bannerHeight: selectedHeightOfBanner // Height is given by user [“50”,”90],
 		ageRestriction: SelectedAge //Age is given by user[“12”,”15”,”18”] )    
     }
```

# Step 3: Add BannerView to the application

1. To integrate the BannerView component in your design prepare a view from storyboard and of BannerView type, prefer below snippet:

![Banner View Image](https://github.com/Management5Exceptions/ProjectManagement/blob/main/ReadmeImage/BannerViewOutlet.png)
   
Steps:
   
  1. Drag a view from library.
  2. At there attribute inspector assign `BannerView` as a class and `Appylar` in module.
  3. Create there outlet of type BannerView.

```swift
@IBOutlet weak var bannerView: BannerView!
```	 

2. Bind component to your activity and implement callback listeners.

```swift
func onNoad(){
    	//Callback for when there is no Ad to show.  
    }
func onLoadAds() {
    	//Callback for when Ads loads successfully.
    }
```

3. Check Ad availability and show the Ad.

![Banner View](https://github.com/Management5Exceptions/ProjectManagement/blob/main/ReadmeImage/ClickShowBanner.png)
```swift
   if Appylar.canShowAd(Adtype: .banner) == true {  
     	if self.selectedPosition == .top {              
  	    self.topBannerView.showAds(position: .top)               
 	    self.constraintHeightTopBannerView.constant = CGFloat(self.selectedHeightOfBanner)	
            }
    }else {
      	  self.onNoad() // if canShowAd function returns false then a onNoad event is fired.
   }
```

4. For hiding the banner at the run time:
![Hide Banner View](https://github.com/Management5Exceptions/ProjectManagement/blob/main/ReadmeImage/HideBanner.png)
```swift
   bannerView.hideBanner()
```

# Step 4: Add Interstitial to the application


1. create a `ViewController` of type `AdViewController` and Add code for orientation lock.
  ![Orientation Lock](https://github.com/Management5Exceptions/ProjectManagement/blob/main/ReadmeImage/HideBanner.png)
  
  After that override function in AppDelegate for orientation lock of interstitial ad.
  
  ![Orientation Lock](https://github.com/Management5Exceptions/ProjectManagement/blob/main/ReadmeImage/HideBanner.png)

2. Implement callback listeners for Interstitial.
```swift
func onNoad(){
    	//Callback for when there is no Ad to show.  
   }
func onLoadAds() {
    	//Callback for when Ads loads successfully.
   }
func onInterstitialClosed() {
    	//Callback for close event of interstitial
   }
```	

3. Check Ad availablity and show the Ad.


![Interstitial Ad](https://github.com/Management5Exceptions/ProjectManagement/blob/main/ReadmeImage/ShowInterstitial.png)


```swift
if  Appylar.canShowAd(Adtype: .interstitial) == true {
   	let storyBoard : UIStoryboard = UIStoryboard(name: "Main", bundle:nil)
   	let <Your_Controller_Name> = storyBoard.instantiateViewController(withIdentifier: "<Your_Controller_Identifier>") as! <Your_Controller_Name>
      	<Your_Controller_Name>.restrictRotation = appDelegate.restrictRotation ?? .all
        self.navigationController?.pushViewController(<Your_Controller_Name>, animated: false)
} else {
   	self.onNoad() // if canShowAd function returns false then a onNoad event is fired.
}
```

# Sample Codes

1. For initialization
	
```swift
import UIKit
import Appylar
		
class ViewController: UIViewController{
    	Override func viewDidLoad() {
		super.viewDidLoad()  
            	Appylar.adeventlistener = self //Attach callback listeners for SDK before initialization
            	//Here ‘adeventlister’ is a variable of AdEventListener
           	//Initialization
           	Appylar.initializeWithApiKey(testmode: true // ‘True’ for development and ‘False’ for production,                         
         	app_Key: "<YOUR_APP_KEY>"?? “” //APP KEY provide by console for Development use    ["OwDmESooYtY2kNPotIuhiQ"] ,
           	app_id: "", 
 		orientations: [Orientation.PORTRAIT, Orientation.LANDSCAPE] 	//Supported orientations for Ads, 
		Adtypes: [AdType.BANNER, AdType.INTERSTITIAL]	//What type of Ads you want to integrate )
     	}
}

```

2. For design:

```swift
import UIKit
import Appylar

class ViewController: UIViewController {
	func setUI() {
        	self.btnBannerPositionTop.isSelected = true
        	self.btnAdTypeBanner.isSelected = true
        	self.btnAdTypeInterstitial.isSelected = true
        	self.btnOrientationPortrait.isSelected = true
        	self.btnOrientationLandScape.isSelected = true
        
        	self.selectedHeightOfBanner = 50
        	self.selectedAge = 12
        	if #available(iOS 13.0, *) {
            		self.txtviewOutput.layer.borderColor = CGColor(red: 0, green: 0, blue: 0, alpha: 1)
        	}
        	self.txtviewOutput.layer.borderWidth = 2
        	self.txtviewOutput.layer.cornerRadius = 5
        
        	self.scrollview.layer.borderWidth = 2
        	self.scrollview.layer.cornerRadius = 5
        	setValueInArrays()
    	        }
}
```

3.  Implements for both the types:

```swift
import UIKit
import Appylar

class ViewController: UIViewController { 
    	@IBAction func btnBannerositionTopDidTapped(_ sender: UIButton) {
        	btnBannerPositionTop.isSelected = true
        	self.constraintHeightBottomBannerView.constant = 0
        	self.bottomBannerView.hideBanner()
        	btnBannerPostionBottom.isSelected = false
        	setValueInArrays()
   	}
    
    	@IBAction func btnBannerositionBottomDidTapped(_ sender: UIButton) {
		btnBannerPostionBottom.isSelected = true
        	self.constraintHeightTopBannerView.constant = 0
        	self.topBannerView.hideBanner()
        	btnBannerPositionTop.isSelected = false
        	setValueInArrays()
   	}
        
    	@IBAction func btnInitDidTapped(_ sender: UIButton) {
            	Appylar.initializeWithApiKey(testmode: true, app_Key: self.txtfieldApiKey.text ?? "", app_id: "", orientations: self.selectedOrientations,                 Adtypes: self.selectedAdTypes)
    	 }
    
    	@IBAction func btnShowBannerDidTapped(_ sender: UIButton) {
        	showBanner()
    	}
    
    	@IBAction func btnHideBannerDidTapped(_ sender: UIButton) {
        	self.topBannerView.hideBanner()
        	self.bottomBannerView.hideBanner()
        	self.constraintHeightBottomBannerView.constant = 0
        	self.constraintHeightTopBannerView.constant = 0
        	self.view.layoutIfNeeded()
    	}
    
    	@IBAction func btnShowIntersitialDidTapped(_ sender: UIButton) {

        	if  Appylar.canShowAd(Adtype: .interstitial) == true {
            		let storyBoard : UIStoryboard = UIStoryboard(name: "Main", bundle:nil)
            		let nextViewController = storyBoard.instantiateViewController(withIdentifier: "WebViewController") as! WebViewController
            		nextViewController.restrictRotation = appDelegate.restrictRotation ?? .all
            		self.navigationController?.pushViewController(nextViewController, animated: false)
       		} else {
            		self.onNoAd()
        	}
   	}
}
extension ViewController : AdEventListener {
		
 	func onInitialized(token: String) {
                 AddLogsToTextView(logs: "Received session token:- \(token)")
        }
    
        func onError(description: String) {
                AddLogsToTextView(logs: "Error:- \(description)")
        }
	func onNoAd() {
        	AddLogsToTextView(logs: "No Ads in buffer")
    	}
    
    	func onLoadAds() {
       		AddLogsToTextView(logs: "Advertisements loaded")
    	}
    
   	func onAdShown(type: Appylar_SDK_iOS.AdType) {
        	AddLogsToTextView(logs: "\(type.rawValue) displayed")
    	}	
    
    	func onInterstitialClosed() {
        	AddLogsToTextView(logs: "interstitial closed")
    	} 
}

```
