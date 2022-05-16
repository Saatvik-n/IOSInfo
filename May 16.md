# May 16 

## Map View with static coordinates 

This will show a map view with fixed coordinates (From the code). 
It can be changed between normal, satellite, hybrid modes using segmented control.

To run the program, import: `MapKit.framework, CoreLocation.framework`

```swift
import UIKit
import MapKit
import CoreLocation

class ViewController: UIViewController {

    @IBOutlet weak var sc1: UISegmentedControl!
    @IBOutlet weak var mv1: MKMapView!
    override func viewDidLoad() {
        super.viewDidLoad()

        // This represents a fixed locaiton using latitude, longtitude
        let staticLocation = CLLocationCoordinate2D(latitude: 12.9291, longitude: 77.6787)
        // This represents the size of the region - using deltas
        let span1 = MKCoordinateSpan(latitudeDelta: 0.003, longitudeDelta: 0.003)
        // This creates a region. The center of the region is the static locaiton, the size is given using span
        let region1 = MKCoordinateRegion(center: staticLocation, span: span1)
        mv1.setRegion(region1, animated: true)
        
        // This is an annotation over the coordinates given
        let annotation = MKPointAnnotation()
        annotation.coordinate = staticLocation
        annotation.title = "Bangalore"
        annotation.subtitle = "India"
        mv1.addAnnotation(annotation)
    }

    // Based on segmented control, change the map type of the view
    @IBAction func onSegClick() {
        let i = sc1.selectedSegmentIndex
        if i == 0 {
            mv1.mapType = .standard
        } else if i == 1 {
            mv1.mapType = .satellite
        } else {
            mv1.mapType = .hybrid
        }
    }
    
}
```

## Geocoding 

This will allow conversion of coordinates to address (reverse) , and address to coordinates (forward).

```swift
import UIKit
import MapKit
import CoreLocation

class ViewController: UIViewController {

    @IBOutlet weak var label2: UILabel!
    @IBOutlet weak var coordPrintBtn: UIButton!
    @IBOutlet weak var label1: UILabel!
    @IBOutlet weak var adressPrintBtn: UIButton!
    override func viewDidLoad() {
        super.viewDidLoad()
    }

    // Converts coordinates to location
    @IBAction func reverseGeoCoding() {
        
        let lat = 17.3850
        let lon = 78.4867
        if let loc = CLLocation(latitude: lat, longitude: lon) as? CLLocation {
            CLGeocoder().reverseGeocodeLocation(loc, completionHandler: { (placemarks, error) in
                if let placemark1 = placemarks?[0] {
                    let name = placemark1.name!
                    let country = placemark1.country!
                    let admin = placemark1.administrativeArea!
                    let locality = placemark1.locality!
                    let postalCode = placemark1.postalCode!
                    // Get the required parts from the address, and set the label text
                    self.label1.text = "\(name), \(admin), \(locality), \(country), \(postalCode)"
                    
                }
            })
        }
    }

    // Converts address to coordinates
    @IBAction func forwardGeoCoding() {
        let address = "Bhagya Reddi Road, TG, India, Hyderabad, 500095"
        CLGeocoder().geocodeAddressString(address, completionHandler: {placemarks, error in
            if (error != nil) {
                return
            }
            if let placemark1 = placemarks?[0] {
                let lat = String(format: "%.04f", (placemark1.location?.coordinate.latitude ?? 0.0)!)
                let long = String(format: "%.04f", (placemark1.location?.coordinate.longitude ?? 0.0)!)
                
                // Get lat, long from placemarks, and set the label text
                self.label2.text = "Latitude: \(lat) and Longitude = \(long)"
            }
        })
    }
}
```
