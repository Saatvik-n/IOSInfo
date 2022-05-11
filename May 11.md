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