# iOS SDK Integration On Atlas Demo App
![version](https://img.shields.io/badge/version-v1.0.0-blue)

There are SDK integration module on Atlas demo App,inside that module there are three submodules(Go For Scan Documents,Go For Capture ID Images,Go For Face Liveness),for each submodule need to integrate framework.

#  Submodule

- [Go For Scan Document](#go-for-scan-document)
- [Go For Capture ID Images](#go-for-capture-id-images)
- [Go For Face Liveness](#go-for-face-liveness)

## Go For Scan Document
   This submodule is related to scan different types of id's(like PAN,AADHAAR,PASSPORT etc).
   To achieve this need to integrate Octus SDK using cocoapods,follow the instruction using link https://github.com/frslabs/octus-ios#installation to install Octus                                            SDK in iOS APP.
   
 On ViewController class import Octus and import IdScannerControllerDelegate.Inside IdScannerControllerDelegate methods will get Octus result.
 
 On Next button action on Scan Document page call invokeOctusSdk() function with parameters.
  
    import UIKit
    import Octus
    
    class ViewController: UIViewController,IdScannerControllerDelegate
    {
    
    func idScannerController(_ scanner: IdScannerController, didFinishScanningWithResults results: IdScannerResults) 
    {
       // Octus Result
       print("OctusResult: ", results.octusResult)
     }
    func idScannerControllerDidCancel(_ scanner: IdScannerController) 
    {
        print("Scan operation cancelled") 
    }
    func idScannerController(_ scanner: IdScannerController, didFailWithError error: Int) 
    {
        print("ErrorCode: ", error)
    }

    @IBAction func invokeOctusSdk(_ sender: UIButton)
   
    { 
    // CALL SDK
   
       invokeOctusSdk(idName:”Enter ID Name”, documentSide:1 or 2,documentSubType: "Enter Subtypes”,documentCountry:”Enter Country”) 

    // For Example if need to Scan PAN Card use InvokeOctusSdk function like this
   
        invokeOctusSdk(idName:Document.PAN.rawValue,documentSide:1,documentSubType:ScanMode.OCR.rawValue,documentCountry:Country.in.rawValue) 

    }

    private func invokeOctusSdk(idName:String,documentSide:Int,documentSubType:String?,documentCountry:String?)

    {
        let scanner = IdScannerController(delegate: self)
        scanner.modalPresentationStyle = .fullScreen
        scanner.licenceKey = “ENTER LICENCE KEY”
        scanner.isManualScanEnabled = false
        scanner.isOrientationFlat = false
        scanner.documentType = idName
        scanner.documentCountry = documentCountry ?? ""
        scanner.documentSubType = documentSubType ?? ""
        scanner.documentSide = documentSide  // 1 or 2 based on ID sides
        present(scanner, animated: false)
     }
    }
  
  ## Go For Capture ID Images
  
  This submodule is related to capture id images vertically and horizontally.
  To achieve this need to integrate Captus SDK using cocoapods,follow the instructionn using link https://github.com/frslabs/captus-ios#installation to install captus SDK in iOS App.
  
  On ViewController class import Captus and import OCRDelegate.Inside OCRDelegate methods will get Captus result.
  
  On Next button action on Capture ID page call invokeOctusSdk() function with parameters.
  
    import UIKit
    import Captus
    
    class ViewController: UIViewController,OCRDelegate
    {
    func captusSuccessResponse(ocr: OCRNavigationViewController, didFinishOcrWithResult results: CaptusResults) {
        let imagePath = results.imagePath
    }
    func captusFailureResponse(ocr: OCRNavigationViewController, didFailWithError error: String) {
        // let error =  error
    }
    @IBAction func invokeCaptusSdk(_ sender: UIButton)
   
    { 
    // CALL SDK
   
       invokeCaptusSdk(idType:”Enter ID Name”) 

    // For Example if need to capture PAN Card images use invokeCaptusSdk function like this
   
       invokeCaptusSdk(idType:CaptusDocument.PAN.rawValue) 

    }
 
     private func invokeCaptusSdk(idType:String,documentSide:Int)
     {
        let ocrVC = OCRNavigationViewController(delegate: self)
        ocrVC.modalPresentationStyle = .fullScreen
        ocrVC.licenceKey = “ENTER LICENCE KEY”
        ocrVC.idSides = documentSide    // 1 or 2 based on ID sides
        ocrVC.documentType = idType 
        ocrVC.isAutomatic = true    
        if ocrVC.isOCR == true{       // if invoking SDK for OCR
            ocrVC.serverHeader = serverHeader
        }else{                       
            ocrVC.serverHeader = ""
        }
        present(ocrVC, animated: true)
   
     }
    }
    
   ## Go For Face Liveness
   
   This submodule is related to record live face.To achieve this need to integrate Vidus SDK using cocoapods,follow the instruction using link 
   https://github.com/frslabs/vidus-ios#installation to install Vidus SDK in iOS APP.
   
   On ViewController class import Vidus and import RecordingDelegate.Inside RecordingDelegate methods will get Vidus result.
  
   addNodeWithParam with parameters function need to call on viewDidLoad() function.
   
   On Next button action on Record Video page call invokeVidusSdk() function.
   
    import UIKit
    import Vidus
    
     class ViewController: UIViewController,RecordingDelegate
     {
      // Vidus Node Variable
      var inputParam = [[String : String]]()
      var inputParameters = [String : String]()
      var nodeNameArray = [String]()
     func screenRecording(recording: ScreenNavigationViewController, didFinishRecordingWithResult results: VidusResults) {
        // Success Response
        let videoUrl = results.vidusVideoUrl
    }
    func screenRecording(recording: ScreenNavigationViewController, didFailWithError error: String) {
       // let errorCode = error
    }
    override func viewDidLoad() 
    {
    addNodeWithParam(nodeName: "Node Name", timeDuration: "Enter Time Duration", baseUrl: "Enter Base URL",keyId: "Enter KEY ID", keySecret: "Enter KEY SECRET",  
    messageText: "ENTER MESSAGE", voiceType: nil, questionID: "ENTER QUESTION ID")
    
    // For Example if need to invoke Simple Recorder Node use addNodeWithParam function like this
   
    addNodeWithParam(nodeName: VidusInput.VidusInputNode.simpleRecorderNode.rawValue, timeDuration: "10", baseUrl: nil,keyId: nil, keySecret: nil, messageText: 
    nil, voiceType: nil, questionID: nil)
    }
  
    private func invokeVidusSdk(){
        if inputParam.count > 0{
            let recorder = ScreenNavigationViewController(delegate: self)
            recorder.modalPresentationStyle = .fullScreen
            recorder.recordingNodeName = nodeNameArray
            recorder.nodeData = inputParam
            recorder.recordingType = VidusInput.RecordingType.screen.rawValue
            recorder.licenceKey = "ENTER LICENCE KEY"
            recorder.isEnableInstructionScreen = false
            recorder.isEnablePreviewScreen = false
            present(recorder, animated: true)
            inputParam.removeAll()
            nodeNameArray.removeAll()
        }
    }
   
    func
    addNodeWithParam(nodeName:String,timeDuration:String?,baseUrl:String?,keyId:String?,keySecret:String?,messageText:String?,voiceType:String?,questionID:String?)      
    
    {
        switch nodeName {
        case VidusInput.VidusInputNode.simpleRecorderNode.rawValue:
            inputParameters[VidusInput.SDKInputParameter.nodeName.rawValue] = nodeName
            inputParameters[VidusInput.SDKInputParameter.timeDuration.rawValue] = timeDuration
            nodeNameArray.append(nodeName)
            inputParam.append(inputParameters)
            break
        case VidusInput.VidusInputNode.challengeCodeNode.rawValue:
            inputParameters[VidusInput.SDKInputParameter.nodeName.rawValue] = nodeName
            inputParameters[VidusInput.SDKInputParameter.challengeCodeText.rawValue] = messageText
            inputParameters[VidusInput.SDKInputParameter.baseUrl.rawValue] = baseUrl
            inputParameters[VidusInput.SDKInputParameter.keyId.rawValue] = keyId
            inputParameters[VidusInput.SDKInputParameter.keySecret.rawValue] = keySecret
            nodeNameArray.append(nodeName)
            inputParam.append(inputParameters)
            break
        default:
            print("Error: Node is empty")
        }
      }
    }
   
   
  
  






