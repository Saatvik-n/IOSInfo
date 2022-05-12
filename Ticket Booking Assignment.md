# Ticket Booking Assignment 

3 screens:
* First screen - button to go the next screen (no code)
* Second screen - display tableView with 3 sections (rail, road, air). Each section should have
cells which contains website name, image. On selecting a cell and clicking button, go to the next view 
* Third screen - display webview with the URL of the website selected in 2nd screen 

## Second screen code 

```swift
import UIKit

class SecondScreenViewController: UIViewController, UITableViewDelegate, UITableViewDataSource {

    @IBOutlet weak var tv1: UITableView!
    
    // Titles, URLs, image paths
    var airTitles: [String] = ["Air India"]
    var airImages: [String] = ["air.jpg"]
    var airURLs: [String] = ["https://www.airindia.in"]
    var railTitles: [String] = ["IRCTC"]
    var railImages: [String] = ["rail.jpg"]
    var railURLs: [String] = ["https://www.irctc.co.in/nget"]
    var roadTitles: [String] = ["Redbus"]
    var roadImages: [String] = ["bus.jpg"]
    var roadURLs: [String] = ["https://www.redbus.in"]
    
    // Whenever a cell is selected, the selected URL is changed
    var selectedURL:String = ""
    // It is saved in the user defaults
    var df1:UserDefaults!
    
    override func viewDidLoad() {
        super.viewDidLoad()

        title = "Second Screen"
        
        df1 = UserDefaults.standard
        
        tv1.delegate = self
        
        tv1.dataSource = self
    }
    
    // 3 sections in the table
    func numberOfSections(in tableView: UITableView) -> Int {
        return 3;
    }
    
    // Different Header for each section
    func tableView(_ tableView: UITableView, titleForHeaderInSection section:Int) -> String? {
        if section == 0 {
            return "Air"
        } else if section == 1 {
            return "Rail"
        } else {
            return "Road"
        }
    }
    
    // 1 cell per section
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return 1
    }
    
    // This function creates the cells
    // Set the title for each cell, and the image for it
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tv1.dequeueReusableCell(withIdentifier: "pc1", for: indexPath)
        cell.accessoryType = .detailDisclosureButton
        cell.backgroundColor = .yellow
        if indexPath.section == 0 {
            cell.textLabel?.text = airTitles[indexPath.row]
            cell.imageView?.image = UIImage(named: airImages[indexPath.row])
            return cell;
        } else if indexPath.section == 1 {
            cell.textLabel?.text = railTitles[indexPath.row]
            cell.imageView?.image = UIImage(named: railImages[indexPath.row])
            return cell;
        } else {
            cell.textLabel?.text = roadTitles[indexPath.row]
            cell.imageView?.image = UIImage(named: roadImages[indexPath.row])
            return cell;
        }
    }
    
    // This function is called whenever you select a cell
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        if indexPath.section == 0 {
            // Set the selected URL based on the section that the row was selected
            selectedURL = airURLs[indexPath.row]
        } else if indexPath.section == 1 {
            selectedURL = railURLs[indexPath.row]
        } else {
            selectedURL = roadURLs[indexPath.row]
        }
        // Set the url in the defaults, so that it can be accessed later
        df1.set(selectedURL, forKey: "url")
             
    }

}
```

## Third screen code 

```swift
import UIKit
import WebKit

class WVViewController: UIViewController {

    @IBOutlet weak var wv1: WKWebView!
    var df1:UserDefaults!
    var url:String!
    var urlRequest:URLRequest!
    override func viewDidLoad() {
        super.viewDidLoad()
        df1 = UserDefaults.standard
        
        // Load the URL saved in the defaults, which was done in the previous screen
        url = df1.string(forKey: "url")
        // Send a request to the URL and load it in the webview
        urlRequest = URLRequest(url: URL(string: url)!)
        wv1.load(urlRequest)

    }
   
}
```

