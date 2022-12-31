# text-classification-starter
<img width="300" alt="スクリーンショット 2022-12-31 9 09 53" src="https://user-images.githubusercontent.com/47273077/210119191-b2d4568d-d295-4b50-b648-f20937723863.gif">

```swift
   private func performTextClassification() {
       guard let img = image,
             let cgImage = img.cgImage,
             let orientation = self.sourceType == .camera ? CGImagePropertyOrientation.right : CGImagePropertyOrientation(rawValue: UInt32(img.imageOrientation.rawValue))
        else {
           return
       }
    
        let request = VNRecognizeTextRequest { (request, error) in
            if let observations = request.results as? [VNRecognizedTextObservation] {
                
                DispatchQueue.global().async {
                    if let result = img.drawOnImage(observations: observations) {
                        DispatchQueue.main.async {
                            self.image = result
                        }
                    }
                }
            }
        }
        
        let handler = VNImageRequestHandler(cgImage: cgImage, orientation: orientation, options: [:])
        
        do {
            try handler.perform([request])
        } catch {
            print(error.localizedDescription)
        }
    }
```
