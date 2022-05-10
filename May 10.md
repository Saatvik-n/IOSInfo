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

