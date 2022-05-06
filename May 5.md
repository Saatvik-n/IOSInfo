# May 5 

## Keyboard hiding 

Keyboard can be hidden when:
* UI is clicked - `touchesBegan`
* Enter key is clicked - `textFieldShouldReturn`

Code:
```swift
class ViewController: UIViewController, UITextFieldDelegate {
    // UITextFieldDelegate - gets delegate of UI Text Field

    // This gets a reference to a textfield made in the storyboard
    // It is created by dragging and dropping textfield onto the code
    @IBOutlet weak var tf2: UITextField! 
    @IBOutlet weak var tf1: UITextField!
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Because of UITextFieldDelegate, these lines can work
        tf1.delegate = self
        tf2.delegate = self
    }
    // These methods (touchesBegan, textFieldShouldReturn) are imported from UITextFieldDelegate, and overridden 
    // resignFirstResponder - it closes the keyboard

    // When the UI is touched, then close the keyboar
    override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
        tf1.resignFirstResponder()
        tf2.resignFirstResponder()
    }
    
    // When return key is pressed, close the keyboard
    func textFieldShouldReturn(_ textField: UITextField) -> Bool {
        textField.resignFirstResponder()
    }

}
```

## Switch Actions 

```swift 
class ViewController: UIViewController {

    @IBOutlet weak var l1: UILabel!
    @IBOutlet weak var s1: UISwitch!
    override func viewDidLoad() {
        super.viewDidLoad()
      
    }

    /**
    * Right click on switch component in story board 
    * Right click on action, and drag the line to code
    */
    @IBAction func click1(_ sender: UISwitch) {
        if (s1.isOn) {
            l1.text = "Switch Is On"
        } else {
            l1.text = "Switch is OFF"
        }
    }
    
}
```

## Change alpha value of image with slider 


```swift
class ViewController: UIViewController {

    @IBOutlet weak var sl1: UISlider!
    @IBOutlet weak var iv1: UIImageView!
    override func viewDidLoad() {
        super.viewDidLoad()
    }

    // This function is called when the slider is moved
    @IBAction func sliderMove(_ sender: UISlider) {

        // Gets the value of slider from 0 - 1
        let value1:Float = sl1.value
        // Sets the alpha of the image view to the slider value
        iv1.alpha = CGFloat(value1)
    }
}
```

## Segmented Control 

It has 2 or more segments. Each segment has index value from 0 - (n - 1).
Its properties:
* Selected - should it be selected by default 
* Enabled - should it be Enabled or disabled 

Here, 4 controls with different colors are created. 
On clicking each control, background color changes.
The number of controls was specified in storyboards.

```swift
class ViewController: UIViewController {

    @IBOutlet weak var sc1: UISegmentedControl!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loadi
    }

    @IBAction func onSegmentClick() {
        // This method gets the selected index of the segmented control 
        // The indices start from 0 to ...
        let indexValue = sc1.selectedSegmentIndex
        switch indexValue {
            // Set BG color based on selected value
        case 0:
            self.view.backgroundColor = .blue
        case 1:
            self.view.backgroundColor = .orange
        case 2:
            self.view.backgroundColor = .red
        case 3:
            self.view.backgroundColor = .yellow
        // Each switch requires default case
        default:
            // Empty default case
            ()
        }
    }
}
```

## Page Control 

Page control is represented by dots. It allows you to move between different pages. 
The dots don't represent pages, they represent actions.

Difference between segmented control and Page control - in segmented control, user can go 
to any page at any time. But in Page control, he can only move one page forward and backward at a time.

```swift
class ViewController: UIViewController {
    @IBOutlet weak var pc1: UIPageControl!
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }

    // This is called on each click on the page control
    @IBAction func pc1Click() {
        // This gets the current page of the control
        let pageValue = pc1.currentPage
        if pageValue == 0 {
            self.view.backgroundColor = .yellow
        } else if pageValue == 1 {
            self.view.backgroundColor = .blue
        } else if pageValue == 2 {
            self.view.backgroundColor = .red
        }
    }
}
```

### Page control assignment 

On changing page, a different image should display.

```swift
class ViewController: UIViewController {
    @IBOutlet weak var iv1: UIImageView!
    @IBOutlet weak var pc1: UIPageControl!
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }


    @IBAction func pc1Click() {
        let pageValue = pc1.currentPage
        if pageValue == 0 {
            // This loads the image from the main path (where image gets stored when it is copied from finder)
            iv1.image = UIImage(named: "image.png")
        } else if pageValue == 1 {
            iv1.image = UIImage(named: "image1.jpg")
        } else if pageValue == 2 {
            iv1.image = UIImage(named: "image2.jpg")
        }
    }

}
```

## Stepper 

Stepper is a control which can increment and decrement a value.
Attributes: value, min, max, step (the increment or decrement amount)

```swift
class ViewController: UIViewController {
    @IBOutlet weak var l1: UILabel!
    @IBOutlet weak var st1: UIStepper!
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }

    // Execute this function every time stepper changes value
    @IBAction func stepperClick(_ sender: UIStepper) {
        // Get the value (numerical) from the stepper
        let stepperVal = Int(st1.value)
        // Set the label text
        l1.text = "\(stepperVal)"
    }
}
```

## Web Kit WebView 

It allows use of WebView in an app.

Code:
```swift
class ViewController: UIViewController {

    @IBOutlet weak var wv1: WKWebView!
    
    var url1: URL!
    var request1: URLRequest!
    override func viewDidLoad() {
        super.viewDidLoad()
        url1 = URL(string: "https://www.apple.com")
        request1 = URLRequest(url: url1)
        wv1.load(request1)
    }

}
```

## User defaults 

The UserDefaults class provides a programmatic interface for interacting with the **defaults system**. The defaults system allows an app to customize its behavior to match a user’s preferences. For example, you can allow users to specify their preferred units of measurement

This contains username, password fields, and a button.
* On clicking button, the username, and password will be saved as the user's defaults.
* On loading the view, the username, and password will be retrived from the user's defaults


```swift
class ViewController: UIViewController {

    @IBOutlet weak var username: UITextField!
    @IBOutlet weak var passwor: UITextField!
    @IBOutlet weak var button: UIButton!
    var defaults1: UserDefaults!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        defaults1 = UserDefaults.standard
        
        // On load, retrieve saved defaults
        username.text = defaults1.string(forKey: "name")
        passwor.text = defaults1.string(forKey: "password")

    }

    // Called when button is clicked
    // Save key value pairs for username, password
    @IBAction func saveInfo(_ sender: UIButton) {
        defaults1.setValue(username.text, forKey: "name")
        defaults1.setValue(passwor.text, forKey: "password")
        
    }
    
}
```
