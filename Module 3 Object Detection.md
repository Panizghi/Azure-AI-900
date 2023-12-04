# Module 3  / Object Detection

# **Introduction**

*Object detection* is a form of machine learning based computer vision in which a model is trained to recognize individual types of objects in an image, and to identify their location in the image.

## **Uses of object detection**

Some sample applications of object detection include:

- **Checking for building safety**: Evaluating the safety of a building by analyzing footage of its interior for fire extinguishers or other emergency equipment.
- **Driving assistance**: Creating software for self-driving cars or vehicles with *lane assist* capabilities. The software can detect whether there is a car in another lane, and whether the driver's car is within its own lanes.
- **Detecting tumors**: Medical imaging such as an MRI or x-rays that can detect known objects for medical diagnosis.

## **Learning objectives**

Learn how to use the Custom Vision service to create an image detection solution. In this module you will:

- Identify services in Azure for creating an object detection solution.
- Provision a Custom Vision resource.
- Train an object detection model.
- Publish and consume an object detection model.

# **What is object detection?**

Let's look at example of object detection. Consider the following image:

![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/produce.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/produce.png)

An object detection model might be used to identify the individual objects in this image and return the following information:

![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/produce-objects.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/produce-objects.png)

Notice that an object detection model returns the following information:

- The *class* of each object identified in the image.
- The probability score of the object classification (which you can interpret as the *confidence* of the predicted class being correct)
- The coordinates of a *bounding box* for each object.

**Note**

**Object detection vs. image classification**

*Image classification* is a machine learning based form of computer vision in which a model is trained to categorize images based on the primary subject matter they contain. *Object detection* goes further than this to classify individual objects within the image, and to return the coordinates of a bounding box that indicates the object's location.

# **Get started with object detection on Azure**

You can create an object detection machine learning model by using advanced deep learning techniques. However, this approach requires significant expertise and a large volume of training data. The **Custom Vision** cognitive service in Azure enables you to create object detection models that meet the needs of many computer vision scenarios with minimal deep learning expertise and fewer training images.

## **Azure resources for Custom Vision**

Creating an object detection solution with Custom Vision consists of three main tasks. First you must use upload and tag images, then you can train the model, and finally you must publish the model so that client applications can use it to generate predictions.

For each of these tasks, you need a resource in your Azure subscription. You can use the following types of resource:

- **Custom Vision**: A dedicated resource for the custom vision service, which can be either a *training*, a *prediction* or a both resource.
- **Cognitive Services**: A general cognitive services resource that includes Custom Vision along with many other cognitive services. You can use this type of resource for *training*, *prediction*, or both.

The separation of training and prediction resources is useful when you want to track resource utilization for model training separately from client applications using the model to predict image classes. However, it can make development of an image classification solution a little confusing.

The simplest approach is to use a general Cognitive Services resource for both training and prediction. This means you only need to concern yourself with one *endpoint* (the HTTP address at which your service is hosted) and *key* (a secret value used by client applications to authenticate themselves).

If you choose to create a Custom Vision resource, you will be prompted to choose *training*, *prediction*, or *both* - and it's important to note that if you choose "both", then ***two*** resources are created - one for training and one for prediction.

It's also possible to take a mix-and-match approach in which you use a dedicated Custom Vision resource for training, but deploy your model to a Cognitive Services resource for prediction. For this to work, the training and prediction resources must be created in the same region.

## **Image tagging**

Before you can train an object detection model, you must tag the classes and bounding box coordinates in a set of training images. This process can be time-consuming, but the *Custom Vision portal* provides a graphical interface that makes it straightforward. The interface will automatically suggest areas of the image where discrete objects are detected, and you can apply a class label to these suggested bounding boxes or drag to adjust the bounding box area. Additionally, after tagging and training with an initial dataset, the Computer Vision service can use *smart tagging* to suggest classes and bounding boxes for images you add to the training dataset.

Key considerations when tagging training images for object detection are ensuring that you have sufficient images of the objects in question, preferably from multiple angles; and making sure that the bounding boxes are defined tightly around each object.

## **Model training and evaluation**

To train the model, you can use the *Custom Vision portal*, or if you have the necessary coding experience you can use one of the Custom Vision service programming language-specific software development kits (SDKs). Training an object detection model can take some time, depending on the number of training images, classes, and objects within each image.

Model training process is an iterative process in which the Custom Vision service repeatedly trains the model using some of the data, but holds some back to evaluate the model. At the end of the training process, the performance for the trained model is indicated by the following evaluation metrics:

- **Precision**: What percentage of class predictions did the model correctly identify? For example, if the model predicted that 10 images are oranges, of which eight were actually oranges, then the precision is 0.8 (80%).
- **Recall**: What percentage of the class predictions made by the model were correct? For example, if there are 10 images of apples, and the model found 7 of them, then the recall is 0.7 (70%).
- **Mean Average Precision (mAP)**: An overall metric that takes into account both precision and recall across all classes).

## **Using the model for prediction**

After you've trained the model, and you're satisfied with its evaluated performance, you can publish the model to your prediction resource. When you publish the model, you can assign it a name (the default is "Iteration*X*", where X is the number of times you have trained the model).

To use you model, client application developers need the following information:

- **Project ID**: The unique ID of the Custom Vision project you created to train the model.
- **Model name**: The name you assigned to the model during publishing.
- **Prediction endpoint**: The HTTP address of the endpoints for the *prediction* resource to which you published the model (***not*** the training resource).
- **Prediction key**: The authentication key for the *prediction* resource to which you published the model (***not*** the training resource).

# **Exercise - Create an object detection solution**

*Object detection* is a form of computer vision in which a machine learning model is trained to classify individual instances of objects in an image, and indicate a *bounding box* that marks its location. You can think of this as a progression from *image classification* (in which the model answers the question "what is this an image of?") to building solutions where we can ask the model "what objects are in this image, and where are they?".

For example, a grocery store might use an object detection model to implement an automated checkout system that scans a conveyor belt using a camera, and can identify specific items without the need to place each item on the belt and scan them individually.

The **Custom Vision** cognitive service in Microsoft Azure provides a cloud-based solution for creating and publishing custom object detection models. In Azure, you can use the Custom Vision service to train an image classification model based on existing images. There are two elements to creating an image classification solution. First, you must train a model to recognize different classes using existing images. Then, when the model is trained you must publish it as a service that can be consumed by applications.

To test the capabilities of the Custom Vision service to detect objects in images, we'll use a simple command-line application that runs in the Cloud Shell. The same principles and functionality apply in real-world solutions, such as web sites or phone apps.

## **Create a *Cognitive Services* resource**

You can use the Custom Vision service by creating either a **Custom Vision** resource or a **Cognitive Services** resource.

**Note**

Not every resource is available in every region. Whether you create a Custom Vision or Cognitive Services resource, only resources created in **[certain regions](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)** can be used to access Custom Vision services. For simplicity, a region is pre-selected for you in the configuration instructions below.

Create a **Cognitive Services** resource in your Azure subscription.

1. In another browser tab, open the Azure portal at [https://portal.azure.com](https://portal.azure.com/), signing in with your Microsoft account.
2. Click the **＋Create a resource** button, search for *Cognitive Services*, and create a **Cognitive Services** resource with the following settings:
    - **Subscription**: *Your Azure subscription*.
    - **Resource group**: *Select or create a resource group with a unique name*.
    - **Region**: East US
    - **Name**: *Enter a unique name*.
    - **Pricing tier**: S0
    - **I confirm I have read and understood the notices**: Selected.
3. Review and create the resource, and wait for deployment to complete. Then go to the deployed resource.
4. View the **Keys and Endpoint** page for your Cognitive Services resource. You will need the endpoint and keys to connect from client applications.

## **Create a Custom Vision project**

To train an object detection model, you need to create a Custom Vision project based on your training resource. To do this, you'll use the Custom Vision portal.

1. In a new browser tab, open the Custom Vision portal at [https://customvision.ai](https://customvision.ai/), and sign in using the Microsoft account associated with your Azure subscription.
2. Create a new project with the following settings:
    - **Name**: Grocery Detection
    - **Description**: Object detection for groceries.
    - **Resource**: *The resource you created previously*
    - **Project Types**: Object Detection
    - **Domains**: General
3. Wait for the project to be created and opened in the browser.

## **Add and tag images**

To train an object detection model, you need to upload images that contain the classes you want the model to identify, and tag them to indicate bounding boxes for each object instance.

1. Download and extract the training images from [https://aka.ms/fruit-objects](https://aka.ms/fruit-objects). The extracted folder contains a collection of images of fruit.
2. In the Custom Vision portal [https://customvision.ai](https://customvision.ai/), make sure you are working in your object detection project *Grocery Detection*. Then select **Add images** and upload all of the images in the extracted folder.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/fruit-upload.jpg](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/fruit-upload.jpg)
    
3. After the images have been uploaded, select the first one to open it.
4. Hold the mouse over any object in the image until an automatically detected region is displayed like the image below. Then select the object, and if necessary resize the region to surround it.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/object-region.jpg](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/object-region.jpg)
    
    Alternatively, you can simply drag around the object to create a region.
    
5. When the region surrounds the object, add a new tag with the appropriate object type (*apple*, *banana*, or *orange*) as shown here:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/object-tag.jpg](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/object-tag.jpg)
    
6. Select and tag each other object in the image, resizing the regions and adding new tags as required.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/object-tags.jpg](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/object-tags.jpg)
    
7. Use the **>** link on the right to go to the next image, and tag its objects. Then just keep working through the entire image collection, tagging each apple, banana, and orange.
8. When you have finished tagging the last image, close the **Image Detail** editor and on the **Training Images** page, under **Tags**, select **Tagged** to see all of your tagged images:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/tagged-images.jpg](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/tagged-images.jpg)
    

## **Train and test a model**

Now that you've tagged the images in your project, you're ready to train a model.

1. In the Custom Vision project, click **Train** to train an object detection model using the tagged images. Select the **Quick Training** option.
2. Wait for training to complete (it might take ten minutes or so), and then review the *Precision*, *Recall*, and *mAP* performance metrics - these measure the prediction accuracy of the object detection model, and should all be high.
3. At the top right of the page, click **Quick Test**, and then in the **Image URL** box, enter `https://aka.ms/apple-orange` and view the prediction that is generated. Then close the **Quick Test** window.

## **Publish the object detection model**

Now you're ready to publish your trained model and use it from a client application.

1. Click **🗸 Publish** to publish the trained model with the following settings:
    - **Model name**: detect-produce
    - **Prediction Resource**: *The resource you created previously*.
2. After publishing, click the *Prediction URL* (🌐) icon to see information required to use the published model. Later, you will need the appropriate URL and Prediction-Key values to get a prediction from an Image URL, so keep this dialog box open and carry on to the next task.

## **Run Cloud Shell**

To test the capabilities of the Custom Vision service, we'll use a simple command-line application that runs in the Cloud Shell on Azure.

1. In the Azure portal, select the **[>_]** (*Cloud Shell*) button at the top of the page to the right of the search box. This opens a Cloud Shell pane at the bottom of the portal.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/powershell-portal-guide-1.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/powershell-portal-guide-1.png)
    
2. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). Select **PowerShell**. If you do not see this option, skip the step.
3. If you are prompted to create storage for your Cloud Shell, ensure your subscription is specified and select **Create storage**. Then wait a minute or so for the storage to be created.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/powershell-portal-guide-2.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/powershell-portal-guide-2.png)
    
4. Make sure the type of shell indicated on the top left of the Cloud Shell pane is switched to *PowerShell*. If it is *Bash*, switch to *PowerShell* by using the drop-down menu.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/powershell-portal-guide-3.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/powershell-portal-guide-3.png)
    
5. Wait for PowerShell to start. You should see the following screen in the Azure portal:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/powershell-prompt.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/powershell-prompt.png)
    

## **Configure and run a client application**

Now that you have a custom model, you can run a simple client application that uses the Custom Vision service to detect objects in an image.

1. In the command shell, enter the following command to download the sample application and save it to a folder called ai-900.
    
    Copy
    
    ```
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    
    ```
    
    **Note**
    
    If you already used this command in another lab to clone the *ai-900* repository, you can skip this step.
    
2. The files are downloaded to a folder named **ai-900**. Now we want to see all of the files in your Cloud Shell storage and work with them. Type the following command into the shell:
    
    Copy
    
    ```
    code .
    
    ```
    
    Notice how this opens up an editor like the one in the image below:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/powershell-portal-guide-4.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/powershell-portal-guide-4.png)
    
3. In the **Files** pane on the left, expand **ai-900** and select **detect-objects.ps1**. This file contains some code that uses the Custom Vision service to detect objects an image, as shown here:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/detect-image-code.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/detect-image-code.png)
    
4. Don't worry too much about the details of the code, the important thing is that it needs the prediction URL and key for your Custom Vision model when using an image URL.
    
    Get the *prediction URL* from the dialog box in your Custom Vision project.
    
    **Note**
    
    Remember, you reviewed the *prediction URL* after you published the image classification model. To find the *prediction URL*, navigate to the **Performance** tab in your project, then click on **Prediction URL** (if the screen is compressed, you may just see a globe icon). A dialogue box will appear. Copy the url for **If you have an image URL**. Paste it into the code editor, replacing **YOUR_PREDICTION_URL**.
    
    Using the same dialog box, get the *prediction key*. Copy the prediction key displayed after *Set Prediction-Key Header to*. Paste it in the code editor, replacing the **YOUR_PREDICTION_KEY** placeholder value.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/find-prediction-url.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/find-prediction-url.png)
    
    After pasting the Prediction URL and Prediction Key values, the first two lines of code should look similar to this:
    
    PowerShellCopy
    
    ```
    $predictionUrl="https..."
    $predictionKey ="1a2b3c4d5e6f7g8h9i0j...."
    
    ```
    
5. At the top right of the editor pane, use the **...** button to open the menu and select **Save** to save your changes. Then open the menu again and select **Close Editor**.
    
    You will use the sample client application to detect objects in this image:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/produce.jpg](https://docs.microsoft.com/en-us/learn/wwl-data-ai/detect-objects-images-custom-vision/media/produce.jpg)
    
6. In the PowerShell pane, enter the following command to run the code:
    
    Copy
    
    ```
    cd ai-900
    ./detect-objects.ps1
    
    ```
    
7. Review the prediction, which should be *apple orange banana*.

## **Learn more**

This simple app shows only some of the capabilities of the Custom Vision service. To learn more about what you can do with this service, see the [Custom Vision page](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/).