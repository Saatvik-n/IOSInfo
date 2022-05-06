# May 6 

## UIView 

A View can have subviews. Each subview can have multiple elements inside it (like a hierarchy).
For each element inside a subview, then its x, y coordinates are relative to the origin of the subview, 
not the device.

Program - show different view based on the selected value of segmented control.
This uses `isHidden` property of a view to set all views to hidden, then change the visibility 
of the views based on the segmented control's `selectedSegmentIndex`

```swift
class ViewController: UIViewController {

    @IBOutlet weak var view2: UIView!
    @IBOutlet weak var label2: UILabel!
    @IBOutlet weak var view1: UIView!
    @IBOutlet weak var label1: UILabel!
    @IBOutlet weak var sc1: UISegmentedControl!
    
    
    override func viewDidLoad() {
        super.viewDidLoad()

        // Set both views to hidden
        view1.isHidden = true
        view2.isHidden = true
    }

    // Change visibility of views based on click
    @IBAction func onSegClick(_ sender: UISegmentedControl) {
        let selectedIndex = sc1.selectedSegmentIndex
        if selectedIndex == 0 {
            view1.isHidden = false
            view2.isHidden = true
        } else if selectedIndex == 1 {
            view1.isHidden = true
            view2.isHidden = false
        }
    }
}
```

## Scroll View

You can scroll horizontally and vertically inside it. 
This is done by using `contentSize` property. Ex:
`sv.contentSize = CGSize(width: 500, height: 600)` means that 
the content inside it can strech up to that amount of height and width.

## Activity Indicator View 

This is used for showing the progress of an Activity. It has options:
animating, hide when stopped. 
When an Activity is ongoing, it keeps spinning. It does not show any specific progres.

This program starts the timer when the view is loaded. The Activity View also starts spinning.
After 5 seconds, the Activity view stops spinning and disappears.


```swift
class ViewController: UIViewController {

    @IBOutlet weak var av1: UIActivityIndicatorView!
    var timer1: Timer!
    override func viewDidLoad() {
        super.viewDidLoad()
        av1.startAnimating()
        
        // Selector - method to call when timer is done 
        // repeats - false because timer should not repeat
        timer1 = Timer.scheduledTimer(timeInterval: 5.0, target: self, selector: #selector(stopActivity), userInfo: nil, repeats: false)
    }
    
    @objc func stopActivity() {
        av1.stopAnimating()
        av1.hidesWhenStopped = true
    }
}
```


## Assigment 

4 textfields: username, email, password, number. Each should have different keyboard type.
On button click, they should save the data to the user defaults. On application load, 
they should load the data into the text fields.

```swift
import UIKit

class ViewController: UIViewController, UITextFieldDelegate {
    
    var tf1: UITextField!
    var tf2: UITextField!
    var tf3: UITextField!
    var tf4: UITextField!
    
    var label1: UILabel!
    var label2: UILabel!
    var label3: UILabel!
    var label4: UILabel!
    
    var btn1: UIButton!
    
    var ud1:UserDefaults!

    override func viewDidLoad() {
        super.viewDidLoad()        
        self.view.backgroundColor = .red
        
        ud1 = UserDefaults.standard
        
        // 4 text fields in a row
        tf1 = UITextField(frame: CGRect(x: 50, y: 100, width: 300, height: 20))
        tf2 = UITextField(frame: CGRect(x: 50, y: 150, width: 300, height: 20))
        tf3 = UITextField(frame: CGRect(x: 50, y: 200, width: 300, height: 20))
        tf4 = UITextField(frame: CGRect(x: 50, y: 250, width: 300, height: 20))
        
        // Labels are slightly above them
        label1 = UILabel(frame: CGRect(x: 50, y: 80, width: 100, height: 20))
        label2 = UILabel(frame: CGRect(x: 50, y: 120, width: 100, height: 20))
        label3 = UILabel(frame: CGRect(x: 50, y: 170, width: 100, height: 20))
        label4 = UILabel(frame: CGRect(x: 50, y: 220, width: 100, height: 20))
        
        btn1 = UIButton(frame: CGRect(x: 50, y: 300, width: 300, height: 30))
        // To set the title of button
        btn1.setTitle("Save Defaults", for: .normal)
        // To customize the button
        btn1.backgroundColor = .white
        btn1.setTitleColor(.black, for: .normal)
        // saveInfo is called on button click, for touching the inside of the button
        btn1.addTarget(self, action: #selector(saveInfo), for: .touchUpInside)
        
        label1.text = "Name"
        label2.text = "Email"
        label3.text = "Number"
        label4.text = "Password"
        
        // This is required
        tf1.borderStyle = UITextField.BorderStyle.line
        tf2.borderStyle = UITextField.BorderStyle.line
        tf3.borderStyle = UITextField.BorderStyle.line
        tf4.borderStyle = UITextField.BorderStyle.line
        
        // This is required
        tf1.backgroundColor = UIColor.white
        tf2.backgroundColor = UIColor.white
        tf3.backgroundColor = UIColor.white
        tf4.backgroundColor = UIColor.white
        
        // For tf1 - tf3, UIKeyboardType can be used
        tf1.keyboardType = UIKeyboardType.default
        tf2.keyboardType = UIKeyboardType.emailAddress
        tf3.keyboardType = UIKeyboardType.phonePad

        // For password field, this needs to be done
        tf4.isSecureTextEntry = true
        
        // Load the default values
        tf1.text = ud1.string(forKey: "name")
        tf2.text = ud1.string(forKey: "email")
        tf3.text = ud1.string(forKey: "number")
        tf4.text = ud1.string(forKey: "password")
        
        // The use of this is unknown
        tf1.delegate = self
        tf2.delegate = self
        tf3.delegate = self
        tf4.delegate = self
        
        self.view.addSubview(tf1)
        self.view.addSubview(tf2)
        self.view.addSubview(tf3)
        self.view.addSubview(tf4)
        
        self.view.addSubview(label1)
        self.view.addSubview(label2)
        self.view.addSubview(label3)
        self.view.addSubview(label4)
        
        self.view.addSubview(btn1)
    }
    
    // This function is called when button is pressed, to save the value into user defaults
    @objc func saveInfo() {
        // tf1.text is an Optional, you need to get the value
        ud1.setValue(tf1.text!, forKey: "name")
        ud1.setValue(tf2.text!, forKey: "email")
        ud1.setValue(tf3.text!, forKey: "number")
        ud1.setValue(tf4.text!, forKey: "password")
       
    }

}

```
