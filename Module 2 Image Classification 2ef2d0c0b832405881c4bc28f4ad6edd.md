# Module 2 / Image Classification

# **Introduction**

Image classification is a common workload in artificial intelligence (AI) applications. It harnesses the predictive power of machine learning to enable AI systems to identify real-world items based on images.

## **Uses of image classification**

Some potential uses for image classification include:

- **Product identification**: performing visual searches for specific products in online searches or even, in-store using a mobile device.
- **Disaster investigation**: identifying key infrastructure for major disaster preparation efforts. For example, identifying bridges and roads in aerial images can help disaster relief teams plan ahead in regions that are not well mapped.
- **Medical diagnosis**: evaluating images from X-ray or MRI devices could quickly classify specific issues found as cancerous tumors, or many other medical conditions related to medical imaging diagnosis.

## **Learning objectives**

Learn how to use the Custom Vision service to create an image classification solution. In this module you will:

- Identify image classification scenarios and technologies
- Provision a Custom Vision resource and use the Custom Vision portal
- Train an image classification model
- Publish and consume an image classification model

# **Understand classification**

You can use a machine learning *classification* technique to predict which category, or *class*, something belongs to. Classification machine learning models use a set of inputs, which we call *features*, to calculate a probability score for each possible class and predict a *label* that indicates the most likely class that an object belongs to.

For example, the features of a flower might include the measurements of its petals, stem, sepals, and other quantifiable characteristics. A machine learning model could be trained by applying an algorithm to these measurements that calculates the most likely species of the flower - its class.

![https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/train-classification.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/train-classification.png)

![https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/classification.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/classification.png)

## **Understand image classification**

*Image classification* is a machine learning technique in which the object being classified is an image, such as a photograph.

To create an image classification model, you need data that consists of features and their labels. The existing data is a set of categorized images. Digital images are made up of an array of pixel values, and these are used as features to train the model based on the known image classes.

![https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/train-image-classification.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/train-image-classification.png)

The model is trained to match the patterns in the pixel values to a set of class labels. After the model has been trained, you can use it with new sets of features to predict unknown label values.

## **Azure's Custom Vision service**

Most modern image classification solutions are based on *deep learning* techniques that make use of *convolutional neural networks* (CNNs) to uncover patterns in the pixels that correspond to particular classes. Training an effective CNN is a complex task that requires considerable expertise in data science and machine learning.

Common techniques used to train image classification models have been encapsulated into the **Custom Vision** cognitive service in Microsoft Azure; making it easy to train a model and publish it as a software service with minimal knowledge of deep learning techniques. You can use the Custom Vision cognitive service to train image classification models and deploy them as services for applications to use.

# **Get started with image classification on Azure**

You can perform image classification using the Custom Vision service, available as part of the Azure Cognitive Services offerings. This is generally easier and quicker than writing your own model training code, and enables people with little or no machine learning expertise to create an effective image classification solution.

## **Azure resources for Custom Vision**

Creating an image classification solution with Custom Vision consists of two main tasks. First you must use existing images to train the model, and then you must publish the model so that client applications can use it to generate predictions.

For each of these tasks, you need a resource in your Azure subscription. You can use the following types of resource:

- **Custom Vision**: A dedicated resource for the custom vision service, which can be *training*, a *prediction*, or *both* resources.
- **Cognitive Services**: A general cognitive services resource that includes Custom Vision along with many other cognitive services. You can use this type of resource for *training*, *prediction*, or both.

The separation of training and prediction resources is useful when you want to track resource utilization for model training separately from client applications using the model to predict image classes. However, it can make development of an image classification solution a little confusing.

The simplest approach is to use a general Cognitive Services resource for both training and prediction. This means you only need to concern yourself with one *endpoint* (the HTTP address at which your service is hosted) and *key* (a secret value used by client applications to authenticate themselves).

If you choose to create a Custom Vision resource, you will be prompted to choose *training*, *prediction*, or *both* - and it's important to note that if you choose "both", then ***two*** resources are created - one for training and one for prediction.

It's also possible to take a mix-and-match approach in which you use a dedicated Custom Vision resource for training, but deploy your model to a Cognitive Services resource for prediction. For this to work, the training and prediction resources must be created in the same region.

## **Model training**

To train a classification model, you must upload images to your training resource and label them with the appropriate class labels. Then, you must train the model and evaluate the training results.

You can perform these tasks in the *Custom Vision portal*, or if you have the necessary coding experience you can use one of the Custom Vision service programming language-specific software development kits (SDKs).

One of the key considerations when using images for classification, is to ensure that you have sufficient images of the objects in question and those images should be of the object from many different angles.

## **Model evaluation**

Model training process is an iterative process in which the Custom Vision service repeatedly trains the model using some of the data, but holds some back to evaluate the model. At the end of the training process, the performance for the trained model is indicated by the following evaluation metrics:

- **Precision**: What percentage of the class predictions made by the model were correct? For example, if the model predicted that 10 images are oranges, of which eight were actually oranges, then the precision is 0.8 (80%).
- **Recall**: What percentage of class predictions did the model correctly identify? For example, if there are 10 images of apples, and the model found 7 of them, then the recall is 0.7 (70%).
- **Average Precision (AP)**: An overall metric that takes into account both precision and recall).

## **Using the model for prediction**

After you've trained the model, and you're satisfied with its evaluated performance, you can publish the model to your prediction resource. When you publish the model, you can assign it a name (the default is "Iteration*X*", where X is the number of times you have trained the model).

To use your model, client application developers need the following information:

- **Project ID**: The unique ID of the Custom Vision project you created to train the model.
- **Model name**: The name you assigned to the model during publishing.
- **Prediction endpoint**: The HTTP address of the endpoints for the *prediction* resource to which you published the model (***not*** the training resource).
- **Prediction key**: The authentication key for the *prediction* resource to which you published the model (***not*** the training resource).

# **Exercise - Create an image classification solution**

The *Computer Vision* cognitive service provides useful pre-built models for working with images, but you'll often need to train your own model for computer vision. For example, suppose the Northwind Traders retail company wants to create an automated checkout system that identifies the grocery items customers want to buy based on an image taken by a camera at the checkout. To do this, you'll need to train a classification model that can classify the images to identify the item being purchased.

In Azure, you can use the ***Custom Vision*** cognitive service to train an image classification model based on existing images. There are two elements to creating an image classification solution. First, you must train a model to recognize different classes using existing images. Then, when the model is trained you must publish it as a service that can be consumed by applications.

To test the capabilities of the Custom Vision service, we'll use a simple command-line application that runs in the Cloud Shell. The same principles and functionality apply in real-world solutions, such as web sites or phone apps.

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

1. Download and extract the training images from [https://aka.ms/fruit-images](https://aka.ms/fruit-images). These images are provided in a zipped folder, which when extracted contains subfolders called **apple**, **banana**, and **orange**.
2. In another browser tab, open the Custom Vision portal at [https://customvision.ai](https://customvision.ai/). If prompted, sign in using the Microsoft account associated with your Azure subscription and agree to the terms of service.
3. In the Custom Vision portal, create a new project with the following settings:
    - **Name**: Grocery Checkout
    - **Description**: Image classification for groceries
    - **Resource**: *The Custom Vision resource you created previously*
    - **Project Types**: Classification
    - **Classification Types**: Multiclass (single tag per image)
    - **Domains**: Food
4. Click **[+] Add images**, and select all of the files in the **apple** folder you extracted previously. Then upload the image files, specifying the tag *apple*, like this:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/upload-apples.jpg](https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/upload-apples.jpg)
    
5. Repeat the previous step to upload the images in the **banana** folder with the tag *banana*, and the images in the **orange** folder with the tag *orange*.
6. Explore the images you have uploaded in the Custom Vision project - there should be 15 images of each class, like this:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/fruit.jpg](https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/fruit.jpg)
    
7. In the Custom Vision project, above the images, click **Train** to train a classification model using the tagged images. Select the **Quick Training** option, and then wait for the training iteration to complete (this may take a minute or so).
8. When the model iteration has been trained, review the *Precision*, *Recall*, and *AP* performance metrics - these measure the prediction accuracy of the classification model, and should all be high.

## **Test the model**

Before publishing this iteration of the model for applications to use, you should test it.

1. Above the performance metrics, click **Quick Test**.
2. In the **Image URL** box, type `https://aka.ms/apple-image` and click ➔
3. View the predictions returned by your model - the probability score for *apple* should be the highest, like this:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/test-apple.jpg](https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/test-apple.jpg)
    
4. Close the **Quick Test** window.

## **Publish the image classification model**

Now you're ready to publish your trained model and use it from a client application.

1. Click **🗸 Publish** to publish the trained model with the following settings:
    - **Model name**: groceries
    - **Prediction Resource**: *The prediction resource you created previously*.
2. After publishing, click the *Prediction URL* (🌐) icon to see information required to use the published model. Later, you will need the appropriate URL and Prediction-Key values to get a prediction from an Image URL, so keep this dialog box open and carry on to the next task.

## **Run Cloud Shell**

To test the capabilities of the Custom Vision service, we'll use a simple command-line application that runs in the Cloud Shell on Azure.

1. In the Azure portal, select the **[>_]** (*Cloud Shell*) button at the top of the page to the right of the search box. This opens a Cloud Shell pane at the bottom of the portal.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/powershell-portal-guide-1.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/powershell-portal-guide-1.png)
    
2. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). Select **PowerShell**. If you do not see this option, skip the step.
3. If you are prompted to create storage for your Cloud Shell, ensure your subscription is specified and select **Create storage**. Then wait a minute or so for the storage to be created.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/powershell-portal-guide-2.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/powershell-portal-guide-2.png)
    
4. Make sure the the type of shell indicated on the top left of the Cloud Shell pane is switched to *PowerShell*. If it is *Bash*, switch to *PowerShell* by using the drop-down menu.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/powershell-portal-guide-3.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/powershell-portal-guide-3.png)
    
5. Wait for PowerShell to start. You should see the following screen in the Azure portal:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/powershell-prompt.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/powershell-prompt.png)
    

## **Configure and run a client application**

Now that you have a Cloud Shell environment, you can run a simple application that uses the Custom Vision service to analyze an image.

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
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/powershell-portal-guide-4.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/powershell-portal-guide-4.png)
    
3. In the **Files** pane on the left, expand **ai-900** and select **classify-image.ps1**. This file contains some code that uses the Custom Vision model to analyze an image, as shown here:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/classify-image-code.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/classify-image-code.png)
    
4. Don't worry too much about the details of the code, the important thing is that it needs the prediction URL and key for your Custom Vision model when using an image URL.
    
    Get the *prediction URL* from the dialog box in your Custom Vision project.
    
    **Note**
    
    Remember, you reviewed the *prediction URL* after you published the image classification model. To find the *prediction URL*, navigate to the **Performance** tab in your project, then click on **Prediction URL** (if the screen is compressed, you may just see a globe icon). A dialogue box will appear. Copy the url for **If you have an image URL**. Paste it into the code editor, replacing **YOUR_PREDICTION_URL**.
    
    Using the same dialog box, get the *prediction key*. Copy the prediction key displayed after *Set Prediction-Key Header to*. Paste it in the code editor, replacing the **YOUR_PREDICTION_KEY** placeholder value.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/find-prediction-url.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/find-prediction-url.png)
    
    After pasting the Prediction URL and Prediction Key values, the first two lines of code should look similar to this:
    
    PowerShellCopy
    
    ```
    $predictionUrl="https..."
    $predictionKey ="1a2b3c4d5e6f7g8h9i0j...."
    
    ```
    
5. At the top right of the editor pane, use the **...** button to open the menu and select **Save** to save your changes. Then open the menu again and select **Close Editor**.
    
    You will use the sample client application to classify several images into the apple, banana, or orange category.
    
6. We will classify this image:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/fruit-1.jpg](https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/fruit-1.jpg)
    
    In the PowerShell pane, enter the following commands to run the code:
    
    Copy
    
    ```
    cd ai-900
    ./classify-image.ps1 1
    
    ```
    
7. Review the prediction, which should be **apple**.
8. Now let's try another image:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/fruit-2.jpg](https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/fruit-2.jpg)
    
    Please run this command:
    
    Copy
    
    ```
    ./classify-image.ps1 2
    
    ```
    
9. Verify that the model classifies this image as **banana**.
10. Finally, let's try the third test image:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/fruit-3.jpg](https://docs.microsoft.com/en-us/learn/wwl-data-ai/classify-images-custom-vision/media/fruit-3.jpg)
    
    Please run this command:
    
    Copy
    
    ```
    ./classify-image.ps1 3
    
    ```
    
11. Verify that the model classifies this image as **orange**.