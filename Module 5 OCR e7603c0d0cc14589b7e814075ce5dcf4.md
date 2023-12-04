# Module 5 / OCR

# **Introduction**

The ability for computer systems to process written or printed text is an area of artificial intelligence (AI) where *computer vision* intersects with *natural language processing*. You need computer vision capabilities to "read" the text, and then you need natural language processing capabilities to make sense of it.

The basic foundation of processing printed text is *optical character recognition* (OCR), in which a model can be trained to recognize individual shapes as letters, numerals, punctuation, or other elements of text. Much of the early work on implementing this kind of capability was performed by postal services to support automatic sorting of mail based on postal codes. Since then, the state-of-the-art for reading text has moved on, and it's now possible to build models that can detect printed or handwritten text in an image and read it line-by-line or even word-by-word.

At the other end of the scale, there is *machine reading comprehension* (MRC), in which an AI system not only reads the text characters, but can use a semantic model to interpret what the text is about.

In this module, we'll focus on the use of OCR technologies to detect text in images and convert it into a text-based data format, which can then be stored, printed, or used as the input for further processing or analysis.

## **Uses of OCR**

The ability to recognize printed and handwritten text in images, is beneficial in many scenarios such as:

- note taking
- digitizing forms, such as medical records or historical documents
- scanning printed or handwritten checks for bank deposits

# **Get started with OCR on Azure**

The ability to extract text from images is handled by the Computer Vision service, which also provides image analysis capabilities.

## **Azure resources for Computer Vision**

The first step towards using the Computer Vision service is to create a resource for it in your Azure subscription. You can use either of the following resource types:

- **Computer Vision**: A specific resource for the Computer Vision service. Use this resource type if you don't intend to use any other cognitive services, or if you want to track utilization and costs for your Computer Vision resource separately.
- **Cognitive Services**: A general cognitive services resource that includes Computer Vision along with many other cognitive services; such as Text Analytics, Translator Text, and others. Use this resource type if you plan to use multiple cognitive services and want to simplify administration and development.

Whichever type of resource you choose to create, it will provide two pieces of information that you will need to use it:

- A **key** that is used to authenticate client applications.
- An **endpoint** that provides the HTTP address at which your resource can be accessed.

**Note**

If you create a Cognitive Services resource, client applications use the same key and endpoint regardless of the specific service they are using.

## **Use the Computer Vision service to read text**

Many times an image contains text. It can be typewritten text or handwritten. Some common examples are images with road signs, scanned documents that are in an image format such as JPEG or PNG file formats, or even just a picture taken of a white board that was used during a meeting.

The Computer Vision service provides two application programming interfaces (APIs) that you can use to read text in images: the **OCR** API and the **Read** API.

### **The OCR API**

The OCR API is designed for quick extraction of small amounts of text in images. It operates synchronously to provide immediate results, and can recognize text in numerous languages.

When you use the OCR API to process an image, it returns a hierarchy of information that consists of:

- **Regions** in the image that contain text
- **Lines** of text in each region
- **Words** in each line of text

For each of these elements, the OCR API also returns *bounding box* coordinates that define a rectangle to indicate the location in the image where the region, line, or word appears.

### **The Read API**

The OCR method can have issues with false positives when the image is considered text-dominate. The Read API uses the latest recognition models and is optimized for images that have a significant amount of text or has considerable visual noise.

The Read API is a better option for scanned documents that have a lot of text. The Read API also has the ability to automatically determine the proper recognition model to use, taking into consideration lines of text and supporting images with printed text as well as recognizing handwriting.

Because the Read API can work with larger documents, it works asynchronously so as not to block your application while it is reading the content and returning results to your application. This means that to use the Read API, your application must use a three-step process:

1. Submit an image to the API, and retrieve an *operation ID* in response.
2. Use the operation ID to check on the status of the image analysis operation, and wait until it has completed.
3. Retrieve the results of the operation.

The results from the Read API are arranged into the following hierarchy:

- **Pages** - One for each page of text, including information about the page size and orientation.
- **Lines** - The lines of text on a page.
- **Words** - The words in a line of text.

Each line and word includes bounding box coordinates indicating its position on the page.

# **Exercise - Read text with the Computer Vision service**

A common computer vision challenge is to detect and interpret text in an image. This kind of processing is often referred to as *optical character recognition* (OCR).

To test the capabilities of the OCR service, we'll use a simple command-line application that runs in the Cloud Shell. The same principles and functionality apply in real-world solutions, such as web sites or phone apps.

## **Use the Computer Vision Service to Read Text in an Image**

The **Computer Vision** cognitive service provides support for OCR tasks, including:

- An **OCR** API that you can use to read text in multiple languages. This API can be used synchronously, and works well when you need to detect and read a small amount of text in an image.
- A **Read** API that is optimized for larger documents. This API is used asynchronously, and can be used for both printed and handwritten text.

## **Create a *Cognitive Services* resource**

You can use the Computer Vision service by creating either a **Computer Vision** resource or a **Cognitive Services** resource.

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

To test the capabilities of the Custom Vision service, we'll use a simple command-line application that runs in the Cloud Shell on Azure.

1. In the Azure portal, select the **[>_]** (*Cloud Shell*) button at the top of the page to the right of the search box. This opens a Cloud Shell pane at the bottom of the portal.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/read-text-computer-vision/media/powershell-portal-guide-1.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/read-text-computer-vision/media/powershell-portal-guide-1.png)
    
2. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). Select **PowerShell**. If you do not see this option, skip the step.
3. If you are prompted to create storage for your Cloud Shell, ensure your subscription is specified and select **Create storage**. Then wait a minute or so for the storage to be created.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/read-text-computer-vision/media/powershell-portal-guide-2.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/read-text-computer-vision/media/powershell-portal-guide-2.png)
    
4. Make sure the the type of shell indicated on the top left of the Cloud Shell pane is switched to *PowerShell*. If it is *Bash*, switch to *PowerShell* by using the drop-down menu.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/read-text-computer-vision/media/powershell-portal-guide-3.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/read-text-computer-vision/media/powershell-portal-guide-3.png)
    
5. Wait for PowerShell to start. You should see the following screen in the Azure portal:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/read-text-computer-vision/media/powershell-prompt.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/read-text-computer-vision/media/powershell-prompt.png)
    

## **Configure and run a client application**

Now that you have a custom model, you can run a simple client application that uses the OCR service.

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
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/read-text-computer-vision/media/powershell-portal-guide-4.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/read-text-computer-vision/media/powershell-portal-guide-4.png)
    
3. In the **Files** pane on the left, expand **ai-900** and select **ocr.ps1**. This file contains some code that uses the Computer Vision service to detect and analyze text in an image, as shown here:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/read-text-computer-vision/media/ocr-code.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/read-text-computer-vision/media/ocr-code.png)
    
4. Don't worry too much about the details of the code, the important thing is that it needs the endpoint URL and either of the keys for your Cognitive Services resource. Copy these from the **Keys and Endpoints** page for your resource from the Azure portal and paste them into the code editor, replacing the **YOUR_KEY** and **YOUR_ENDPOINT** placeholder values respectively.
    
    **Tip**
    
    You may need to use the separator bar to adjust the screen area as you work with the **Keys and Endpoint** and **Editor** panes.
    
    After pasting the key and endpoint values, the first two lines of code should look similar to this:
    
    PowerShellCopy
    
    ```
    $key="1a2b3c4d5e6f7g8h9i0j...."
    $endpoint="https..."
    
    ```
    
5. At the top right of the editor pane, use the **...** button to open the menu and select **Save** to save your changes. Then open the menu again and select **Close Editor**. Now that you've set up the key and endpoint, you can use your Cognitive Services resource to extract text from an image.
    
    Let's use the **OCR** API, which enables you to synchronously analyze an image and read any text it contains. In this case, you have an advertising image for the fictional Northwind Traders retail company that includes some text.
    
    The sample client application will analyze the following image:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/read-text-computer-vision/media/advert.jpg](https://docs.microsoft.com/en-us/learn/wwl-data-ai/read-text-computer-vision/media/advert.jpg)
    
6. In the PowerShell pane, enter the following commands to run the code to read the text:
    
    Copy
    
    ```
    cd ai-900
    ./ocr.ps1 advert.jpg
    
    ```
    
7. Review the details found in the image. The text found in the image is organized into a hierarchical structure of regions, lines, and words, and the code reads these to retrieve the results.
    
    Note that the location of text is indicated by the top- left coordinates, and the width and height of a *bounding box*, as shown here:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/read-text-computer-vision/media/lab-05-bounding-boxes.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/read-text-computer-vision/media/lab-05-bounding-boxes.png)
    
8. Now let's try another image:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/read-text-computer-vision/media/letter.jpg](https://docs.microsoft.com/en-us/learn/wwl-data-ai/read-text-computer-vision/media/letter.jpg)
    
    To analyze the second image, enter the following command:
    
    Copy
    
    ```
    ./ocr.ps1 letter.jpg
    
    ```
    
9. Review the results of the analysis for the second image. It should also return the text and bounding boxes of the text.

## **Learn more**

This simple app shows only some of the OCR capabilities of the Computer Vision service. To learn more about what you can do with this service, see the [OCR page](https://docs.microsoft.com/en-us/azure/cognitive-services/computer-vision/overview-ocr).