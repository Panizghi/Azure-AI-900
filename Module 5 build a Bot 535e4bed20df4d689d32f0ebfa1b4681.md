# Module 5 / build a Bot

# **Introduction**

In today's connected world, people use a variety of technologies to communicate. For example:

- Voice calls
- Messaging services
- Online chat applications
- Email
- Social media platforms
- Collaborative workplace tools

We've become so used to ubiquitous connectivity, that we expect the organizations we deal with to be easily contactable and immediately responsive through the channels we already use. Additionally, we expect these organizations to engage with us individually, and be able to answer complex questions at a personal level.

## **Conversational AI**

While many organizations publish support information and answers to frequently asked questions (FAQs) that can be accessed through a web browser or dedicated app. The complexity of the systems and services they offer means that answers to specific questions are hard to find. Often, these organizations find their support personnel being overloaded with requests for help through phone calls, email, text messages, social media, and other channels.

Increasingly, organizations are turning to artificial intelligence (AI) solutions that make use of AI agents, commonly known as *bots* to provide a first-line of automated support through the full range of channels that we use to communicate. Bots are designed to interact with users in a conversational manner, as shown in this example of a chat interface:

![https://docs.microsoft.com/en-us/learn/wwl-data-ai/build-faq-chatbot-qna-maker-azure-bot-service/media/bot.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/build-faq-chatbot-qna-maker-azure-bot-service/media/bot.png)

**Note**

The example shown here is a chat interface, such as you might find on a web site; but bots can be designed to work across multiple channels, including email, social media platforms, and even voice calls. Regardless of the channel used, bots typically manage conversation flows using a combination of natural language and constrained option responses that guide the user to a resolution.

Conversations typically take the form of messages exchanged in turns; and one of the most common kinds of conversational exchange is a question followed by an answer. This pattern forms the basis for many user support bots, and can often be based on existing FAQ documentation. To implement this kind of solution, you need:

- A *knowledge base* of question and answer pairs - usually with some built-in natural language processing model to enable questions that can be phrased in multiple ways to be understood with the same semantic meaning.
- A *bot service* that provides an interface to the knowledge base through one or more channels.

# **Get started with the Language service and Azure Bot Service**

You can easily create a user support bot solution on Microsoft Azure using a combination of two core services:

- **Language service**. The Language service includes a custom question answering feature that enables you to create a knowledge base of question and answer pairs that can be queried using natural language input.
    
    **Note**
    
    The question answering capability in the Language service is a newer version of the QnA Maker service - which is still available as a separate service.
    
- **Azure Bot service**. This service provides a framework for developing, publishing, and managing bots on Azure.

## **Creating a custom question answering knowledge base**

The first challenge in creating a user support bot is to use the Language service to create a knowledge base. You can use the *Language Studio*'s custom question answering feature to create, train, publish, and manage knowledge bases.

**Note**

You can write code to create and manage knowledge bases using the Language service REST API or SDK. However, in most scenarios it is easier to use the Language Studio.

### **Provision a Language service Azure resource**

To create a knowledge base, you must first provision a **Language service** resource in your Azure subscription.

### **Define questions and answers**

After provisioning a Language service resource, you can use the Language Studio's custom question answering feature to create a knowledge base that consists of question-and-answer pairs. These questions and answers can be:

- Generated from an existing FAQ document or web page.
- Entered and edited manually.

In many cases, a knowledge base is created using a combination of all of these techniques; starting with a base dataset of questions and answers from an existing FAQ document and extending the knowledge base with additional manual entries.

Questions in the knowledge base can be assigned *alternative phrasing* to help consolidate questions with the same meaning. For example, you might include a question like:

> What is your head office location?
> 

You can anticipate different ways this question could be asked by adding an alternative phrasing such as:

> Where is your head office located?
> 

### **Test the knowledge base**

After creating a set of question-and-answer pairs, you must save it. This process analyzes your literal questions and answers and applies a built-in natural language processing model to match appropriate answers to questions, even when they are not phrased exactly as specified in your question definitions. Then you can use the built-in test interface in the Language Studio to test your knowledge base by submitting questions and reviewing the answers that are returned.

### **Use the knowledge base**

When you're satisfied with your knowledge base, deploy it. Then you can use it over its REST interface. To access the knowledge base, client applications require:

- The knowledge base ID
- The knowledge base endpoint
- The knowledge base authorization key

## **Build a bot with the Azure Bot Service**

After you've created and deployed a knowledge base, you can deliver it to users through a bot.

### **Create a bot for your knowledge base**

You can create a custom bot by using the Microsoft Bot Framework SDK to write code that controls conversation flow and integrates with your knowledge base. However, an easier approach is to use the automatic bot creation functionality, which enables you create a bot for your deployed knowledge base and publish it as an Azure Bot Service application with just a few clicks.

### **Extend and configure the bot**

After creating your bot, you can manage it in the Azure portal, where you can:

- Extend the bot's functionality by adding custom code.
- Test the bot in an interactive test interface.
- Configure logging, analytics, and integration with other services.

For simple updates, you can edit bot code directly in the Azure portal. However, for more comprehensive customization, you can download the source code and edit it locally; republishing the bot directly to Azure when you're ready.

### **Connect channels**

When your bot is ready to be delivered to users, you can connect it to multiple *channels*; making it possible for users to interact with it through web chat, email, Microsoft Teams, and other common communication media.

![https://docs.microsoft.com/en-us/learn/wwl-data-ai/build-faq-chatbot-qna-maker-azure-bot-service/media/bot-solution.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/build-faq-chatbot-qna-maker-azure-bot-service/media/bot-solution.png)

Users can submit questions to the bot through any of its channels, and receive an appropriate answer from the knowledge base on which the bot is based.

# **Exercise - Create a bot**

For customer support scenarios, it's common to create a bot that can interpret and answer frequently asked questions through a website chat window, email, or voice interface. Underlying the bot interface is a knowledge base of questions and appropriate answers that the bot can search for suitable responses.

## **Create a custom question answering knowledge base**

The Language service's custom question answering feature enables you to quickly create a knowledge base, either by entering question and answer pairs or from an existing document or web page. It can then use some built-in natural language processing capabilities to interpret questions and find appropriate answers.

1. Open the Azure portal at [https://portal.azure.com](https://portal.azure.com/), signing in with your Microsoft account.
2. Click the **＋Create a resource** button, search for *Language service*, and create a **Language service** resource with the following settings: **Select Additional Features**
    - **Default features**: *Keep the default features*
    - **Custom features**: *Select custom question answering* Click **Continue to create your resource**.
    - **Subscription**: *Your Azure subscription*
    - **Resource group**: *Select an existing resource group or create a new one*
    - **Name**: *A unique name for your Language resource*
    - **Pricing tier**: Standard S
    - **Azure Search location**: *Any available location*
    - **Azure Search pricing tier**: Free F (*If this tier is not available, select Standard (S)*)
    - **Legal Terms**: *Agree*
    - **Responsible AI Notice**: *Agree*
    
    **Note**
    
    If you have already provisioned a free-tier **Azure Cognitive Search** resources, your quota may not allow you to create another one. In which case, select a tier other than **F**.
    
3. Click **Review and Create** and then click **Create**. Wait for the deployment of the Language service that will support your custom question answering knowledge base.
4. In a new browser tab, open the Language Studio portal at [https://language.azure.com](https://language.azure.com/) and sign in using the Microsoft account associated with your Azure subscription.
5. If prompted to choose a Language resource, select the following settings:
    - **Azure Directory**: The Azure directory containing your subscription.
    - **Azure subscription**: Your Azure subscription.
    - **Language resource**: The Language resource you created previously.
6. If you are ***not*** prompted to choose a language resource, it may be because you have multiple Language resources in your subscription; in which case:
    1. On the bar at the top if the page, click the **Settings (⚙)** button.
    2. On the **Settings** page, view the **Resources** tab.
    3. Select the language resource you just created, and click **Switch resource**.
    4. At the top of the page, click **Language Studio** to return to the Language Studio home page.
7. At the top of the Language Studio portal, in the **Create new** menu, select **Custom question answering**.
8. On the **Enter basic information** page, enter the following details and click **Next**:
    - **Language resource**: *choose your language resource*.
    - **Azure search resource**: *choose your Azure search resource*.
    - **Name**: MargiesTravel
    - **Description**: A simple knowledge base
    - **Source language**: English
    - **Default answer when no answer is returned**: No answer found
9. On the *Review and finish* page, click **Create project**.
10. You will be taken to the **Manage sources** page. Click **＋Add source** and select **URLs**.
11. In the **Add URLs** box, click **+ Add URL**. Type in the following:
    - **URL name**: MargiesKB
    - **URL**: `https://raw.githubusercontent.com/MicrosoftLearning/AI-900-AIFundamentals/main/data/qna/margies_faq.docx`
    - **Classify file structure**: *Auto-detect* Select **Add all**.

## **Edit the knowledge base**

Your knowledge base is based on the details in the FAQ document and some pre-defined responses. You can add custom question-and-answer pairs to supplement these.

1. Click **Edit knowledge base** on the left hand panel. Then click **+ Add question pair**.
2. In the **Questions** box, type `Hello`. Then click **+ Add alternative phrasing** and type `Hi`.
3. In the **Answer and prompts** box, type `Hello`. Keep the **Source**: Editorial.
4. Click **Submit**. Then at the top of the page click **Save changes**. You may need to change the size of your window to see the button.

## **Train and test the knowledge base**

Now that you have a knowledge base, you can test it.

1. At the top of the page, click **Test** to test your knowledge base.
2. In the test pane, at the bottom enter the message *Hi*. The response **Hello** should be returned.
3. In the test pane, at the bottom enter the message *I want to book a flight*. An appropriate response from the FAQ should be returned.
    
    **Note**
    
    The response includes a *short answer* as well as a more verbose *answer passage* - the answer passage shows the full text in the FAQ document for the closest matched question, while the short answer is intelligently extracted from the passage. You can control whether the short answer is from the response by using the **Display short answer** checkbox at the top of the test pane.
    
4. Try another question, such as *How can I cancel a reservation?*
5. When you're done testing the knowledge base, click **Test** to close the test pane.

## **Create a bot for the knowledge base**

The knowledge base provides a back-end service that client applications can use to answer questions through some sort of user interface. Commonly, these client applications are bots. To make the knowledge base available to a bot, you must publish it as a service that can be accessed over HTTP. You can then use the Azure Bot Service to create and host a bot that uses the knowledge base to answer user questions.

1. At the left of the Language Studio page, click **Deploy knowledge base**. Click **Deploy**.
2. After the service has been deployed, click **Create a Bot**. This opens the Azure portal in a new browser tab so you can create a Web App Bot in your Azure subscription.
3. In the Azure portal, create a Web App Bot with the following settings (most of these will be pre-populated for you):
    - **Bot handle**: *A unique name for your bot*
    - **Subscription**: *Your Azure subscription*
    - **Resource group**: *The resource group containing your Language resource*
    - **Location**: *The same location as your Language service*.
    - **Pricing tier**: Free (F0)
    - **App name**: *Same as the **Bot handle** with **.azurewebsites.net** appended automatically*
    - **SDK language**: *Choose either C# or Node.js*
    - **Language Resource Key**: *automatically generated, if you do not see it, you need to start by creating a question answering project in the Language Studio*
    - **App service plan/Location**: *Select the arrow to create a plan. Then create a unique App service plan name and choose a suitable location*
    - **Application Insights**: Off
    - **Microsoft App ID and password**: *Auto create App ID and password*
4. Wait for your bot to be created (the notification icon at the top right, which looks like a bell, will be animated while you wait). Then in the notification that deployment has completed, click **Go to resource** (or alternatively, on the home page, click **Resource groups**, open the resource group where you created the web app bot, and click it.)
5. In the left-hand pane of your bot look for **Settings**, click on **Test in Web Chat**, and wait until the bot displays the message **Hello and welcome!** (it may take a few seconds to initialize).
6. Use the test chat interface to ensure your bot answers questions from your knowledge base as expected. For example, try submitting *I need to cancel my hotel*.

Experiment with the bot. You'll probably find that it can answer questions from the FAQ quite accurately, but it will have limited ability to interpret questions that it has not been trained with. You can always use the Language Studio to edit the knowledge base to improve it, and republish it.

## **Learn more**

- To learn more about the Question Answering service, view [the documentation](https://docs.microsoft.com/en-us/azure/cognitive-services/language-service/question-answering/overview).
- To learn more about the Microsoft Bot Service, view [the Azure Bot Service page](https://azure.microsoft.com/services/bot-service/).