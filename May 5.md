# May 5 

## Keyboard hiding 

Keyboard can be hidden when:
* UI is clicked
* Enter key is clicked 

Code:
```swift
class ViewController: UIViewController, UITextFieldDelegate {
    // UITextFieldDelegate - gets delegate of UI Text Field


    // This gets a reference to a textfield made in the storyboard
    // It is created by dragging and dropping
    @IBOutlet weak var tf2: UITextField! 
    @IBOutlet weak var tf1: UITextField!
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Because of UITextFieldDelegate, these lines can work
        tf1.delegate = self
        tf2.delegate = self
    }


    // These methods are imported from UITextFieldDelegate, and overridden 
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

