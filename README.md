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


1. Create an extension of UIViewController(), override its viewDidLoad() method, and implement the `AppylarDelegate` protocol. You can, of course, use your Application subclass if you already have one in your project.

```swift
import UIKit
import Appylar

class ViewController: UIViewController{
      	Override func viewDidLoad(){
            	super.viewDidLoad()  
                AppylarManager.setEventListener(delegate: self,bannerDelegate: self,interstitialDelegate: self)  //Attach callback listeners for SDK before initialization
            	//Here ‘setEventListener’ is a method for AppylarManager
            	//Initialization
            	……
         }
}
extension ViewController: AppylarDelegate {
    	func onInitialized() {
              	//Callback for successful initialization
      	}

     	func onError(error: String) {
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
                AppylarManager.setEventListener(delegate: self,bannerDelegate: self,interstitialDelegate: self)  //Attach callback listeners for SDK before initialization
            	//Here ‘setEventListener’ is a method for AppylarManager//Initialization
           	AppylarManager.Init(                        
          		app_Key: "<YOUR_APP_KEY>"?? “”, //APP KEY provided by console for Development use    ["OwDmESooYtY2kNPotIuhiQ"]
 			Adtypes: [AdType.BANNER, AdType.INTERSTITIAL]	//Types of Ads to integrate
			orientations: [Orientation.PORTRAIT, Orientation.LANDSCAPE], 	//Supported orientations for Ads
			testmode: true // ‘True’ for development and ‘False’ for production, 
			)
          }
}

```

3. Another method is available for parameter customization:
```swift
Override func viewDidLoad(){
   	super.viewDidLoad()  
     	AppylarManager.setEventListener(delegate: self,bannerDelegate: self,interstitialDelegate: self)//Attach callback listeners for SDK before initialization
        //Here ‘setEventListener’ is a method for AppylarManager
	//Initialization
     	AppylarManager.setParameters(dict: [
            "banner_height" : self.selectedHeightOfBanner != nil ? ["\(String(self.selectedHeightOfBanner!))"] : nil, // Height is given by user [“50”,”90]
            "age_restriction" : self.selectedAge != nil ? ["\(String(self.selectedAge!))"] : nil //Age is given by user[“12”,”15”,”18”] 
        ])  
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
func onNoBanner(){
    	//Callback for when there is no Ad to show.  
   }
func onBannerShown() {
       //Callback for Ad shown.  
   }
```

3. Check Ad availability and show the Ad.

   ![Banner View](https://github.com/Management5Exceptions/ProjectManagement/blob/main/ReadmeImage/bannerButton.png)
   
```swift
   if BannerView.canShowAd(){             
  	    self.bannerView.showAd(placement: "String" ?? "" )            
    }
```

4. For hiding the banner at the run time:
   
   ![Hide Banner View](https://github.com/Management5Exceptions/ProjectManagement/blob/main/ReadmeImage/hideBanner.png)
   
```swift
   bannerView.hideBanner()
```

# Step 4: Add Interstitial to the application


1. Make your `ViewController` of type `InterstitialViewController`.  
```swift
   class ViewController: InterstitialViewController
```

2. Implement callback listeners for Interstitial.
```swift
func onNoInterstitial(){
    	//Callback for when there is no Ad to show.  
   }
func onInterstitialShown() {
       //Callback for Ad shown.  
   }
func onInterstitialClosed() {
    	//Callback for close event of interstitial
   }
```	

3. Check Ad availablity and show the Ad.


   ![Interstitial Ad](https://github.com/Management5Exceptions/ProjectManagement/blob/main/ReadmeImage/interstitialAd.png)


```swift
if  InterstitialViewController.canShowAd(){
   	 if InterstitialViewController.canShowAd(){
            self.showAd(placement: String ?? "")
        }
}
```
4. For lock the orientation of interstitial create a file `AppDelegate` and create a structure in it.
```swift
      func application(_ application: UIApplication, supportedInterfaceOrientationsFor window: UIWindow?) -> UIInterfaceOrientationMask {
           return AppylarManager.supportedOrientation
      }
```

# Sample Codes

1. For initialization
	
```swift
import UIKit
import Appylar
		
class ViewController: InterstitialViewController{
    	Override func viewDidLoad() {
		super.viewDidLoad()  
                AppylarManager.setEventListener(delegate: self,bannerDelegate: self,interstitialDelegate: self) //Attach callback listeners for SDK before initialization
            	//Here ‘setEventListener’ is a method for AppylarManager//Initialization
           	AppylarManager.Init(                        
          		app_Key: "<YOUR_APP_KEY>"?? “”, //APP KEY provided by console for Development use    ["OwDmESooYtY2kNPotIuhiQ"]
 			Adtypes: [AdType.BANNER, AdType.INTERSTITIAL]	//Types of Ads to integrate
			orientations: [Orientation.PORTRAIT, Orientation.LANDSCAPE], 	//Supported orientations for Ads
			testmode: true // ‘True’ for development and ‘False’ for production, 
			)
     	}
}

```

2. For design:

```swift
import UIKit
import Appylar

class ViewController: InterstitialViewController {
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

class ViewController: InterstitialViewController { 
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
            	AppylarManager.Init(app_Key: self.txtfieldApiKey.text ?? "",Adtypes: self.selectedAdTypes,orientations:      self.selectedOrientations,testmode: true)
    	 }
    
    	@IBAction func btnShowBannerDidTapped(_ sender: UIButton) {
            if BannerView.canShowAd(){             
  	         self.bannerView.showAd(placement: txtfieldEnterPlacement.text ?? "" )            
            }
    	 }
    
    	@IBAction func btnHideBannerDidTapped(_ sender: UIButton) {
        	self.bannerView.hideBanner()
        	self.view.layoutIfNeeded()
    	}
    
    	@IBAction func btnShowIntersitialDidTapped(_ sender: UIButton) {
        	if  InterstitialViewController.canShowAd(){
            		self.showAd(placement: self.txtfieldEnterPlacement.text ?? "")
       		} 
   	}
}
extension ViewController : AppylarDelegate {
    func onInitialized() {
        AddLogsToTextView(logs: "onInitialized() ")
    }
    
    func onError(error : String) {
        AddLogsToTextView(logs: "onError() - \(error)")
    }
}  
   
extension ViewController: BannerViewDelegate{
    func onNoBanner() {
        AddLogsToTextView(logs: "onNoBanner()")
    }
    
    func onBannerShown() {
        AddLogsToTextView(logs: "onBannerShown()")
    }
    
    
}

extension ViewController: InterstitialDelegate{
    func onNoInterstitial() {
        AddLogsToTextView(logs: "onNoInterstitial()")
    }
    
    func onInterstitialShown() {
        AddLogsToTextView(logs: "onInterstitialShown()")
    }
    
    func onInterstitialClosed() {
        AddLogsToTextView(logs: "onInterstitialClosed()")
    }
}
```
