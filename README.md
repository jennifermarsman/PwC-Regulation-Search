# PwC-Regulation-Search

Azure Cognitive Search service which leverages machine learning models from PwC to identify actionable regulations in documents

## Overview

PwC is a network of business consultancy and accounting firms in 150 countries, making it the largest professional services organization in the world, according to the Reference for Business.  Regulated companies like PwC's Fortune 100 clients can invest a lot of time and effort to identify and understand the regulations that they must meet.  To help its clients comply more easily, PwC built a Regulatory Obligation Identifier (ROI) classifier, which identifies if given text contains an actionable regulation.  

Cognitive Search allows developers to compose a set of skills (both prebuilt functionality that can be added with a simple checkbox, like language detection, translation, sentiment analysis, key phrase extraction, entity extraction, OCR, image captioning, etc. as well as extensibility for your own custom machine learning skills like PwC's ROI model).  So an end user could use these together beautifully by simply dumping regulatory documents or pdfs in Azure blob storage, then Cognitive Search could perform language detection and translate into English, run through PwC's ROI model to find the actionable regulations, as well as anything else they might want to do (extract entities like organization names, locations, etc.) and have all of this extra intelligence from their documents stored and queryable.  

For more information, see <https://aka.ms/PwCCognitiveSearchStory>.  

![Architecture Diagram](images\CC0946_MS_PwC_ArchitectureDiagram_v2.02_051120.png)

## Setup

First, you will need an Azure account.  If you don't already have one, you can start a free trial of Azure [here](https://azure.microsoft.com/free/).  

Secondly, this implementation uses the PwC machine learning model to identify actionable regulations in documents.  If you are interested in leveraging this model, please contact TODO.  Their model is hosted in an Azure Function and can be called as a custom skill.  

Now, on the Azure portal, you can create a new Azure storage account at <https://portal.azure.com/#create/Microsoft.StorageAccount>. Remember the subscription, resource group, and location that you use, and use the same ones for the Azure Cognitive Search service and Cognitive Services key when you create them.  Choose your own unique storage account name (it must be lowercase letters and numbers only).  You can change the replication to LRS.  You can use the defaults for everything else, and then create the storage.  Once it has been deployed, update the blob_connection_string variable in the SetupAzureCognitiveSearchService.ipynb notebook.  (This setup notebook will create a container in your blob storage called "pwc" and upload sample data there, but don't run it yet.)  Finally, create a container in your blob storage account called "knowledgeStore".  After you have created it, note down the connection string to that container for later.  

Before running the notebook, you will also need to update the "TODO" placeholders in the skillset.json.  Open skillset.json, search for "TODO", and replace each instance with the following:

1. **PwcRegulationSkill URI:** this value should be provided by PwC, in the format "https://some-function-name.azurewebsites.net/api/classify?code=NumbersAndLetters=="
2. **Cognitive Services key:** create a new Cognitive Services key in the [Azure portal](https://portal.azure.com/#create/Microsoft.CognitiveServicesAllInOne) using the same subscription, location, and resource group that you did for your Azure storage account.  Click "Create" and after the resource is ready, click it.  Click "Keys and Endpoint" in the left-hand sidebar.  Copy the Key 1 value into this TODO placeholder.  
3. **Knowledge Store connection string:** use the value that you noted down earlier of the connection string to the knowledgeStore container in your Azure blob storage.  It should be of the format "DefaultEndpointsProtocol=https;AccountName=YourValueHere;AccountKey=YourValueHere;EndpointSuffix=core.windows.net".  

Finally, you can open the Jupyter notebook SetupAzureCognitiveSearchService.ipynb, follow the instructions to modify the values in the first code cell, and then run all of the cells.  This notebook will walk you through [creating an Azure Cognitive Search service](https://portal.azure.com/#create/Microsoft.Search) and calling REST endpoints on the search service to set up the data source, index, skillset, and indexer.
