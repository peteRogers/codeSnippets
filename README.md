# codeSnippets
## Show Video
This code creates a videoPlayer and loads a video and displays it on a UIViewController

```swift
  func initializeVideoPlayerWithVideo() {

         // get the path string for the video from assets
         let videoString:String? = Bundle.main.path(forResource: "sampleVid", ofType: "mp4")
         guard let unwrappedVideoPath = videoString else {return}

         // convert the path string to a url
         let videoUrl = URL(fileURLWithPath: unwrappedVideoPath)

         // initialize the video player with the url
         let player = AVPlayer(url: videoUrl)

         // create a video layer for the player
         let layer: AVPlayerLayer = AVPlayerLayer(player: player)

         // make the layer the same size as the container view
        layer.frame = self.view.bounds

         // make the video fill the layer as much as possible while keeping its aspect size
        layer.videoGravity = AVLayerVideoGravity.resizeAspect

         // add the layer to the container view
        self.view.layer.addSublayer(layer)
        player.play()
     }
 ```
 
 ## Storyboard navigation: Unwind Segue
 Add this code to your starting view controller to unwind segue or in English go back home
 ```swift
 @IBAction func unwind( _ seg: UIStoryboardSegue) {
}
 ```
 ## Load a web page and stop navigation to other links
 ```swift
 import UIKit
import WebKit

class WebViewController: UIViewController, WKNavigationDelegate {
    let urlString = "https://www.bbc.co.uk"
    @IBOutlet weak var webView: WKWebView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        let url = URL(string: urlString)!
        webView.navigationDelegate = self
        webView.load(URLRequest(url: url))
    }

    func webView(_ webView: WKWebView, decidePolicyFor navigationAction: WKNavigationAction, decisionHandler: @escaping (WKNavigationActionPolicy) -> Void) {
        if navigationAction.request.url?.absoluteString == urlString{
            decisionHandler(.allow)
        }else{
            decisionHandler(.cancel)
        }
    }
}
```

# Load a camera
```swift
import UIKit
import AVFoundation


class ViewController: UIViewController{
    
    private let captureSession = AVCaptureSession()
    private lazy var previewLayer = AVCaptureVideoPreviewLayer(session: self.captureSession)


    override func viewDidLoad() {
        super.viewDidLoad()
        self.addCameraInput()
        self.showCameraFeed()
        self.captureSession.startRunning()
    }
    
    override func viewDidLayoutSubviews() {
        super.viewDidLayoutSubviews()
        self.previewLayer.frame = self.view.frame
    }

    
    private func addCameraInput() {
        guard let device = AVCaptureDevice.DiscoverySession(
            deviceTypes: [.builtInWideAngleCamera, .builtInDualCamera, .builtInTrueDepthCamera],
            mediaType: .video,
            position: .back).devices.first else {
                fatalError("No back camera device found, please make sure to run SimpleLaneDetection in an iOS device and not a simulator")
        }
        let cameraInput = try! AVCaptureDeviceInput(device: device)
        self.captureSession.addInput(cameraInput)
    }
    
    private func showCameraFeed() {
        self.previewLayer.videoGravity = .resizeAspectFill
        self.view.layer.addSublayer(self.previewLayer)
        self.previewLayer.frame = self.view.frame
    }
  
}
```
