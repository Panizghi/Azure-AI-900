# Module 3 / Translate

# **Introduction**

As organizations and individuals increasingly need to collaborate with people in other cultures and geographic locations, the removal of language barriers has become a significant problem.

One solution is to find bilingual, or even multilingual, people to translate between languages. However the scarcity of such skills, and the number of possible language combinations can make this approach difficult to scale. Increasingly, automated translation, sometimes known as *machine translation*, is being employed to solve this problem.

## **Literal and semantic translation**

Early attempts at machine translation applied *literal* translations. A literal translation is where each word is translated to the corresponding word in the target language. This approach presents some issues. For one case, there may not be an equivalent word in the target language. Another case is where literal translation can change the meaning of the phrase or not get the context correct.

For example, the French phrase "*éteindre la lumière*" can be translated to English as "*turn off the light*". However, in French you might also say "*fermer la lumiere*" to mean the same thing. The French verb *fermer* literally means to "*close*", so a literal translation based only on the words would indicate, in English, "*close the light*"; which for the average English speaker, doesn't really make sense, so to be useful, a translation service should take into account the semantic context and return an English translation of "*turn off the light*".

Artificial intelligence systems must be able to understand, not only the words, but also the *semantic* context in which they are used. In this way, the service can return a more accurate translation of the input phrase or phrases. The grammar rules, formal versus informal, and colloquialisms all need to be considered.

## **Text and speech translation**

*Text translation* can be used to translate documents from one language to another, translate email communications that come from foreign governments, and even provide the ability to translate web pages on the Internet. Many times you will see a *Translate* option for posts on social media sites, or the Bing search engine can offer to translate entire web pages that are returned in search results.

*Speech translation* is used to translate between spoken languages, sometimes directly (speech-to-speech translation) and sometimes by translating to an intermediary text format (speech-to-text translation).

# **Get started with translation in Azure**

Microsoft Azure provides cognitive services that support translation. Specifically, you can use the following services:

- The **Translator** service, which supports text-to-text translation.
- The **Speech** service, which enables speech-to-text and speech-to-speech translation.

## **Azure resources for Translator and Speech**

Before you can use the Translator or Speech services, you must provision appropriate resources in your Azure subscription.

There are dedicated **Translator** and **Speech** resource types for these services, which you can use if you want to manage access and billing for each service individually.

Alternatively, you can create a **Cognitive Services** resource that provides access to both services through a single Azure resource, consolidating billing and enabling applications to access both services through a single endpoint and authentication key.

## **Text translation with the Translator service**

The Translator service is easy to integrate in your applications, websites, tools, and solutions. The service uses a Neural Machine Translation (NMT) model for translation, which analyzes the semantic context of the text and renders a more accurate and complete translation as a result.

### **Translator service language support**

The Translator service supports text-to-text translation between [more than 60 languages](https://docs.microsoft.com/en-us/azure/cognitive-services/translator/languages). When using the service, you must specify the language you are translating ***from*** and the language you are translating ***to*** using ISO 639-1 language codes, such as *en* for English, *fr* for French, and *zh* for Chinese. Alternatively, you can specify cultural variants of languages by extending the language code with the appropriate 3166-1 cultural code - for example, *en-US* for US English, *en-GB* for British English, or *fr-CA* for Canadian French.

When using the Translator service, you can specify one ***from*** language with multiple ***to*** languages, enabling you to simultaneously translate a source document into multiple languages.

### **Optional Configurations**

The Translator API offers some optional configuration to help you fine-tune the results that are returned, including:

- **Profanity filtering**. Without any configuration, the service will translate the input text, without filtering out profanity. Profanity levels are typically culture-specific but you can control profanity translation by either marking the translated text as profane or by omitting it in the results.
- **Selective translation**. You can tag content so that it isn't translated. For example, you may want to tag code, a brand name, or a word/phrase that doesn't make sense when localized.

## **Speech translation with the Speech service**

The Speech service includes the following application programming interfaces (APIs):

- **Speech-to-text** - used to transcribe speech from an audio source to text format.
- **Text-to-speech** - used to generate spoken audio from a text source.
- **Speech Translation** - used to translate speech in one language to text or speech in another.

You can use the **Speech Translation** API to translate spoken audio from a streaming source, such as a microphone or audio file, and return the translation as text or an audio stream. This enables scenarios such as real-time closed captioning for a speech or simultaneous two-way translation of a spoken conversation.

### **Speech service language support**

As with the Translator service, you can specify one source language and one or more target languages to which the source should be translated. You can translate speech into [over 60 languages](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/language-support#speech-translation).

The source language must be specified using the extended language and culture code format, such as *es-US* for American Spanish. This requirement helps ensure that the source is understood properly, allowing for localized pronunciation and linguistic idioms.

The target languages must be specified using a two-character language code, such as *en* for English or *de* for German.

# **Exercise - Translate text and speech**

One of the driving forces that has enabled human civilization to develop is the ability to communicate with one another. In most human endeavors, communication is key.

Artificial Intelligence (AI) can help simplify communication by translating text or speech between languages, helping to remove barriers to communication across countries and cultures.

To test the capabilities of the Translator service, we'll use a simple command-line application that runs in the Cloud Shell. The same principles and functionality apply in real-world solutions, such as web sites or phone apps.

## **Create a *Cognitive Services* resource**

You can use the Translator service by creating either a **Translator** resource or a **Cognitive Services** resource.

If you haven't already done so, create a **Cognitive Services** resource in your Azure subscription.

1. In another browser tab, open the Azure portal at [https://portal.azure.com](https://portal.azure.com/), signing in with your Microsoft account.
2. Select the **＋Create a resource** button, search for *Cognitive Services*, and create a **Cognitive Services** resource with the following settings:
    - **Subscription**: *Your Azure subscription*.
    - **Resource group**: *Select or create a resource group with a unique name*.
    - **Region**: *Choose any available region*:
    - **Name**: *Enter a unique name*.
    - **Pricing tier**: S0
    - **I confirm I have read and understood the notices**: Selected.
3. Review and create the resource, and wait for deployment to complete. Then go to the deployed resource.
4. View the **Keys and Endpoint** page for your Cognitive Services resource. You will need the keys and location to connect from client applications.

### **Get the Key and Location for your Cognitive Services resource**

1. Wait for deployment to complete. Then go to your Cognitive Services resource, and on the **Overview** page, select the link to manage the keys for the service. You will need the keys and location to connect to your Cognitive Services resource from client applications.
2. View the **Keys and Endpoint** page for your resource. You will need the **location/region** and **key** to connect from client applications.

**Note**

To use the Translator service you do not need to use the Cognitive Service endpoint. A global endpoint just for the Translator service is provided.

## **Run Cloud Shell**

To test the capabilities of the Translation service, we'll use a simple command-line application that runs in the Cloud Shell on Azure.

1. In the Azure portal, select the **[>_]** (*Cloud Shell*) button at the top of the page to the right of the search box. This opens a Cloud Shell pane at the bottom of the portal.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/translate-text-with-translation-service/media/powershell-portal-guide-1.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/translate-text-with-translation-service/media/powershell-portal-guide-1.png)
    
2. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). Select **PowerShell**. If you do not see this option, skip the step.
3. If you are prompted to create storage for your Cloud Shell, ensure your subscription is specified and select **Create storage**. Then wait a minute or so for the storage to be created.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/translate-text-with-translation-service/media/powershell-portal-guide-2.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/translate-text-with-translation-service/media/powershell-portal-guide-2.png)
    
4. Make sure the type of shell indicated on the top left of the Cloud Shell pane is switched to *PowerShell*. If it is *Bash*, switch to *PowerShell* by using the drop-down menu.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/translate-text-with-translation-service/media/powershell-portal-guide-3.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/translate-text-with-translation-service/media/powershell-portal-guide-3.png)
    
5. Wait for PowerShell to start. You should see the following screen in the Azure portal:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/translate-text-with-translation-service/media/powershell-prompt.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/translate-text-with-translation-service/media/powershell-prompt.png)
    

## **Configure and run a client application**

Now that you have a custom model, you can run a simple client application that uses the Translation service.

1. In the command shell, enter the following command to download the sample application and save it to a folder called ai-900.
    
    Copy
    
    ```
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    
    ```
    
    **Tip**
    
    If you already used this command in another lab to clone the *ai-900* repository, you can skip this step.
    
2. The files are downloaded to a folder named **ai-900**. Now we want to see all of the files in your Cloud Shell storage and work with them. Type the following command into the shell:
    
    Copy
    
    ```
    code .
    
    ```
    
    Notice how this opens up an editor like the one in the image below:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/translate-text-with-translation-service/media/powershell-portal-guide-4.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/translate-text-with-translation-service/media/powershell-portal-guide-4.png)
    
3. In the **Files** pane on the left, expand **ai-900** and select **translator.ps1**. This file contains some code that uses the Translator service:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/translate-text-with-translation-service/media/translate-code.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/translate-text-with-translation-service/media/translate-code.png)
    
4. Don't worry too much about the details of the code, the important thing is that it needs the region/location and either of the keys for your Cognitive Services resource. Copy these from the **Keys and Endpoints** page for your resource from the Azure portal and paste them into the code editor, replacing the **YOUR_KEY** and **YOUR_LOCATION** placeholder values respectively.
    
    After pasting the key and location values, the first lines of code should look similar to this:
    
    PowerShellCopy
    
    ```
    $key="1a2b3c4d5e6f7g8h9i0j...."
    $location="somelocation"
    
    ```
    
5. At the top right of the editor pane, use the **...** button to open the menu and select **Save** to save your changes. Then open the menu again and select **Close Editor**.
    
    The sample client application will use the Translator service to do several tasks:
    
    - Translate text from English into French, Italian, and Chinese.
    - Translate audio from English into text in French
        - Use the video player below to hear the input audio the application will process:
    
    **Note**
    
    A real application could accept the input from a microphone and send the response to a speaker, but in this simple example, we'll use pre-recorded input in an audio file.
    
6. In the Cloud Shell pane, enter the following command to run the code:
    
    Copy
    
    ```
    cd ai-900
    ./translator.ps1
    
    ```
    
7. Review the output. Did you see the translation from text in English to French, Italian, and Chinese? Did you see the English audio "hello" translated into text in French?

## **Learn more**

This simple app shows only some of the capabilities of the Translator service. To learn more about what you can do with this service, see the [Translator page](https://docs.microsoft.com/en-us/azure/cognitive-services/translator/translator-overview).