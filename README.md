# Modified Content Localization on AWS 

Welcome to the Modified Content Localization on AWS project! This project will help you extend the reach of your VOD content by quickly and efficiently creating accurate multi-language subtitles using AWS AI Services. You can make manual corrections to the automatically created subtitles and use advanced AWS AI Service customization features to improve the results of the automation for your content domain. Content Localization is built on Modified Media Insights Engine (MIE), a framework that helps accelerate the development of serverless applications that process video, images, audio, and text with artificial intelligence services and multimedia services on AWS.

Enhanced Language Support with OpenAI's Whisper: <br>
The modified version of the Content Localization on AWS project has also integrated OpenAI's Whisper for Arabic and Turkish models, providing exceptional transcription capabilities for these languages. This integration ensures a higher level of accuracy and efficiency in transcribing Arabic and Turkish audio content, which is essential for creating quality subtitles in these languages.

Streamlined Workflow with Subtitle File Option: <br>
Another significant improvement to the original AWS solution is enabling the workflow to be run based on a subtitle file only. This modification greatly streamlines the process of creating multi-language subtitles, resulting in a significant reduction of both time and cost by up to 70% from the initial solution if you intend to run the translation only (in the previous solution you were forced to have the video as well).
<br>
![Architecture Overview](doc/images/ContentLocalizationArchitectureOverview.png)
Localization is the process of taking video content that was created for audiences in one geography and transforming it to make it relevant and accessible to audiences in a new geography.  Creating alternative language subtitle tracks is central to the localization process.  This application presents a guided experience for automatically generating and correcting subtitles for videos in multiple languages using AWS AI Services.  The corrections made by editors can be used to customize the results of AWS AI services for future workflows.  This type of AI/ML workflow, which incorporates user corrections is often referred to as “human in the loop”.

Content Localization workflows can make use of advanced customization features provided by Amazon Transcribe and Amazon Translate:

* [Amazon Transcribe Custom Vocabulary](https://docs.aws.amazon.com/transcribe/latest/dg/how-vocabulary.html) - you provide Amazon Transcribe with a list of terms that are specific to your content and how you want the terms to be displayed in transcipts
* [Amazon Transcribe Custom Language Models](https://docs.aws.amazon.com/transcribe/latest/dg/custom-language-models.html) - you provide Amazon Transcribe with domain-specific text data that is representative of the audio you want to transcribe. 
* [Amazon Translate Custom Terminologies](https://docs.aws.amazon.com/translate/latest/dg/how-custom-terminology.html) - you provide Amazon Translate with a list of terms or phrases in the source language content and specify how you want them to appear in the translated result.
* [Amazon Translate Parallel Data for Active Custom Translation](https://docs.aws.amazon.com/translate/latest/dg/customizing-translations-parallel-data.html) - you provide Amazon Translate with a list of parallel phrases: the source language and the pharase tranlated the way you want it.  The Parallel Data customizes Amazon Translate models so they create more contectual translations based on the sample you provide.

Application users can manually correct the results of the automation at different points in the automated workflow and then trigger a new workflow to inclue their corrections in downstream processing.  Corrections are tracked and can be used to update Amazon Transcribe Custom Vocabularies and Amazon Translate Custom Terminologies to improve future results.  
<br>


### Why use customizations and human-in-the-loop?

Automating the creation of translated subtitles using AI/ML promises to speed up the process of localization for your content, but there are still challenges to acheive the level of accuracy that is required for specific use cases.  With natural language processing, many aspects of the content itself may determine the level of accuracy AI/ML analysis is capable of achieving.  Some content characteristics that can impact transcription and translation accuracy include: domain specific language, speaker accents and dialects, new words recently introduced to common language, the need for contextual interpretation of ambiguous phases, and correct translation of proper names.   AWS AI services provide a variety of features to help customize the results of the machine learning to specific content.   Therefore, the workflow in this application seeks to provide users with a guided experience to use these customization features as an extension of their normal editing workflow.

### Doesn’t content localization involve more than just subtitles?

As a first step, this project seeks to create an efficient, customizable workflow for creating multi-language subtitles.  We hope that this project will grow to apply AWS more AI Services to help automate other parts of the localization process.  For this reason, the application workflow includes the options to generate other useful types of analysis available in AWS AI Services.  While this analysis is not performed in the base workflow, developers can enable it to explore the available data to help with extending the application.  Here are some ideas to inspire the builders out there to extend this application:

* Identifying names of people within the collection and auto-suggesting or correcting them in subtitiles.  People may be identified using voice recognition with Amazon Transcribe, or with Amazon Rekognition Celebrity Recognition, or Face  Search.
* Identify text or phrases in the visual content that may need to be translated.
* Use shot boundary and location of visual artifacts withing the content to help with placement of subtitles within relative to the content.

# Deployment

The following Cloudformation templates will deploy the Content Localization front-end application with a prebuilt version of the most recent MIE release.

| Region            | Launch                                                                                                                                                                                                                     |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| EU West (Ireland) | [![Launch in eu-west-1](doc/images/launch-stack.png)](https://zero-and-one-solutions-eu-west-1.s3.eu-west-1.amazonaws.com/content-localization-on-aws-modified/content-localization/v1.0/content-localization-on-aws.yaml) |

# Screenshots

Translation analysis: <br>
![Translation Analysis](doc/images/ig-view-subtitles.png) <br>

Workflow configuration: <br>
![Workflow configuration](doc/images/ig-upload-configure.png) <br><br>

# COST

You are responsible for the cost of the AWS services used while running this application. The primary cost factors are from using Amazon Rekognition, Amazon Transcribe, Amazon Translate, Amazon Comprehend, Amazon Polly and Amazon OpenSearch Service (successor to Amazon Elasticsearch Service). With all services enabled, Videos cost about $0.50 per minute to process, but can vary between $0.10 per minute and $0.60 per minute depending on the video content and the types of analysis enabled in the application.  The default workflow for Content Localization only enables Amazon Transcribe, Amazon Translate, Amazon Comprehend, and Amazon Polly.   Data storage and Amazon ES will cost approximately ***$10.00 per day*** regardless of the quantity or type of video content.

After a video is uploaded into the solution, the costs for processing are a one-time expense. However, data storage costs occur daily.

For more information about cost, see the pricing webpage for each AWS service you will be using in this solution. If you need to process a large volume of videos, we recommend that you contact your AWS account representative for at-scale pricing. 

# Subtitle workflow

After uploading a video or image in the GUI, the application runs a workflow in Media Insights on AWS that extracts insights using a variety of media analysis services on AWS and stores them in a search engine for easy exploration. The following flow diagram illustrates this workflow:

[Image: Workflow.png]
This application includes the following features:


* Proxy encode of videos and separation of video and audio tracks using **AWS Elemental MediaConvert**. 
* Convert speech to text from audio and video assets using **Amazon Transcribe**.
* Convert Transcribe transcripts to subtitles
* Convert subtitles from one language to another using **Amazon Translate**.
* Generate a voice audio track for translations using **Amazon Polly**

Users can enable or disable operators in the upload view shown below:

![operators categories](doc/images/ig-operator-categories.png)


# Search Capabilities:

The search field in the Collection view provides the ability to find media assets that contain specified metadata terms. Search queries are executed by Amazon OpenSearch, which uses full-text search techniques to examine all the words in every metadata document in its database. Everything you see in the analysis page is searchable. Even data that is excluded by the threshold you set in the Confidence slider is searchable. Search queries must use valid Lucene syntax.

Here are some sample searches:

* Search for filenames containing the suffix ".mp4" with, `*.mp4` or `filename:*.mp4`
* Since Content Moderation returns a "Violence" label when it detects violence in a video, you can search for any video containing violence simply with: `Violence`
* Search for videos containing violence with a 80% confidence threshold: `Violence AND Confidence:>80` 
* The previous queries may match videos whose transcript contains the word "Violence". You can restrict your search to only Content Moderation results, like this: `Operator:content_moderation AND (Name:Violence AND Confidence:>80)`
* To search for Violence results in Content Moderation and guns or weapons identified by Label Detection, try this: `(Operator:content_moderation AND Name:Violence AND Confidence:>80) OR (Operator:label_detection AND (Name:Gun OR Name:Weapon))`  
* You can search for phrases in Comprehend results like this, `PhraseText:"some deep water" AND Confidence:>80`
* To see the full set of attributes that you can search for, click the Analytics menu item and search for "*" in the Discover tab of Kibana.