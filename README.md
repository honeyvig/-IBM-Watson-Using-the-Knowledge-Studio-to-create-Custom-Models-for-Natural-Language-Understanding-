# -IBM-Watson-Using-the-Knowledge-Studio-to-create-Custom-Models-for-Natural-Language-Understanding
IBM Watson Knowledge Studio is a powerful tool that allows you to create custom natural language understanding (NLU) models tailored to your specific needs. By using IBM Watson Knowledge Studio, you can train custom models to identify and extract entities, classify intents, and understand the relationships between various parts of text.

The typical workflow involves:

    Creating and labeling your training data in Watson Knowledge Studio.
    Training and deploying the model in Watson NLU.
    Integrating the model with your application (via the Watson NLU API).

To demonstrate how you can use IBM Watson Knowledge Studio and Watson NLU to create and use custom models for NLU tasks, follow the steps below.
Step 1: Create and Label Custom Models in IBM Watson Knowledge Studio

    Create an Account on IBM Cloud:
        Sign up for an IBM Cloud account if you don’t already have one: IBM Cloud.

    Set Up Watson Knowledge Studio:
        Go to the Watson Knowledge Studio page and create a new project.
        Select the Natural Language Understanding (NLU) template when prompted.

    Label Your Data:
        Upload your training data (in .txt, .csv, etc.) containing example sentences or documents.
        Start annotating your data:
            Entities: Label specific pieces of information like names, dates, and locations (e.g., "IBM" as an organization or "2025-01-01" as a date).
            Intents: Label the overall intent or purpose of the sentence (e.g., "Book a flight").
            Relationships: Annotate relationships between entities (e.g., "Person A" is flying to "City B").

    Train the Model:
        Once you've labeled the data, click on Train to create the custom NLU model.
        Watson Knowledge Studio will automatically build and evaluate the model using the labeled data.

    Deploy the Model:
        Once the model is trained, deploy it to Watson NLU for use.

Step 2: Using the Custom Model with IBM Watson NLU

Once your custom model is created and deployed using Watson Knowledge Studio, you can use the IBM Watson Natural Language Understanding (NLU) API to analyze text and extract insights based on your custom model.

Here's an example of how to integrate and use the custom NLU model with Python:
Step 2.1: Install IBM Watson SDK

If you haven't installed the IBM Watson SDK for Python, use the following command:

pip install ibm-watson

Step 2.2: Python Code to Use the Custom NLU Model

import json
from ibm_watson import NaturalLanguageUnderstandingV1
from ibm_watson.natural_language_understanding_v1 import Features, Entities, Keywords
from ibm_cloud_sdk_core.authenticators import IAMAuthenticator

# Replace with your own IBM Watson NLU credentials
api_key = 'YOUR_API_KEY'  # Replace with your NLU API key
url = 'YOUR_URL'  # Replace with your NLU service URL

# Initialize the Watson NLU client
authenticator = IAMAuthenticator(api_key)
nlu = NaturalLanguageUnderstandingV1(
    version='2021-08-01',  # Use the latest version available
    authenticator=authenticator
)

nlu.set_service_url(url)

# Sample text to analyze
text = "IBM is a global technology company founded in 1911. It operates in over 170 countries."

# Function to analyze text using the custom NLU model
def analyze_text_with_custom_model(text):
    # Call the NLU API with your custom model
    response = nlu.analyze(
        text=text,
        features=Features(
            entities=Entities(model='YOUR_CUSTOM_MODEL_ID', # Replace with your model ID
                              sentiment=True),
            keywords=Keywords()
        )
    ).get_result()

    # Print out the response from Watson NLU
    print(json.dumps(response, indent=2))

# Run the analysis on the sample text
analyze_text_with_custom_model(text)

Step 2.3: Explanation of the Code

    IAMAuthenticator: This authenticates the API using the API key for Watson NLU.
    NaturalLanguageUnderstandingV1: This is the main class used to interact with the Watson NLU API.
    Features and Entities: The Features parameter allows you to specify the types of analysis to perform, such as Entities, Keywords, or even custom models you've trained in Watson Knowledge Studio.
    Custom Model: In the Entities feature, you can specify your custom model by providing its ID (replace 'YOUR_CUSTOM_MODEL_ID' with the actual model ID).
    Text: The input text is passed to Watson NLU for analysis.

Step 3: Analyze Custom Data

Once you have your custom NLU model deployed and integrated into the script, you can start analyzing various kinds of text and extract the insights you need (e.g., detecting entities, classifying intents, and identifying relationships). Here's an example of a response you might get:
Example Response

{
  "language": "en",
  "entities": [
    {
      "type": "Company",
      "text": "IBM",
      "disambiguation": {
        "subtype": ["Organization"]
      },
      "count": 1,
      "sentiment": {
        "score": 0.5
      },
      "confidence": 0.98
    },
    {
      "type": "Date",
      "text": "1911",
      "disambiguation": {
        "subtype": ["Date"]
      },
      "count": 1,
      "sentiment": {
        "score": 0.2
      },
      "confidence": 0.95
    }
  ],
  "keywords": [
    {
      "text": "technology",
      "relevance": 0.9
    },
    {
      "text": "global",
      "relevance": 0.8
    }
  ]
}

In this case, the entities detected are "IBM" (Company) and "1911" (Date), along with sentiment and confidence scores. The keywords detected are "technology" and "global" with their relevance scores.
Step 4: Handling the Response

You can handle the response from Watson NLU based on your application's needs. For example, you could:

    Extract and display the entities or keywords to the user.
    Use sentiment analysis to understand the tone of a text.
    Integrate this data into a larger system (e.g., for chatbots or document processing).

Conclusion

By following these steps, you can effectively use IBM Watson Knowledge Studio to create custom models that enhance your Natural Language Understanding capabilities. You can create models for specific tasks (like extracting product names, dates, etc.) and then use them in a Python application or any other integration that supports REST APIs. Once deployed, you can easily use the Watson NLU API to analyze text and retrieve valuable insights tailored to your business needs.
