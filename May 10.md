# May 10

## Tab Bar Controller 

Steps to create:
1. Click on View Controller
2. Go to editor
3. Embed in -> Tab Bar Controller

For each bar item, you can change the image, title in Attribute Inspector. 
Images:
* Normal image - when the item  is not highlighted 
* Selected image - when the item is highlighted

Adding view Controller to tab bar:
1. Select tab bar Controller screen, right click and drag to the new view Controller
2. A dialog will appear. Select Relationship segue -> "view controllers"
3. To change the icon, go to the new view Controller and change the icon there

No code for this 

## UITableView

Table view consists of sections. Sections consist of rows.
Ex: In contacts app, there are different sections for people's names beginning with different letters.
Each of these sections will have multiple rows. Each row is called **cell**.

To create prototype cell:
* Go to Attribute Inspector of Table View, Set prototype cells = 1 
* Select the prototype cell, and give an identifier to it 

To use this, `UITableViewDelegate, UITableViewDataSource` protocols must be imported. 
After importing, 2 functions have to be overriden / implemented:
* Number of sections 
* Section header titles

`indexPath` contains 2 variables inside it. `indexPath.row` for the row, `indexPath.section` for the section

Program to make a table view with 2 sections, and 3 rows per section:
```swift
import UIKit

class ViewController: UIViewController, UITableViewDelegate, UITableViewDataSource {

    @IBOutlet weak var tv1: UITableView!
    
    // There will be 2 sections in the table. Each one will have names for the rows and images for each row
    var section1Names: [String] = []
    var section1Images: [String] = []
    
    var section2Names: [String] = []
    var section2Images: [String] = []


    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Names for each row in the section, and image path for each section
        section1Names = ["India", "USA"]
        section1Images = ["image2.jpg", "image.jpg"]
        
        section2Names = ["A", "B"]
        section2Images = ["image2.jpg", "image.jpg"]
        
        tv1.delegate = self
        tv1.dataSource = self
    }
    
    // Number of sections in table
    func numberOfSections(in tableView: UITableView) -> Int {
        return 2;
    }
    
    // Depending on the section, returns the header for the section
    func tableView(_ tableView: UITableView, titleForHeaderInSection section:Int) -> String? {
        if section == 0 {
            return "List of Countries"
        } else {
            return "List of Letters"
        }
    }
    
    // Depending on the section, returns the footer for the section
    func tableView(_ tableView: UITableView, titleForFooterInSection section:Int) -> String? {
        if section == 0 {
            return "List of Countries End"
        } else {
            return "List of Letters End"
        }
    }
    
    // Returns the number of rows for each section
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        if section == 0 {
            return section1Names.count;
        } else {
            return section2Names.count;
        }
    }
    
    // This creates the cells for each section
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        // This is the base cell, with yellow color
        // withIdentifier - a prototype cell was created in Storyboard with an identifier. Put that here
        let cell = tv1.dequeueReusableCell(withIdentifier: "id1", for: indexPath)
        cell.accessoryType = .detailDisclosureButton // An information button will be displayed next to the cell
        cell.backgroundColor = .yellow
        // Depending on the section, set its text and image. Get the text, images from section1Names, section1Images
        if indexPath.section == 0 {
            cell.textLabel?.text = section1Names[indexPath.row]
            cell.imageView?.image = UIImage(named: section1Images[indexPath.row])
            return cell;
        } else {
            cell.textLabel?.text = section2Names[indexPath.row]
            cell.imageView?.image = UIImage(named: section2Images[indexPath.row])
            return cell;
        }
    }

}
```
Other functions:
* `func tableView(_ tableView: UITableView,  heightForRowAt indexPath: IndexPath) -> CGFloat` - changes the height for
each row. Return a `CGFloat`. 
* `func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath)` - calls this function whenever 
a row is selected. Using `indexPath`, different actions can be taken.
* `func tableView(_ tableView: UITableView, accessoryButtonTappedForRowWith indexPath: IndexPath)` - calls this function whenever 
an accesory button (like the information button) is tapped. Using `indexPath`, different actions can be taken.
* `func sectionIndexTitles(for tableView: UITableView) -> [String]?` - It returns an array of strings which are the titles 
for each section. The titles appear on the right side of the table view


## Invoking other applications 

This will invoke safari, Phone, SMS on clicking the button.

```swift
import UIKit

class ViewController: UIViewController {

    // 3 text fields, 3 buttons
    @IBOutlet weak var safaribtn: UIButton!
    @IBOutlet weak var urltf: UITextField!
    @IBOutlet weak var smstb: UIButton!
    @IBOutlet weak var phonebtn: UIButton!
    @IBOutlet weak var smstf: UITextField!
    @IBOutlet weak var phonetf: UITextField!
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }

    @IBAction func onSafariClick() {
        // For safari, the normal url in the format https://www.google.com is enough
        if let url1 = URL(string: urltf.text!) {
            UIApplication.shared.open(url1)
        }
    }
    
    @IBAction func onPhoneClick() {
        // For phone, "tel" needs to be prefixed to the phone number. Won't work in simulator
        if let url1 = URL(string: "tel://\(phonetf.text!)") {
            UIApplication.shared.open(url1)
        }
    }
    
    @IBAction func onSMSClick() {
        // For SMS, "sms" needs to be prefixed to the phone number. Won't work in simulator
        if let url1 = URL(string: "sms://\(smstf.text!)") {
            UIApplication.shared.open(url1)
        }
    }
}

```

## Picker View 

It will let you select from a list of options. UIPickerView can have multiple wheels (named components).
In this example, components = 1.

This program allows user to select a row from the picker. It will contains a city's name.
That name will be set as the text of a label.
```swift
import UIKit

class ViewController: UIViewController, UIPickerViewDelegate, UIPickerViewDataSource {

    // Label displays the selected row
    @IBOutlet weak var label1: UILabel!
    @IBOutlet weak var pv1: UIPickerView!
    
    // The wheel will display a list of cities
    var listOfCities: [String] = []
    override func viewDidLoad() {
        super.viewDidLoad()
        listOfCities = ["Bangalore", "delhi", "Mumbai"]
        pv1.delegate = self
        pv1.dataSource = self
    }
    
    // Number of wheels
    func numberOfComponents(in pickerView: UIPickerView) -> Int {
        return 1;
    }
    
    // Number of rows in each component (wheel). Since there is only one component, no. of rows is constant
    func pickerView(_ pickerView: UIPickerView, numberOfRowsInComponent component: Int) -> Int {
        return listOfCities.count;
    }
    
    // Title for each row
    func pickerView(_ pickerView: UIPickerView, titleForRow row: Int, forComponent component: Int) -> String? {
        return listOfCities[row];
    }
    
    // This function is called each time a row is selcted.
    // It sets the label text to the selected city
    func pickerView(_ pickerView: UIPickerView, didSelectRow row: Int, inComponent component: Int) {
        label1.text = listOfCities[row]
    }
}
```

### To utilize pickerView in textField 

Picker view occupies too much space. To solve this, you can bring up pickerView seperately when 
a textField is clicked. 

```swift 
class ViewController: UIViewController, UIPickerViewDelegate, UIPickerViewDataSource {
    @IBOutlet weak var tf1: UITextField!
    var pv1: UIPickerView

    override func viewDidLoad() {
        super.viewDidLoad()
        
    }
}
```

