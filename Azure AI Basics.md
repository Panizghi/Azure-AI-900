# Azure AI Basics

## **What is AI?**

Simply put, AI is the creation of software that imitates human behaviors and capabilities. Key workloads include:

- **Machine learning** - This is often the foundation for an AI system, and is the way we "teach" a computer model to make prediction and draw conclusions from data.
- **Anomaly detection** - The capability to automatically detect errors or unusual activity in a system.
- **Computer vision** - The capability of software to interpret the world visually through cameras, video, and images.
- **Natural language processing** - The capability for a computer to interpret written or spoken language, and respond in kind.
- **Knowledge mining** - The capability to extract information from large volumes of often unstructured data to create a searchable knowledge store.

## **How machine learning works**

So how do machines learn?

The answer is, from data. In today's world, we create huge volumes of data as we go about our everyday lives. From the text messages, emails, and social media posts we send to the photographs and videos we take on our phones, we generate massive amounts of information. More data still is created by millions of sensors in our homes, cars, cities, public transport infrastructure, and factories.

Data scientists can use all of that data to train machine learning models that can make predictions and inferences based on the relationships they find in the data.

For example, suppose an environmental conservation organization wants volunteers to identify and catalog different species of wildflower using a phone app. The following animation shows how machine learning can be used to enable this scenario.

![machine-learn.gif](Azure%20AI%20Basics%202a78d947b9c54d8d89fc9388d279927b/machine-learn.gif)

1. A team of botanists and scientists collect data on wildflower samples.
2. The team labels the samples with the correct species.
3. The labeled data is processed using an algorithm that finds relationships between the features of the samples and the labeled species.
4. The results of the algorithm are encapsulated in a model.
5. When new samples are found by volunteers, the model can identify the correct species label.

## **Machine learning in Microsoft Azure**

Microsoft Azure provides the **Azure Machine Learning** service - a cloud-based platform for creating, managing, and publishing machine learning models. Azure Machine Learning provides the following features and capabilities:

![Screen Shot 2022-05-30 at 1.33.18 AM.png](Azure%20AI%20Basics%202a78d947b9c54d8d89fc9388d279927b/Screen_Shot_2022-05-30_at_1.33.18_AM.png)

# **Understand anomaly detection**

Imagine you're creating a software system to monitor credit card transactions and detect unusual usage patterns that might indicate fraud. Or an application that tracks activity in an automated production line and identifies failures. Or a racing car telemetry system that uses sensors to proactively warn engineers about potential mechanical failures before they happen.

These kinds of scenario can be addressed by using *anomaly detection* - a machine learning based technique that analyzes data over time and identifies unusual changes.

Let's explore how anomaly detection might help in the racing car scenario.

1. Sensors in the car collect telemetry, such as engine revolutions, brake temperature, and so on.
2. An anomaly detection model is trained to understand expected fluctuations in the telemetry measurements over time.
3. If a measurement occurs outside of the normal expected range, the model reports an anomaly that can be used to alert the race engineer to call the driver in for a pit stop to fix the issue before it forces retirement from the race.

## **Anomaly detection in Microsoft Azure**

In Microsoft Azure, the **Anomaly Detector** service provides an application programming interface (API) that developers can use to create anomaly detection solutions.

# **Understand computer vision**

Computer Vision is an area of AI that deals with visual processing. Let's explore some of the possibilities that computer vision brings.

The **Seeing AI** app is a great example of the power of computer vision. Designed for the blind and low vision community, the Seeing AI app harnesses the power of AI to open up the visual world and describe nearby people, text and objects.

## **Computer Vision models and capabilities**

Most computer vision solutions are based on machine learning models that can be applied to visual input from cameras, videos, or images. The following table describes common computer vision tasks.

- Image classification : Image classification involves training a machine learning model to classify images based on their contents. For example, in a traffic monitoring solution you might use an image classification model to classify images based on the type of vehicle they contain, such as taxis, buses, cyclists, and so on.

![Untitled](Azure%20AI%20Basics%202a78d947b9c54d8d89fc9388d279927b/Untitled.png)

- Object detection : Object detection machine learning models are trained to classify individual objects within an image, and identify their location with a bounding box. For example, a traffic monitoring solution might use object detection to identify the location of different classes of vehicle.
    
    ![Untitled](Azure%20AI%20Basics%202a78d947b9c54d8d89fc9388d279927b/Untitled%201.png)
    

- Semantic segmentation : Semantic segmentation is an advanced machine learning technique in which individual pixels in the image are classified according to the object to which they belong. For example, a traffic monitoring solution might overlay traffic images with "mask" layers to highlight different vehicles using specific colors.

![Untitled](Azure%20AI%20Basics%202a78d947b9c54d8d89fc9388d279927b/Untitled%202.png)

- Image analysis : You can create solutions that combine machine learning models with advanced image analysis techniques to extract information from images, including "tags" that could help catalog the image or even descriptive captions that summarize the scene shown in the image.

![Untitled](Azure%20AI%20Basics%202a78d947b9c54d8d89fc9388d279927b/Untitled%203.png)

- Face detection, analysis, and recognition : Face detection is a specialized form of object detection that locates human faces in an image. This can be combined with classification and facial geometry analysis techniques to infer details such as age and emotional state; and even recognize individuals based on their facial features.

![Untitled](Azure%20AI%20Basics%202a78d947b9c54d8d89fc9388d279927b/Untitled%204.png)

- Optical character recognition (OCR) : Optical character recognition is a technique used to detect and read text in images. You can use OCR to read text in photographs (for example, road signs or store fronts) or to extract information from scanned documents such as letters, invoices, or forms.

![Untitled](Azure%20AI%20Basics%202a78d947b9c54d8d89fc9388d279927b/Untitled%205.png)

![Screen Shot 2022-05-30 at 1.46.14 AM.png](Azure%20AI%20Basics%202a78d947b9c54d8d89fc9388d279927b/Screen_Shot_2022-05-30_at_1.46.14_AM.png)

# **Understand natural language processing**

Natural language processing (NLP) is the area of AI that deals with creating software that understands written and spoken language.

NLP enables you to create software that can:

- Analyze and interpret text in documents, email messages, and other sources.
- Interpret spoken language, and synthesize speech responses.
- Automatically translate spoken or written phrases between languages.
- Interpret commands and determine appropriate actions.

For example, *Starship Commander*, is a virtual reality (VR) game from Human Interact, that takes place in a science fiction world. The game uses natural language processing to enable players to control the narrative and interact with in-game characters and starship systems.

## **Natural language processing in Microsoft Azure**

In Microsoft Azure, you can use the following cognitive services to build natural language processing solutions:

![Screen Shot 2022-05-30 at 1.57.51 AM.png](Azure%20AI%20Basics%202a78d947b9c54d8d89fc9388d279927b/Screen_Shot_2022-05-30_at_1.57.51_AM.png)

# **Understand knowledge mining**

Knowledge mining is the term used to describe solutions that involve extracting information from large volumes of often unstructured data to create a searchable knowledge store.

## **Knowledge mining in Microsoft Azure**

One of these knowledge mining solutions is **Azure Cognitive Search**, a private, enterprise, search solution that has tools for building indexes. The indexes can then be used for internal only use, or to enable searchable content on public facing internet assets.

Azure Cognitive Search can utilize the built-in AI capabilities of Azure Cognitive Services such as image processing, content extraction, and natural language processing to perform knowledge mining of documents. The product's AI capabilities makes it possible to index previously unsearchable documents and to extract and surface insights from large amounts of data quickly.

---

# **Challenges and risks with AI**

Artificial Intelligence is a powerful tool that can be used to greatly benefit the world. However, like any tool, it must be used responsibly.

The following table shows some of the potential challenges and risks facing an AI application developer.

![Screen Shot 2022-05-30 at 2.00.28 AM.png](Azure%20AI%20Basics%202a78d947b9c54d8d89fc9388d279927b/Screen_Shot_2022-05-30_at_2.00.28_AM.png)

# **Understand responsible AI**

At Microsoft, AI software development is guided by a set of six principles, designed to ensure that AI applications provide amazing solutions to difficult problems without any unintended negative consequences.

## **Fairness**

AI systems should treat all people fairly. For example, suppose you create a machine learning model to support a loan approval application for a bank. The model should make predictions of whether or not the loan should be approved without incorporating any bias based on gender, ethnicity, or other factors that might result in an unfair advantage or disadvantage to specific groups of applicants.

Azure Machine Learning includes the capability to interpret models and quantify the extent to which each feature of the data influences the model's prediction. This capability helps data scientists and developers identify and mitigate bias in the model.

## **Reliability and safety**

AI systems should perform reliably and safely. For example, consider an AI-based software system for an autonomous vehicle; or a machine learning model that diagnoses patient symptoms and recommends prescriptions. Unreliability in these kinds of system can result in substantial risk to human life.

AI-based software application development must be subjected to rigorous testing and deployment management processes to ensure that they work as expected before release.

## **Privacy and security**

AI systems should be secure and respect privacy. The machine learning models on which AI systems are based rely on large volumes of data, which may contain personal details that must be kept private. Even after the models are trained and the system is in production, it uses new data to make predictions or take action that may be subject to privacy or security concerns.

## **Inclusiveness**

AI systems should empower everyone and engage people. AI should bring benefits to all parts of society, regardless of physical ability, gender, sexual orientation, ethnicity, or other factors.

## **Transparency**

AI systems should be understandable. Users should be made fully aware of the purpose of the system, how it works, and what limitations may be expected.

## **Accountability**

People should be accountable for AI systems. Designers and developers of AI-based solutions should work within a framework of governance and organizational principles that ensure the solution meets ethical and legal standards that are clearly defined.

![Screen Shot 2022-05-30 at 2.36.58 AM.png](Azure%20AI%20Basics%202a78d947b9c54d8d89fc9388d279927b/Screen_Shot_2022-05-30_at_2.36.58_AM.png)

##