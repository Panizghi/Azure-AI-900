# Module 1 / Text Analysis

# **Introduction**

Analyzing text is a process where you evaluate different aspects of a document or phrase, in order to gain insights into the content of that text. For the most part, humans are able to read some text and understand the meaning behind it. Even without considering grammar rules for the language the text is written in, specific insights can be identified in the text.

As an example, you might read some text and identify some key phrases that indicate the main talking points of the text. You might also recognize names of people or well-known landmarks such as the Eiffel Tower. Although difficult at times, you might also be able to get a sense for how the person was feeling when they wrote the text, also commonly known as sentiment.

## **Text Analytics Techniques**

Text analytics is a process where an artificial intelligence (AI) algorithm, running on a computer, evaluates these same attributes in text, to determine specific insights. A person will typically rely on their own experiences and knowledge to achieve the insights. A computer must be provided with similar knowledge to be able to perform the task. There are some commonly used techniques that can be used to build software to analyze text, including:

- Statistical analysis of terms used in the text. For example, removing common "stop words" (words like "the" or "a", which reveal little semantic information about the text), and performing *frequency analysis* of the remaining words (counting how often each word appears) can provide clues about the main subject of the text.
- Extending frequency analysis to multi-term phrases, commonly known as *N-grams* (a two-word phrase is a *bi-gram*, a three-word phrase is a *tri-gram*, and so on).
- Applying *stemming* or *lemmatization* algorithms to normalize words before counting them - for example, so that words like "power", "powered", and "powerful" are interpreted as being the same word.
- Applying linguistic structure rules to analyze sentences - for example, breaking down sentences into tree-like structures such as a *noun phrase*, which itself contains *nouns*, *verbs*, *adjectives*, and so on.
- Encoding words or terms as numeric features that can be used to train a machine learning model. For example, to classify a text document based on the terms it contains. This technique is often used to perform *sentiment analysis*, in which a document is classified as positive or negative.
- Creating *vectorized* models that capture semantic relationships between words by assigning them to locations in n-dimensional space. This modeling technique might, for example, assign values to the words "flower" and "plant" that locate them close to one another, while "skateboard" might be given a value that positions it much further away.

While these techniques can be used to great effect, programming them can be complex. In Microsoft Azure, the **Language** cognitive service can help simplify application development by using pre-trained models that can:

- Determine the language of a document or text (for example, French or English).
- Perform sentiment analysis on text to determine a positive or negative sentiment.
- Extract key phrases from text that might indicate its main talking points.
- Identify and categorize entities in the text. Entities can be people, places, organizations, or even everyday items such as dates, times, quantities, and so on.

In this module, you'll explore some of these capabilities and gain an understanding of how you might apply them to applications such as:

- A social media feed analyzer to detect sentiment around a political campaign or a product in market.
- A document search application that extracts key phrases to help summarize the main subject matter of documents in a catalog.
- A tool to extract brand information or company names from documents or other text for identification purposes.

These examples are just a small sample of the many areas that the Language service can help with text analytics.

# **Get started with text analysis**

Completed100 XP

- 10 minutes

The Language service is a part of the Azure Cognitive Services offerings that can perform advanced natural language processing over raw text.

## **Azure resources for the Language service**

To use the Language service in an application, you must provision an appropriate resource in your Azure subscription. You can choose to provision either of the following types of resource:

- A **Language** resource - choose this resource type if you only plan to use natural language processing services, or if you want to manage access and billing for the resource separately from other services.
- A **Cognitive Services** resource - choose this resource type if you plan to use the Language service in combination with other cognitive services, and you want to manage access and billing for these services together.

## **Language detection**

Use the language detection capability of the Language service to identify the language in which text is written. You can submit multiple documents at a time for analysis. For each document submitted to it, the service will detect:

- The language name (for example "English").
- The ISO 6391 language code (for example, "en").
- A score indicating a level of confidence in the language detection.

For example, consider a scenario where you own and operate a restaurant where customers can complete surveys and provide feedback on the food, the service, staff, and so on. Suppose you have received the following reviews from customers:

> Review 1: "A fantastic place for lunch. The soup was delicious."
> 
> 
> **Review 2**: "*Comida maravillosa y gran servicio.*"
> 
> **Review 3**: "*The croque monsieur avec frites was terrific. Bon appetit!*"
> 

You can use the text analytics capabilities in the Language service to detect the language for each of these reviews; and it might respond with the following results:

| Document | Language Name | ISO 6391 Code | Score |
| --- | --- | --- | --- |
| Review 1 | English | en | 1 |
| Review 2 | Spanish | es | 1 |
| Review 3 | English | en | 0.9 |

Notice that the language detected for review 3 is English, despite the text containing a mix of English and French. The language detection service will focus on the ***predominant*** language in the text. The service uses an algorithm to determine the predominant language, such as length of phrases or total amount of text for the language compared to other languages in the text. The predominant language will be the value returned, along with the language code. The confidence score may be less than 1 as a result of the mixed language text.

### **Ambiguous or mixed language content**

There may be text that is ambiguous in nature, or that has mixed language content. These situations can present a challenge to the service. An ambiguous content example would be a case where the document contains limited text, or only punctuation. For example, using the service to analyze the text ":-)", results in a value of **unknown** for the language name and the language identifier, and a score of **NaN** (which is used to indicate *not a number*).

## **Sentiment analysis**

The text analytics capabilities in the Language service can evaluate text and return sentiment scores and labels for each sentence. This capability is useful for detecting positive and negative sentiment in social media, customer reviews, discussion forums and more.

Using the pre-built machine learning classification model, the service evaluates the text and returns a sentiment score in the range of 0 to 1, with values closer to 1 being a positive sentiment. Scores that are close to the middle of the range (0.5) are considered neutral or indeterminate.

For example, the following two restaurant reviews could be analyzed for sentiment:

> "We had dinner at this restaurant last night and the first thing I noticed was how courteous the staff was. We were greeted in a friendly manner and taken to our table right away. The table was clean, the chairs were comfortable, and the food was amazing."
> 

and

> "Our dining experience at this restaurant was one of the worst I've ever had. The service was slow, and the food was awful. I'll never eat at this establishment again."
> 

The sentiment score for the first review might be around 0.9, indicating a positive sentiment; while the score for the second review might be closer to 0.1, indicating a negative sentiment.

### **Indeterminate sentiment**

A score of 0.5 might indicate that the sentiment of the text is indeterminate, and could result from text that does not have sufficient context to discern a sentiment or insufficient phrasing. For example, a list of words in a sentence that has no structure, could result in an indeterminate score. Another example where a score may be 0.5 is in the case where the wrong language code was used. A language code (such as "en" for English, or "fr" for French) is used to inform the service which language the text is in. If you pass text in French but tell the service the language code is **en** for English, the service will return a score of precisely 0.5.

## **Key phrase extraction**

Key phrase extraction is the concept of evaluating the text of a document, or documents, and then identifying the main talking points of the document(s). Consider the restaurant scenario discussed previously. Depending on the volume of surveys that you have collected, it can take a long time to read through the reviews. Instead, you can use the key phrase extraction capabilities of the Language service to summarize the main points.

You might receive a review such as:

> "We had dinner here for a birthday celebration and had a fantastic experience. We were greeted by a friendly hostess and taken to our table right away. The ambiance was relaxed, the food was amazing, and service was terrific. If you like great food and attentive service, you should try this place."
> 

Key phrase extraction can provide some context to this review by extracting the following phrases:

- attentive service
- great food
- birthday celebration
- fantastic experience
- table
- friendly hostess
- dinner
- ambiance
- place

Not only can you use sentiment analysis to determine that this review is positive, you can use the key phrases to identify important elements of the review.

## **Entity recognition**

You can provide the Language service with unstructured text and it will return a list of *entities* in the text that it recognizes. The service can also provide links to more information about that entity on the web. An entity is essentially an item of a particular type or a category; and in some cases, subtype, such as those as shown in the following table.

| Type | SubType | Example |
| --- | --- | --- |
| Person |  | "Bill Gates", "John" |
| Location |  | "Paris", "New York" |
| Organization |  | "Microsoft" |
| Quantity | Number | "6" or "six" |
| Quantity | Percentage | "25%" or "fifty percent" |
| Quantity | Ordinal | "1st" or "first" |
| Quantity | Age | "90 day old" or "30 years old" |
| Quantity | Currency | "10.99" |
| Quantity | Dimension | "10 miles", "40 cm" |
| Quantity | Temperature | "45 degrees" |
| DateTime |  | "6:30PM February 4, 2012" |
| DateTime | Date | "May 2nd, 2017" or "05/02/2017" |
| DateTime | Time | "8am" or "8:00" |
| DateTime | DateRange | "May 2nd to May 5th" |
| DateTime | TimeRange | "6pm to 7pm" |
| DateTime | Duration | "1 minute and 45 seconds" |
| DateTime | Set | "every Tuesday" |
| URL |  | "https://www.bing.com" |
| Email |  | "support@microsoft.com" |
| US-based Phone Number |  | "(312) 555-0176" |
| IP Address |  | "10.0.1.125" |

The service also supports *entity linking* to help disambiguate entities by linking to a specific reference. For recognized entities, the service returns a URL for a relevant *Wikipedia* article.

For example, suppose you use the Language service to detect entities in the following restaurant review extract:

> "I ate at the restaurant in Seattle last week."
> 

| Entity | Type | SubType | Wikipedia URL |
| --- | --- | --- | --- |
| Seattle | Location |  | https://en.wikipedia.org/wiki/Seattle |
| last week | DateTime | DateRange | https://www.notion.soundefined |

---

# **Exercise - Analyze text**

Natural Language Processing (NLP) is a branch of artificial intelligence (AI) that deals with written and spoken language. You can use NLP to build solutions that extracting semantic meaning from text or speech, or that formulate meaningful responses in natural language.

Microsoft Azure *Cognitive Services* includes the text analytics capabilities in the *Language* service, which provides some out-of-the-box NLP capabilities, including the identification of key phrases in text, and the classification of text based on sentiment.

For example, suppose the fictional *Margie's Travel* organization encourages customers to submit reviews for hotel stays. You could use the Language service to summarize the reviews by extracting key phrases, determine which reviews are positive and which are negative, or analyze the review text for mentions of known entities such as locations or people.

To test the capabilities of the Language service, we'll use a simple command-line application that runs in the Cloud Shell. The same principles and functionality apply in real-world solutions, such as web sites or phone apps.

## **Create a *Cognitive Services* resource**

You can use the Language service by creating either a **Language** resource or a **Cognitive Services** resource.

If you haven't already done so, create a **Cognitive Services** resource in your Azure subscription.

1. In another browser tab, open the Azure portal at [https://portal.azure.com](https://portal.azure.com/), signing in with your Microsoft account.
2. Select the **＋Create a resource** button, search for *Cognitive Services*, and create a **Cognitive Services** resource with the following settings:
    - **Subscription**: *Your Azure subscription*.
    - **Resource group**: *Select or create a resource group with a unique name*.
    - **Region**: *Choose any available region*:
    - **Name**: *Enter a unique name*.
    - **Pricing tier**: S0
    - **I confirm I have read and understood the notices**: Selected.
3. Review and create the resource.

### **Get the Key and Location for your Cognitive Services resource**

1. Wait for deployment to complete. Then go to your Cognitive Services resource, and on the **Overview** page, select the link to manage the keys for the service. You will need the endpoint and keys to connect to your Cognitive Services resource from client applications.
2. View the **Keys and Endpoint** page for your resource. You will need the **location/region** and **key** to connect from client applications.

## **Run Cloud Shell**

To test the text analytics capabilities of the Language service, we'll use a simple command-line application that runs in the Cloud Shell on Azure.

1. In the Azure portal, select the **[>_]** (*Cloud Shell*) button at the top of the page to the right of the search box. This opens a Cloud Shell pane at the bottom of the portal.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-text-with-text-analytics-service/media/powershell-portal-guide-1.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-text-with-text-analytics-service/media/powershell-portal-guide-1.png)
    
2. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). Select **PowerShell**. If you do not see this option, skip the step.
3. If you are prompted to create storage for your Cloud Shell, ensure your subscription is specified and select **Create storage**. Then wait a minute or so for the storage to be created.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-text-with-text-analytics-service/media/powershell-portal-guide-2.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-text-with-text-analytics-service/media/powershell-portal-guide-2.png)
    
4. Make sure the type of shell indicated on the top left of the Cloud Shell pane is switched to *PowerShell*. If it is *Bash*, switch to *PowerShell* by using the drop-down menu.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-text-with-text-analytics-service/media/powershell-portal-guide-3.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-text-with-text-analytics-service/media/powershell-portal-guide-3.png)
    
5. Wait for PowerShell to start. You should see the following screen in the Azure portal:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-text-with-text-analytics-service/media/powershell-prompt.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-text-with-text-analytics-service/media/powershell-prompt.png)
    

## **Configure and run a client application**

Now that you have a custom model, you can run a simple client application that uses the Language service.

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
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-text-with-text-analytics-service/media/powershell-portal-guide-4.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-text-with-text-analytics-service/media/powershell-portal-guide-4.png)
    
3. In the **Files** pane on the left, expand **ai-900** and select **analyze-text.ps1**. This file contains some code that uses the Language service:
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-text-with-text-analytics-service/media/analyze-text-code.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-text-with-text-analytics-service/media/analyze-text-code.png)
    
4. Don't worry too much about the details of the code. In the Azure portal, navigate to your Cognitive Services resource. Then select the **Keys and Endpoints** page on the left hand pane. Copy the key and endpoint from the page and paste them into the code editor, replacing the **YOUR_KEY** and **YOUR_ENDPOINT** placeholder values respectively.
    
    **Tip**
    
    You may need to use the separator bar to adjust the screen area as you work with the **Keys and Endpoint** and **Editor** panes.
    
    ![https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-text-with-text-analytics-service/media/key-endpoint-support.png](https://docs.microsoft.com/en-us/learn/wwl-data-ai/analyze-text-with-text-analytics-service/media/key-endpoint-support.png)
    
    After replacing the key and endpoint values, the first lines of code should look similar to this:
    
    PowerShellCopy
    
    ```
    $key="1a2b3c4d5e6f7g8h9i0j...."
    $endpoint="https..."
    
    ```
    
5. At the top right of the editor pane, use the **...** button to open the menu and select **Save** to save your changes. Then open the menu again and select **Close Editor**.
    
    The sample client application will use Cognitive Services' Language service to detect language, extract key phrases, determine sentiment, and extract known entities for reviews.
    
6. In the Cloud Shell, enter the following command to run the code:
    
    Copy
    
    ```
    cd ai-900
    ./analyze-text.ps1 review1.txt
    
    ```
    
    You will be reviewing this text:
    
    > Good Hotel and staff The Royal Hotel, London, UK 3/2/2018 Clean rooms, good service, great location near Buckingham Palace and Westminster Abbey, and so on. We thoroughly enjoyed our stay. The courtyard is very peaceful and we went to a restaurant which is part of the same group and is Indian ( West coast so plenty of fish) with a Michelin Star. We had the taster menu which was fabulous. The rooms were very well appointed with a kitchen, lounge, bedroom and enormous bathroom. Thoroughly recommended.
    > 
7. Review the output.
8. In the PowerShell pane, enter the following command to run the code:
    
    Copy
    
    ```
    ./analyze-text.ps1 review2.txt
    
    ```
    
    You will be reviewing this text:
    
    > Tired hotel with poor service The Royal Hotel, London, United Kingdom 5/6/2018 This is a old hotel (has been around since 1950's) and the room furnishings are average - becoming a bit old now and require changing. The internet didn't work and had to come to one of their office rooms to check in for my flight home. The website says it's close to the British Museum, but it's too far to walk.
    > 
9. Review the output.
10. In the PowerShell pane, enter the following command to run the code:
    
    Copy
    
    ```
    ./analyze-text.ps1 review3.txt
    
    ```
    
    You will be reviewing this text:
    
    > Good location and helpful staff, but on a busy road. The Lombard Hotel, San Francisco, USA 8/16/2018 We stayed here in August after reading reviews. We were very pleased with location, just behind Chestnut Street, a cosmopolitan and trendy area with plenty of restaurants to choose from. The Marina district was lovely to wander through, very interesting houses. Make sure to walk to the San Francisco Museum of Fine Arts and the Marina to get a good view of Golden Gate bridge and the city. On a bus route and easy to get into centre. Rooms were clean with plenty of room and staff were friendly and helpful. The only down side was the noise from Lombard Street so ask to have a room furthest away from traffic noise.
    > 
11. Review the output.
12. In the PowerShell pane, enter the following command to run the code:
    
    Copy
    
    ```
    ./analyze-text.ps1 review4.txt
    
    ```
    
    You will be reviewing this text:
    
    > Very noisy and rooms are tiny The Lombard Hotel, San Francisco, USA 9/5/2018 Hotel is located on Lombard street which is a very busy SIX lane street directly off the Golden Gate Bridge. Traffic from early morning until late at night especially on weekends. Noise would not be so bad if rooms were better insulated but they are not. Had to put cotton balls in my ears to be able to sleep--was too tired to enjoy the city the next day. Rooms are TINY. I picked the room because it had two queen size beds--but the room barely had space to fit them. With family of four in the room it was tight. With all that said, rooms are clean and they've made an effort to update them. The hotel is in Marina district with lots of good places to eat, within walking distance to Presidio. May be good hotel for young stay-up-late adults on a budget
    > 
13. Review the output.

## **Learn more**

This simple app shows only some of the capabilities of the Language service. To learn more about what you can do with this service, see the [Language service page](https://azure.microsoft.com/services/cognitive-services/language-service/).