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
