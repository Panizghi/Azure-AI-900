# Module 4 / Face Detection

# i**ntroduction**

Face detection and analysis is an area of artificial intelligence (AI) in which we use algorithms to locate and analyze human faces in images or video content.

## **Face detection**

Face detection involves identifying regions of an image that contain a human face, typically by returning *bounding box* coordinates that form a rectangle around the face, like this:

![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/face-detection.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/face-detection.png)

## **Facial analysis**

Moving beyond simple face detection, some algorithms can also return other information, such as facial landmarks (nose, eyes, eyebrows, lips, and others).

![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/landmarks-1.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/landmarks-1.png)

These facial landmarks can be used as features with which to train a machine learning model from which you can infer information about a person, such as their perceived age or perceived emotional state, like this:

![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/face-attributes.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/face-attributes.png)

## **Facial recognition**

A further application of facial analysis is to train a machine learning model to identify known individuals from their facial features. This usage is more generally known as *facial recognition*, and involves using multiple images of each person you want to recognize to train a model so that it can detect those individuals in new images on which it wasn't trained.

![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/facial-recognition.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/facial-recognition.png)

## **Uses of face detection and analysis**

There are many applications for face detection, analysis, and recognition. For example,

- Security - facial recognition can be used in building security applications, and increasingly it is used in smart phones operating systems for unlocking devices.
- Social media - facial recognition can be used to automatically tag known friends in photographs.
- Intelligent monitoring - for example, an automobile might include a system that monitors the driver's face to determine if the driver is looking at the road, looking at a mobile device, or shows signs of tiredness.
- Advertising - analyzing faces in an image can help direct advertisements to an appropriate demographic audience.
- Missing persons - using public cameras systems, facial recognition can be used to identify if a missing person is in the image frame.
- Identity validation - useful at ports of entry kiosks where a person holds a special entry permit.

# **Get started with Face analysis on Azure**

Microsoft Azure provides multiple cognitive services that you can use to detect and analyze faces, including:

- **Computer Vision**, which offers face detection and some basic face analysis, such as determining age.
- **Video Indexer**, which you can use to detect and identify faces in a video.
- **Face**, which offers pre-built algorithms that can detect, recognize, and analyze faces.

Of these, Face offers the widest range of facial analysis capabilities, so we'll focus on that service in this module.

## **Face**

Face currently supports the following functionality:

- Face Detection
- Face Verification
- Find Similar Faces
- Group faces based on similarities
- Identify people

Face can return the rectangle coordinates for any human faces that are found in an image, as well as a series of attributes related to those faces such as:

- **Age**: a guess at an age
- **Blur**: how blurred the face is (which can be an indication of how likely the face is to be the main focus of the image)
- **Emotion**: what emotion is displayed
- **Exposure**: aspects such as underexposed or over exposed and applies to the face in the image and not the overall image exposure
- **Facial hair**: the estimated facial hair presence
- **Glasses**: if the person is wearing glasses
- **Hair**: the hair type and hair color
- **Head pose**: the face's orientation in a 3D space
- **Makeup**: whether the face in the image has makeup applied
- **Noise**: refers to visual noise in the image. If you have taken a photo with a high ISO setting for darker settings, you would notice this noise in the image. The image looks grainy or full of tiny dots that make the image less clear
- **Occlusion**: determines if there may be objects blocking the face in the image
- **Smile**: whether the person in the image is smiling

## **Azure resources for Face**

To use Face, you must create one of the following types of resource in your Azure subscription:

- **Face**: Use this specific resource type if you don't intend to use any other cognitive services, or if you want to track utilization and costs for Face separately.
- **Cognitive Services**: A general cognitive services resource that includes Computer Vision along with many other cognitive services; such as Computer Vision, Text Analytics, Translator Text, and others. Use this resource type if you plan to use multiple cognitive services and want to simplify administration and development.

Whichever type of resource you choose to create, it will provide two pieces of information that you will need to use it:

- A **key** that is used to authenticate client applications.
- An **endpoint** that provides the HTTP address at which your resource can be accessed.

**Note**

If you create a Cognitive Services resource, client applications use the same key and endpoint regardless of the specific service they are using.

## **Tips for more accurate results**

There are some considerations that can help improve the accuracy of the detection in the images:

- image format - supported images are JPEG, PNG, GIF, and BMP
- file size - 6 MB or smaller
- face size range - from 36 x 36 up to 4096 x 4096. Smaller or larger faces will not be detected
- other issues - face detection can be impaired by extreme face angles, occlusion (objects blocking the face such as sunglasses or a hand). Best results are obtained when the faces are full-frontal or as near as possible to full-frontal

# **Exercise - Detect and analyze faces with the Face service**

Computer vision solutions often require an artificial intelligence (AI) solution to be able to detect, analyze, or identify human faces. For example, suppose the retail company Northwind Traders has decided to implement a "smart store", in which AI services monitor the store to identify customers requiring assistance, and direct employees to help them. One way to accomplish this is to perform facial detection and analysis - in other words, determine if there are any faces in the images, and if so analyze their features.

To test the capabilities of the Face service, we'll use a simple command-line application that runs in the Cloud Shell. The same principles and functionality apply in real-world solutions, such as web sites or phone apps.

## **Create a *Cognitive Services* resource**

You can use the Face service by creating either a **Face** resource or a **Cognitive Services** resource.

If you haven't already done so, create a **Cognitive Services** resource in your Azure subscription.

1. In another browser tab, open the Azure portal at [https://portal.azure.com](https://portal.azure.com/), signing in with your Microsoft account.
2. Click the **＋Create a resource** button, search for *Cognitive Services*, and create a **Cognitive Services** resource with the following settings:
    - **Subscription**: *Your Azure subscription*.
    - **Resource group**: *Select or create a resource group with a unique name*.
    - **Region**: *Choose any available region*:
    - **Name**: *Enter a unique name*.
    - **Pricing tier**: S0
    - **I confirm I have read and understood the notices**: Selected.
3. Review and create the resource, and wait for deployment to complete. Then go to the deployed resource.
4. View the **Keys and Endpoint** page for your Cognitive Services resource. You will need the endpoint and keys to connect from client applications.

## **Run Cloud Shell**

To test the capabilities of the Face service, we'll use a simple command-line application that runs in the Cloud Shell on Azure.

1. In the Azure portal, select the **[>_]** (*Cloud Shell*) button at the top of the page to the right of the search box. This opens a Cloud Shell pane at the bottom of the portal.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/powershell-portal-guide-1.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/powershell-portal-guide-1.png)
    
2. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). Select **PowerShell**. If you do not see this option, skip the step.
3. If you are prompted to create storage for your Cloud Shell, ensure your subscription is specified and select **Create storage**. Then wait a minute or so for the storage to be created.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/powershell-portal-guide-2.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/powershell-portal-guide-2.png)
    
4. Make sure the the type of shell indicated on the top left of the Cloud Shell pane is switched to *PowerShell*. If it is *Bash*, switch to *PowerShell* by using the drop-down menu.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/powershell-portal-guide-3.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/powershell-portal-guide-3.png)
    
5. Wait for PowerShell to start. You should see the following screen in the Azure portal:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/powershell-prompt.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/powershell-prompt.png)
    

## **Configure and run a client application**

Now that you have a custom model, you can run a simple client application that uses the Face service.

1. In the command shell, enter the following command to download the sample application and save it to a folder called ai-900.
    
    Copy
    
    ```
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    
    ```
    
2. The files are downloaded to a folder named **ai-900**. Now we want to see all of the files in your Cloud Shell storage and work with them. Type the following command into the shell:
    
    Copy
    
    ```
    code .
    
    ```
    
    Notice how this opens up an editor like the one in the image below:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/powershell-portal-guide-4.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/powershell-portal-guide-4.png)
    
3. In the **Files** pane on the left, expand **ai-900** and select **find-faces.ps1**. This file contains some code that uses the Face service to detect and analyze faces in an image, as shown here:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/find-faces-code.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/find-faces-code.png)
    
4. Don't worry too much about the details of the code, the important thing is that it needs the endpoint URL and either of the keys for your Cognitive Services resource. Copy these from the **Keys and Endpoints** page for your resource from the Azure portal and paste them into the code editor, replacing the **YOUR_KEY** and **YOUR_ENDPOINT** placeholder values respectively.
    
    **Tip**
    
    You may need to use the separator bar to adjust the screen area as you work with the **Keys and Endpoint** and **Editor** panes.
    
    After pasting the key and endpoint values, the first two lines of code should look similar to this:
    
    PowerShellCopy
    
    ```
    $key="1a2b3c4d5e6f7g8h9i0j...."
    $endpoint="https..."
    
    ```
    
5. At the top right of the editor pane, use the **...** button to open the menu and select **Save** to save your changes. Then open the menu again and select **Close Editor**.
    
    The sample client application will use your Face service to analyze the following image, taken by a camera in the Northwind Traders store:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/store-camera-1.jpg](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/store-camera-1.jpg)
    
6. In the PowerShell pane, enter the following commands to run the code:
    
    Copy
    
    ```
    cd ai-900
    ./find-faces.ps1 store-camera-1.jpg
    
    ```
    
7. Review the details of the faces found in the image, which include:
    - The location of the face in the image
    - The approximate age of the person
    - An indication of the emotional state of the person (based on proportional scores for a range of emotions)
    
    Note that the location of a face is indicated by the top- left coordinates, and the width and height of a *bounding box*, as shown here:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/store-camera-1-face.jpg](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/store-camera-1-face.jpg)
    
8. Now let's try another image:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/store-camera-2.jpg](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/store-camera-2.jpg)
    
    To analyze the second image, enter the following command:
    
    Copy
    
    ```
    ./find-faces.ps1 store-camera-2.jpg
    
    ```
    
9. Review the results of the face analysis for the second image.
10. Let's try one more:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/store-camera-3.jpg](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-analyze-faces/media/store-camera-3.jpg)
    
    To analyze the third image, enter the following command:
    
    Copy
    
    ```
    ./find-faces.ps1 store-camera-3.jpg
    
    ```
    
11. Review the results of the face analysis for the third image.

## **Learn more**

This simple app shows only some of the capabilities of the Face service. To learn more about what you can do with this service, see the [Face page](https://azure.microsoft.com/services/cognitive-services/face/).