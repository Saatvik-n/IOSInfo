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


