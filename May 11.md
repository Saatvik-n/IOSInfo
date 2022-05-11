# May 11 

## Opening Photo Gallery / Camera app

There are 2 buttons and imageView. On clicking a button, the app will either an image picker or camera app to take a photo. 
After selecting the photo, it will be displayed in the imageView.
(Camera doesn't work on simulator). The component used is `UIImagePickerController`.
It can select images from both camera, and photo gallery.

```swift
import UIKit

class ViewController: UIViewController, UIImagePickerControllerDelegate, UINavigationControllerDelegate {

    @IBOutlet weak var camerabtn: UIButton!
    @IBOutlet weak var photobtn: UIButton!
    @IBOutlet weak var iv1: UIImageView!
    
    // There are 2 image pickers - one for gallery, one for photo
    var PhotoLibraryImagePicker1: UIImagePickerController!
    var CameraImagePicker1: UIImagePickerController!
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }

    // Call this function on clicking the photo gallery button
    @IBAction func photoBtnClick() {
        PhotoLibraryImagePicker1 = UIImagePickerController()
        PhotoLibraryImagePicker1.delegate = self
        // The source for this photo picker is the user's photo gallery
        PhotoLibraryImagePicker1.sourceType = .photoLibrary
        // We can edit the picture before sending it to the app
        PhotoLibraryImagePicker1.allowsEditing = true
        
        // This brings up the picker dialog
        self.present(PhotoLibraryImagePicker1, animated: true, completion: nil)
        
    }
    
    // This is called whenever the user is done picking the image
    internal func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
        // info is a dictionary containing 2 images - originalImage and editedImage
        // Since we are allowing user to edit the image, editedImage is used
        // This image is set as the image for the imageView
        let image1 = info[.editedImage] as! UIImage
        iv1.image = image1
        self.dismiss(animated: true, completion: nil)
  
    }
    
    // This is called whenever the user presses cancel on the image picker
    internal func imagePickerControllerDidCancel(_ picker: UIImagePickerController) {
        self.dismiss(animated: true, completion: nil)
    }
    
    // Call this function on clicking the camera button
    @IBAction func cameraBtnClick() {
        // Only execute if a camera device is available
        if (UIImagePickerController.isSourceTypeAvailable(UIImagePickerController.SourceType.camera)) {
            CameraImagePicker1 = UIImagePickerController()
            CameraImagePicker1.delegate = self
            // Set the source of this picker as the camera
            CameraImagePicker1.sourceType = .camera
            // Set the camera ot rear camera
            CameraImagePicker1.cameraDevice = .rear
            CameraImagePicker1.allowsEditing = true
            self.present(CameraImagePicker1, animated: true, completion: nil)

        }
    }
}
```

## Show action sheet on clicking a button, to open photo gallery, or camera 

On clicking "select image" button, action sheet will be presented with 3 options:
* Photo gallery
* Camera 
* Cancel 

User can click one of them to select an image, and this iamge should be displayed on the imageView.
Class used for action sheet is `UIAlertController`

```swift
import UIKit

class ViewController: UIViewController, UIImagePickerControllerDelegate, UINavigationControllerDelegate
{
    
    @IBOutlet weak var iv1: UIImageView!
    @IBOutlet weak var addImageBtn: UIButton!
    var ac1: UIAlertController!
    var PhotoLibraryImagePicker1: UIImagePickerController!
    var CameraImagePicker1: UIImagePickerController!

    override func viewDidLoad() {
        super.viewDidLoad()
        // Initialize Alert Controller
        ac1 = UIAlertController(title: "Select Option", message: "Options", preferredStyle: .actionSheet)
        
        // Cancel button - dismisses on clicking it
        let cancelActionButton = UIAlertAction(title: "Cancel", style: .cancel) { action -> Void in
            self.ac1.dismiss(animated: true, completion: nil)
        }
        
        // Photo Gallery Button - opens photo gallery
        let photoAction = UIAlertAction(title: "Photo Gallery", style: .default) { action -> Void in
            self.executePhoto()
        }

        // Camera Button - opens camera
        let cameraAction = UIAlertAction(title: "Camera", style: .default) {  action -> Void in
            self.executeCamera()
        }
        
        // Add all these actions to the action controller
        ac1.addAction(photoAction)
        ac1.addAction(cameraAction)
        ac1.addAction(cancelActionButton)
    }

    // On clicking the "select image" button, show the action controller
    @IBAction func onButtonClick() {
        self.present(ac1, animated: true, completion: nil)
    }

    // Explained above
    internal func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
        let image1 = info[.editedImage] as! UIImage
        self.iv1.image = image1
        self.dismiss(animated: true, completion: nil)
    }
    
    // Called when user selects the Photo Gallery option in action sheet
    func executePhoto() {
        self.PhotoLibraryImagePicker1 = UIImagePickerController()
        self.PhotoLibraryImagePicker1.delegate = self
        self.PhotoLibraryImagePicker1.sourceType = .photoLibrary
        self.PhotoLibraryImagePicker1.allowsEditing = true
        // This dismisses the action sheet
        ac1.dismiss(animated: true, completion: nil)
        // This presents the image picker
        self.present(PhotoLibraryImagePicker1, animated: true, completion: nil)
    }
    
    // Called when user selects the Camera option in action sheet
    func executeCamera() {
        if (UIImagePickerController.isSourceTypeAvailable(UIImagePickerController.SourceType.camera)) {
            CameraImagePicker1 = UIImagePickerController()
            CameraImagePicker1.delegate = self
            CameraImagePicker1.sourceType = .camera
            CameraImagePicker1.cameraDevice = .rear
            CameraImagePicker1.allowsEditing = true
            self.present(CameraImagePicker1, animated: true, completion: nil)
        }
    }
    
}
```

## Activity view 

Activity View is a pop up which brings up options like share to facebook, upload, etc. 

```swift
import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var sharebtn: UIButton!
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
    }

    @IBAction func onButtonClick() {
        let text = "Sharing Details"
        let image = UIImage(named: "image.jpg")
        let myWebsite = URL(string: "https://www.apple.com")
        
        let shareAll = [text, image!, myWebsite!] as [Any]
        let activityViewController = UIActivityViewController(activityItems: shareAll, applicationActivities: nil)
        activityViewController.popoverPresentationController?.sourceView = self.view
        self.present(activityViewController, animated: true, completion: nil)
    }
    
}
```


