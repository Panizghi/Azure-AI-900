# Module 6 / Receipt Analysis

# **Introduction**

A common problem in many organizations is the need to process receipt or invoice data. For example, a company might require expense claims to be submitted electronically with scanned receipts, or invoices might need to be digitized and routed to the correct accounts department. Typically after a document is scanned, someone will still need to manually enter the extracted text into a database.

Increasingly, organizations with large volumes of receipts and invoices to process are looking for artificial intelligence (AI) solutions that can not only extract the text data from receipts, but also intelligently interpret the information they contain.

Azure's Form Recognizer service can solve for this issue by digitizing fields from forms using optical character recognition (OCR). Azure's OCR technologies extract the contents and structure from forms, such as key, value pairs (eg. Quantity: 3).

![https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-receipts-form-recognizer/media/contoso-receipt-small.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-receipts-form-recognizer/media/contoso-receipt-small.png)

Using the Form Recognizer service, we can input an image of a receipt like the one above, and return useful information that might be required for an expense claim, including:

- The name, address, and telephone number of the merchant.
- The date and time of the purchase.
- The quantity and price of each item purchased.
- The subtotal, tax, and total amounts.

In this module you will:

- Identify suitable Azure services for processing receipts
- Provision a Form Recognizer resource
- Use a Form Recognizer resource to extract information from a receipt

# **Get started with receipt analysis on Azure**

The **Form Recognizer** in Azure provides intelligent form processing capabilities that you can use to automate the processing of data in documents such as forms, invoices, and receipts. It combines state-of-the-art optical character recognition (OCR) with predictive models that can interpret form data by:

- Matching field names to values.
- Processing tables of data.
- Identifying specific types of field, such as dates, telephone numbers, addresses, totals, and others.

Form Recognizer supports automated document processing through:

- **A pre-built receipt model** that is provided out-of-the-box, and is trained to recognize and extract data from sales receipts.
- **Custom models**, which enable you to extract what are known as key/value pairs and table data from forms. Custom models are trained using your own data, which helps to tailor this model to your specific forms. Starting with only five samples of your forms, you can train the custom model. After the first training exercise, you can evaluate the results and consider if you need to add more samples and re-train.

**Note**

The next hands-on exercise will only step through **a pre-built receipt model**. If you would like to train a **custom model** you can refer to the **[Form Recognizer documentation](https://docs.microsoft.com/en-us/azure/cognitive-services/form-recognizer/)** for quickstarts.

## **Azure resources to access Form Recognizer services**

To use the Form recognizer, you need to either create a **Form Recognizer** resource or a **Cognitive Services** resource in your Azure subscription. Both resource types give access to the Form Recognizer service.

After the resource has been created, you can create client applications that use its **key** and **endpoint** to connect submit forms for analysis.

## **Using the pre-built receipt model**

Currently the pre-built receipt model is designed to recognize common receipts, in English, that are common to the USA. Examples are receipts used at restaurants, retail locations, and gas stations. The model is able to extract key information from the receipt slip:

- time of transaction
- date of transaction
- merchant information
- taxes paid
- receipt totals
- other pertinent information that may be present on the receipt
- all text on the receipt is recognized and returned as well

Use the following guidelines to get the best results when using a custom model.

- Images must be JPEG, PNG, BMP, PDF, or TIFF formats
- File size must be less than 50 MB
- Image size between 50 x 50 pixels and 10000 x 10000 pixels
- For PDF documents, no larger than 17 inches x 17 inches

There is a free tier subscription plan for the receipt model along with paid subscriptions. For the free tier, only the first two pages will be processed when passing in PDF or TIFF formatted documents.

# **Exercise - Analyze receipts with Form Recognizer**

In the artificial intelligence (AI) field of computer vision, optical character recognition (OCR) is commonly used to read printed or handwritten documents. Often, the text is simply extracted from the documents into a format that can be used for further processing or analysis.

A more advanced OCR scenario is the extraction of information from forms, such as purchase orders or invoices, with a semantic understanding of what the fields in the form represent. The **Form Recognizer** service is specifically designed for this kind of AI problem.

Form Recognizer uses machine learning models trained to extract text from images of invoices, receipts, and more. While other computer vision models can capture text, Form Recognizer also captures the structure of the text, such as key/value pairs and information in tables. This way, instead of having to manually type in entries from a form into a database, you can automatically capture the relationships between text from the original file.

To test the capabilities of the Form Recognizer service, we'll use a simple command-line application that runs in the Cloud Shell. The same principles and functionality apply in real-world solutions, such as web sites or phone apps.

## **Create a *Cognitive Services* resource**

You can use the Form Recognizer service by creating either a **Form Recognizer** resource or a **Cognitive Services** resource.

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

To test the capabilities of the Form Recognizer service, we'll use a simple command-line application that runs in the Cloud Shell on Azure.

1. In the Azure portal, select the **[>_]** (*Cloud Shell*) button at the top of the page to the right of the search box. This opens a Cloud Shell pane at the bottom of the portal.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-receipts-form-recognizer/media/powershell-portal-guide-1.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-receipts-form-recognizer/media/powershell-portal-guide-1.png)
    
2. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). Select **PowerShell**. If you do not see this option, skip the step.
3. If you are prompted to create storage for your Cloud Shell, ensure your subscription is specified and select **Create storage**. Then wait a minute or so for the storage to be created.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-receipts-form-recognizer/media/powershell-portal-guide-2.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-receipts-form-recognizer/media/powershell-portal-guide-2.png)
    
4. Make sure the the type of shell indicated on the top left of the Cloud Shell pane is switched to *PowerShell*. If it is *Bash*, switch to *PowerShell* by using the drop-down menu.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-receipts-form-recognizer/media/powershell-portal-guide-3.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-receipts-form-recognizer/media/powershell-portal-guide-3.png)
    
5. Wait for PowerShell to start. You should see the following screen in the Azure portal:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-receipts-form-recognizer/media/powershell-prompt.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-receipts-form-recognizer/media/powershell-prompt.png)
    

## **Configure and run a client application**

Now that you have a custom model, you can run a simple client application that uses the Form Recognizer service.

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
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-receipts-form-recognizer/media/powershell-portal-guide-4.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-receipts-form-recognizer/media/powershell-portal-guide-4.png)
    
3. In the **Files** pane on the left, expand **ai-900** and select **form-recognizer.ps1**. This file contains some code that uses the Form Recognizer service to analyze the fields in a receipt, as shown here:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-receipts-form-recognizer/media/recognize-receipt-code.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-receipts-form-recognizer/media/recognize-receipt-code.png)
    
4. Don't worry too much about the details of the code, the important thing is that it needs the endpoint URL and either of the keys for your Cognitive Services resource. Copy these from the **Keys and Endpoints** page for your resource from the Azure portal and paste them into the code editor, replacing the **YOUR_KEY** and **YOUR_ENDPOINT** placeholder values respectively.
    
    **Tip**
    
    You may need to use the separator bar to adjust the screen area as you work with the **Keys and Endpoint** and **Editor** panes.
    
    After pasting the key and endpoint values, the first two lines of code should look similar to this:
    
    PowerShellCopy
    
    ```
    $key="1a2b3c4d5e6f7g8h9i0j...."
    $endpoint="https..."
    
    ```
    
5. At the top right of the editor pane, use the **...** button to open the menu and select **Save** to save your changes. Then open the menu again and select **Close Editor**. Now that you've set up the key and endpoint, you can use your resource to analyze fields from a receipt. In this case, you'll use the Form Recognizer's built-in model to analyze a receipt for the fictional Northwind Traders retail company.
    
    The sample client application will analyze the following image:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-receipts-form-recognizer/media/receipt.jpg](https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-receipts-form-recognizer/media/receipt.jpg)
    
6. In the PowerShell pane, enter the following commands to run the code to read the text:
    
    Copy
    
    ```
    cd ai-900
    ./form-recognizer.ps1
    
    ```
    
7. Review the returned results. See that Form Recognizer is able to interpret the data in the form, correctly identifying the merchant address and phone number, and the transaction date and time, as well as the line items, subtotal, tax, and total amounts.

## **Learn more**

This simple app shows only some of the Form Recognizer capabilities of the Computer Vision service. To learn more about what you can do with this service, see the [Form Recognizer page](https://docs.microsoft.com/en-us/azure/applied-ai-services/form-recognizer/overview).