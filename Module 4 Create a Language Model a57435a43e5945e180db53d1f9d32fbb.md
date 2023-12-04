# Module 4 / Create a Language Model

# **Introduction**

In 1950, the British mathematician Alan Turing devised the *Imitation Game*, which has become known as the *Turing Test* and hypothesizes that if a dialog is natural enough, you may not know whether you're conversing with a human or a computer. As artificial intelligence (AI) grows ever more sophisticated, this kind of conversational interaction with applications and digital assistants is becoming more and more common, and in specific scenarios can result in human-like interactions with AI agents. Common scenarios for this kind of solution include customer support applications, reservation systems, and home automation among others.

To realize the aspiration of the imitation game, computers need not only to be able to accept language as input (either in text or audio format), but also to be able to interpret the semantic meaning of the input - in other words, *understand* what is being said.

On Microsoft Azure, conversational language understanding is supported through the **Language Service**. To work with Conversational Language Understanding, you need to take into account three core concepts: *utterances*, *entities*, and *intents*.

## **Utterances**

An utterance is an example of something a user might say, and which your application must interpret. For example, when using a home automation system, a user might use the following utterances:

> "Switch the fan on."
> 
> 
> "*Turn on the light.*"
> 

## **Entities**

An entity is an item to which an utterance refers. For example, **fan** and **light** in the following utterances:

> "Switch the fan on."
> 
> 
> "*Turn on the **light**.*"
> 

You can think of the **fan** and **light** entities as being specific instances of a general **device** entity.

## **Intents**

An intent represents the purpose, or goal, expressed in a user's utterance. For example, for both of the previously considered utterances, the intent is to turn a device on; so in your Conversational Language Understanding application, you might define a **TurnOn** intent that is related to these utterances.

A Language Understanding application defines a model consisting of intents and entities. Utterances are used to train the model to identify the most likely intent and the entities to which it should be applied based on a given input. The home assistant application we've been considering might include multiple intents, like the following examples:

| Intent | Related Utterances | Entities |
| --- | --- | --- |
| Greeting | "Hello" |  |
|  | "Hi" |  |
|  | "Hey" |  |
|  | "Good morning" |  |
| TurnOn | "Switch the fan on" | fan (device) |
|  | "Turn the light on" | light (device) |
|  | "Turn on the light" | light (device) |
| TurnOff | "Switch the fan off" | fan (device) |
|  | "Turn the light off" | light (device) |
|  | "Turn off the light" | light (device) |
| CheckWeather | "What is the weather for today?" | today (datetime) |
|  | "Give me the weather forecast" |  |
|  | "What is the forecast for Paris?" | Paris (location) |
|  | "What will the weather be like in Seattle tomorrow?" | Seattle (location), tomorrow (datetime) |
| None | "What is the meaning of life?" |  |
|  | "Is this thing on?" |  |

In this table there are numerous utterances used for each of the intents. The intent should be a concise way of grouping the utterance tasks. Of special interest is the ***None*** intent. You should consider always using the None intent to help handle utterances that do not map any of the utterances you have entered. The None intent is considered a fallback, and is typically used to provide a generic response to users when their requests don't match any other intent.

**Tip**

In a Conversational Language Understanding application, the **None** intent is created but left empty on purpose. The None intent is a required intent and can't be deleted or renamed. Fill it with utterances that are outside of your domain.

After defining the entities and intents with sample utterances in your Conversational Language Understanding application, you can train a language model to predict intents and entities from user input - even if it doesn't match the sample utterances exactly. You can then use the model from a client application to retrieve predictions and respond appropriately.

# **Getting started with Conversational Language Understanding**

Creating an application with Conversational Language Understanding consists of two main tasks. First you must define entities, intents, and utterances with which to train the language model - referred to as *authoring* the model. Then you must publish the model so that client applications can use it for intent and entity *prediction* based on user input.

## **Azure resources for Conversational Language Understanding**

For each of the authoring and prediction tasks, you need a resource in your Azure subscription. You can use the following types of resource:

- **Language Service**: A resource that enables you to build apps with industry-leading natural language understanding capabilities without machine learning expertise.
- **Cognitive Services**: A general cognitive services resource that includes Conversational Language Understanding along with many other cognitive services. You can only use this type of resource for *prediction*.

The separation of resources is useful when you want to track resource utilization for Language Service use separately from client applications using all Cognitive Services applications.

When your client application uses a Cognitive Services resource, you can manage access to all of the cognitive services being used, including the Language Service, through a single endpoint and key.

## **Authoring**

After you've created an authoring resource, you can use it to author and train a Conversational Language Understanding application by defining the entities and intents that your application will predict as well as utterances for each intent that can be used to train the predictive model.

Conversational Language Understanding provides a comprehensive collection of prebuilt *domains* that include pre-defined intents and entities for common scenarios; which you can use as a starting point for your model. You can also create your own entities and intents.

When you create entities and intents, you can do so in any order. You can create an intent, and select words in the sample utterances you define for it to create entities for them; or you can create the entities ahead of time and then map them to words in utterances as you're creating the intents.

You can write code to define the elements of your model, but in most cases it's easiest to author your model using the Language Understanding portal - a web-based interface for creating and managing Conversational Language Understanding applications.

**Tip**

Best practice is to use the Language portal for authoring and to use the SDK for runtime predictions.

### **Creating intents**

Define intents based on actions a user would want to perform with your application. For each intent, you should include a variety of utterances that provide examples of how a user might express the intent.

If an intent can be applied to multiple entities, be sure to include sample utterances for each potential entity; and ensure that each entity is identified in the utterance.

### **Creating entities**

There are four types of entities:

- **Machine-Learned**: Entities that are learned by your model during training from context in the sample utterances you provide.
- **List**: Entities that are defined as a hierarchy of lists and sublists. For example, a **device** list might include sublists for **light** and **fan**. For each list entry, you can specify synonyms, such as **lamp** for **light**.
- **RegEx**: Entities that are defined as a *regular expression* that describes a pattern - for example, you might define a pattern like **[0-9]{3}-[0-9]{3}-[0-9]{4}** for telephone numbers of the form ***555-123-4567***.
- **Pattern.any**: Entities that are used with *patterns* to define complex entities that may be hard to extract from sample utterances.

### **Training the model**

After you have defined the intents and entities in your model, and included a suitable set of sample utterances; the next step is to train the model. Training is the process of using your sample utterances to teach your model to match natural language expressions that a user might say to probable intents and entities.

After training the model, you can test it by submitting text and reviewing the predicted intents. Training and testing is an iterative process. After you train your model, you test it with sample utterances to see if the intents and entities are recognized correctly. If they're not, make updates, retrain, and test again.

## **Predicting**

When you are satisfied with the results from the training and testing, you can publish your Conversational Language Understanding application to a prediction resource for consumption.

Client applications can use the model by connecting to the endpoint for the prediction resource, specifying the appropriate authentication key; and submit user input to get predicted intents and entities. The predictions are returned to the client application, which can then take appropriate action based on the predicted intent.

# **Exercise - Create a Conversational Language Understanding application**

Increasingly, we expect computers to be able to use AI in order to understand spoken or typed commands in natural language. For example, you might want to implement a home automation system that enables you to control devices in your home by using voice commands such as "switch on the light" or "put the fan on", and have an AI-powered device understand the command and take appropriate action.

To test the capabilities of the Conversational Language Understanding service, we'll use a command-line application that runs in the Cloud Shell. The same principles and functionality apply in real-world solutions, such as web sites or phone apps.

## **Create a *Language service* resource**

You can use the Conversational Language Understanding service by creating a **Language service** resource.

If you haven't already done so, create a **Language service** resource in your Azure subscription.

1. In another browser tab, open the Azure portal at [https://portal.azure.com](https://portal.azure.com/), signing in with your Microsoft account.
2. Click the **＋Create a resource** button, search for *Language service*, and create a **Language service** resource with the following settings:
    - Select additional features: *Keep the default features and click Continue to create your resource*
    - **Subscription**: *Your Azure subscription*.
    - **Resource group**: *Select or create a resource group with a unique name*.
    - **Region**: *Choose either the West US 2 or West Europe region*
    - **Name**: *Enter a unique name*.
    - **Pricing tier**: Standard (S)
    - **Legal Terms**: *Agree*
    - **Responsible AI Notice**: *Agree*
3. Review and create the resource, and wait for deployment to complete. Then go to the deployed resource.
4. View the **Keys and Endpoint** page for your Language service resource. You will need the endpoint and keys to connect from client applications.

### **Create a Conversational Language Understanding App**

To implement natural language understanding with Conversational Language Understanding, you create an app; and then add entities, intents, and utterances to define the commands you want the app.

1. In a new browser tab, open the Language Studio portal at [https://language.azure.com](https://language.azure.com/) and sign in using the Microsoft account associated with your Azure subscription.
2. If prompted to choose a Language resource, select the following settings:
    - **Azure Directory**: The Azure directory containing your subscription.
    - **Azure subscription**: Your Azure subscription.
    - **Language resource**: The Language resource you created previously.
    
    **Tip**
    
    If you are ***not*** prompted to choose a language resource, it may be because you have multiple Language resources in your subscription; in which case: 1. On the bar at the top if the page, click the **Settings (⚙)** button. 2. On the **Settings** page, view the **Resources** tab. 3. Select your language resource, and click **Switch resource**. 4. At the top of the page, click **Language Studio** to return to the Language Studio home page.
    
3. At the top of the portal, in the **Create new** menu, select **Conversational language understanding**.
4. In the **Create a project** dialog box, on the **Enter basic information** page, enter the following details and click **Next**:
    - **Name**: HomeAutomation
    - **Description**: Simple home automation
    - **Utterances primary language**: English
    - **Enable multiple languages in project**: *Do not select*
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/create-project.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/create-project.png)
    
5. On the *Review and finish* page, click **Create**.

### **Create intents, utterances, and entities**

An *intent* is an action you want to perform - for example, you might want to switch on a light, or turn off a fan. In this case, you'll define two intents: one to switch on a device, and another to switch off a device. For each intent, you'll specify sample *utterances* that indicate the kind of language used to indicate the intent.

1. In the **Build schema** pane, ensure that **Intents** is selected Then click **Add**, and add an intent with the name **switch_on** (in lower-case) and click **Add intent**.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/build-schema.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/build-schema.png)
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/add-intent.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/add-intent.png)
    
2. Select the **switch_on** intent. It will take you to the **Tag utterances** page. Next to the **switch_on** intent, type the utterance ***turn the light on*** and press **Enter** to submit this utterance to the list.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/add-utterance-on.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/add-utterance-on.png)
    
3. The language service needs at least five different utterance examples for each intent to sufficiently train the language model. Add five more utterance examples for the **switch_on** intent:
    - ***switch on the fan***
    - ***put the fan on***
    - ***put the light on***
    - ***switch on the light***
    - ***turn the fan on***
4. On the **Tagging entities for training** pane on the right-hand side of the screen, select **Tags**, then select **Add entity**. Type **device** (in lower-case), select **List** and select **Done**.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/add-entity.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/add-entity.png)
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/add-entity-device.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/add-entity-device.png)
    
5. In the ***turn the fan on*** utterance, highlight the word "fan". Then in the list that appears, in the *Search for an entity* box select **device**.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/switch-on-entity.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/switch-on-entity.png)
    
6. Do the same for all the utterances. Tag the rest of the *fan* or *light* utterances with the **device** entity. When you're finished, verify that you have the following utterances and make sure to select **Save changes**:
    
    
    | intent | utterance | entity |
    | --- | --- | --- |
    | switch_on | Put on the fan | Device - select fan |
    | switch_on | Put on the light | Device - select light |
    | switch_on | Switch on the light | Device - select light |
    | switch_on | Turn the fan on | Device - select fan |
    | switch_on | Switch on the fan | Device - select fan |
    | switch_on | Turn the light on | Device - select light |
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/save-changes.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/save-changes.png)
    
7. In the pane on the left, click **Build schema** and verify that your **switch_on** intent is listed. Then click **Add** and add a new intent with the name **switch_off** (in lower-case).
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/add-switch-off.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/add-switch-off.png)
    
8. Click on the **switch_off** intent. It will take you to the **Tag utterances** page. Next to the **switch_off** intent, add the utterance ***turn the light off***.
9. Add five more utterance examples to the **switch_off** intent.
    - ***switch off the fan***
    - ***put the fan off***
    - ***put the light off***
    - ***turn off the light***
    - ***switch the fan off***
10. Tag the words *light* or *fan* with the **device** entity. When you're finished, verify that you have the following utterances and make sure to select **Save changes**:
    
    [Untitled Database](Module%204%20Create%20a%20Language%20Model%20a57435a43e5945e180db53d1f9d32fbb/Untitled%20Database%20706de82a51a84ba7b790c105e50b2d5d.csv)
    

### **Train the model**

Now you're ready to use the intents and entities you have defined to train the conversational language model for your app.

1. On the left hand side of Language Studio, select **Train model**. Use the following settings:
    - **Train a new model**: *Selected and choose a model name*
    - **Run evaluation with training**: *Enabled evaluation*
    - Click **Train** at the bottom of the page.
2. Wait for training to complete.

### **Deploy and test the model**

To use your trained model in a client application, you must deploy it as an endpoint to which the client applications can send new utterances; from which intents and entities will be predicted.

1. On the left-hand side of Language Studio, click **Deploy model**.
2. Select your model name and click **Add deployment**. Use these settings:
    - **Create a new deployment name**: *Create a unique name*
    - **Assign trained model to your deployment name**: *Select the name of the trained model* Click **Submit**.
3. When the model is deployed, click **Test model** on the left-hand side of the page, and then select your deployed model under **Deployment name**.
4. Enter the following text, and then click **Run the test**:
    
    *switch the light on*
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/test-model.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/test-model.png)
    
    Review the result that is returned, noting that it includes the predicted intent (which should be **switch_on**) and the predicted entity (**device**) with confidence scores that indicates the probability the model calculated for the predicted intent and entity. The JSON tab shows the comparative confidence for each potential intent (the one with the highest confidence score is the predicted intent)
    
5. Clear the text box and test the model with the following utterances:
    - *turn off the fan*
    - *put the light on*
    - *put the fan off*

## **Run Cloud Shell**

Now let's try out your deployed model. To do so, we'll use a command-line application that runs in the Cloud Shell on Azure.

1. Leaving the browser tab with Language Studio open, switch back to browser tab containing the Azure portal.
2. In the Azure portal, select the **[>_]** (*Cloud Shell*) button at the top of the page to the right of the search box. Clicking the button opens a Cloud Shell pane at the bottom of the portal.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/powershell-portal-guide-1.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/powershell-portal-guide-1.png)
    
3. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). Select **PowerShell**. If you do not see this option, skip the step.
4. If you are prompted to create storage for your Cloud Shell, ensure your subscription is specified and select **Create storage**. Then wait a minute or so for the storage to be created.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/powershell-portal-guide-2.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/powershell-portal-guide-2.png)
    
5. Make sure the type of shell indicated on the top left of the Cloud Shell pane is switched to *PowerShell*. If it is *Bash*, switch to *PowerShell* by using the drop-down menu.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/powershell-portal-guide-3.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/powershell-portal-guide-3.png)
    
6. Wait for PowerShell to start. You should see the following screen in the Azure portal:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/powershell-prompt.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/powershell-prompt.png)
    

## **Configure and run a client application**

Now let's open and edit a pre-written script, which will run the client application.

1. In the command shell, enter the following command to download the sample application and save it to a folder called ai-900.
    
    Copy
    
    ```
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    
    ```
    
    **Note**
    
    If you already used this command in another lab to clone the *ai-900* repository, you can skip this step.
    
2. The files are downloaded to a folder named **ai-900**. Now we want to see all of the files in this folder and work with them. Type the following commands into the shell:
    
    Copy
    
    ```
    cd ai-900
    code .
    
    ```
    
    Notice how the script opens up an editor like the one in the image below:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/powershell-portal-guide-4.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/create-language-model-with-language-understanding/media/powershell-portal-guide-4.png)
    
3. In the **Files** pane on the left, select the **understand.ps1** file in the **ai-900** folder. This file contains some code that uses your Conversational Language Understanding model.
    
    Don't worry too much about the details of the code, the important thing is that it needs the endpoint and key for your Language service model. You'll get the endpoint and key from the Language Studio.
    
4. Switch back to the browser tab containing Language Studio. Then in Language Studio, open the **Deploy model** page and select your model. Then click the **Get prediction URL** button. The two pieces of information you need are in this dialog box:
    - The endpoint for your model - you can copy the endpoint from the **Prediction URL** box.
    - The key for your model - the key is in the **Sample request** as the value for the **Ocp-Apim-Subscription-Key** parameter, and looks similar to ***0ab1c23de4f56gh7i8901234jkl567m8***.
5. Copy the endpoint value, then switch back to the browser tab containing the Cloud Shell and paste it into the code editor, replacing **YOUR_ENDPOINT** (within the quotation marks). The repeat that process for the key, replacing **YOUR_KEY**.
    
    After pasting the key and endpoint values, the first lines of code should look similar to what you see below:
    
    PowerShellCopy
    
    ```
    $endpointUrl="https://some-name.cognitiveservices.azure.com/language/..."
    $key = "0ab1c23de4f56gh7i8901234jkl567m8"
    
    ```
    
6. At the top right of the editor pane, use the **...** button to open the menu and select **Save** to save your changes. Then open the menu again and select **Close Editor**.
7. In the PowerShell pane, enter the following command to run the code:
    
    Copy
    
    ```
    ./understand.ps1 "Turn on the light"
    
    ```
    
8. Review the results. The app should have predicted that the intended action is to switch on the light.
9. Now try another command:
    
    Copy
    
    ```
    ./understand.ps1 "Switch the fan off"
    
    ```
    
10. Review the results from this command. The app should have predicted that the intended action is to switch off the fan.
11. Experiment with a few more commands; including commands that the model was not trained to support, such as "Hello" or "switch on the oven". The app should generally understand commands for which its language model is defined, and fail gracefully for other input.

## **Learn more**

This app shows only some of the capabilities of the Conversational Language Understanding feature of the Language service. To learn more about what you can do with this service, see the [Conversational Language Understanding page](https://docs.microsoft.com/en-us/azure/cognitive-services/language-service/conversational-language-understanding/overview).