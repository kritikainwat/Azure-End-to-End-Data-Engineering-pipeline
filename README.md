# Azure-End-to-End-Data-Engineering-pipeline

This project builds an End-to-End Azure Data Engineering Solution. A Pipeline performing Data Ingestion, ETL and Analytics all-in-one solution using Microsoft Azure Services and Power BI.

# Goal of the Project

The goal is to create an Azure solution which can take an On-premise Database such as the Microsoft SQL Server Management System (SSMS) and move it to the Cloud. It does so by building an ETL pipeline using Azure Data Factory, Azure Databricks and Azure Synapse Analytics.

This solution can be connected to a visualization and reporting dashboard using Microsoft Power 

BI.<img width="1280" height="556" alt="Arc img 2" src="https://github.com/user-attachments/assets/faac9ec4-3dcd-44c1-981d-20124b7ba7d7" />

Data Migration to the Cloud is one of the most common scenarios the Data Engineers encounter when building solutions for a small-medium organization. By working on this project, I was able to learn these skills:

- Data Ingestion
  
- ETL techniques using Azure Cloud Services
  
- Data Transformation
  
- Data Analytics and Dashboard Reporting
  
- Data Security and Governance

 # Prerequisites:
 
1 Microsoft SQL Server Managment System (SSMS)

2 Azure Subscription (Azure Data Lake Storage Gen2, Azure Data Factory, Azure Key Vault, Azure Databricks, Azure Synapse Analytics, Microsoft Entra ID)
Microsoft Power BI

3 Set up "AdventureWorksLT2017" Database with credentials 'usr1'. Set up the same credentials as Secrets in Azure Key Vault

4 The Database used for this project demonstration is: AdventureWorksLT2017 Sales Database []

# Implementation:

Part 1: Data Ingestion

1 Restore the Sales Database from the .bak file.


<img width="718" height="523" alt="SOURCE 2017LTv1" src="https://github.com/user-attachments/assets/7f3543fd-7b06-4059-8866-28c40adbcbd2" />


2 Setup the Microsoft Integration Runtime between Azure and the On-premise SQL Server.

3 Create a Copy Pipeline which loads the data from local on-premise server into Azure Data Lake Storage Gen2 "bronze" directory.

Note that the Data is stored in "Parquet format" in ADLS Gen2 storage folders.

<img width="759" height="366" alt="299250973-d2126d21-6f67-4fd1-bfa8-0902c5182ddc" src="https://github.com/user-attachments/assets/3ec6933d-e986-4cc9-9671-909d62a170aa" />

Part 2: Data Transformation

Data is Loaded into Azure Databricks where can create PySpark Notebooks. Cluster nodes, and compute automatically managed by the Databricks service. The Initial Data is cleaned and processed in two steps. Bronze to Silver and Silver to Gold. 0. Mounting the ADLS

1 In Bronze to Silver transformation, we apply Attribute Type Changes and move this preprocessed data from Bronze to Silver folders.

2 In Silver to Gold transformation, we rename the Attributes to follow similar Naming Convention throughout the database. Then we move this into Gold folder.

The Final Gold-level Data is suitable for business reporting and making dashboard visualizations. Gold-level data is in "Delta" format.

<img width="976" height="551" alt="Storage Mount 0" src="https://github.com/user-attachments/assets/2837b8a7-1836-491c-baf9-27bd1d15f29e" />
<img width="946" height="540" alt="Storage Mount 1" src="https://github.com/user-attachments/assets/3b01f662-2e53-4d78-b20b-f6c3e659664d" />


<img width="938" height="504" alt="299252885-cff35231-e9d0-4a82-b857-dfcc2845c7cb" src="https://github.com/user-attachments/assets/6e25ebbb-a9cc-482b-bedd-7262c0a69bae" />


Launch Azure Databricks and run transformations using these notebooks "bronze to silver" and "silver to gold".

<img width="926" height="542" alt="299253166-782503d8-453b-4bc4-8b24-5a2417bff378" src="https://github.com/user-attachments/assets/c84713ec-47d3-4fc8-8e2b-7bff21aaa09e" />

These Notebooks are integrated into the Azure Data Factory Pipeline. Thus automating the Data Ingestion and Transformation process.

 <img width="940" height="250" alt="299458142-32a352fe-7a0b-498b-91ee-f4284a19a24a" src="https://github.com/user-attachments/assets/421c58ca-bc58-4b34-b15e-5dc9fd666c10" />

Part 3: Data Loading

- Load the "gold" level data and run the Azure Synapse Pipeline. This pipeline:

- Retrieves the Table Names from the gold folder.

For each table, A Stored Procedure is executed which creates and updates View in Azure SQL Database..
<img width="466" height="245" alt="299255821-7f935213-4dd9-471a-aa24-bc4b1c68f41b" src="https://github.com/user-attachments/assets/07c4568b-d79a-4b1c-a9fd-13100ac61991" />

Part 4: Data Reporting

Finally, load the data from the views using Microsoft Power BI. The Data is retrieved using DirectQuery to automatically run and update from the Cloud Pipelines.

An Interactive Dashboard is created to showcase the sales data figures.

<img width="959" height="500" alt="PowerBI Reporting output" src="https://github.com/user-attachments/assets/db8ca4d8-164f-4da7-b189-4af4f0d4e8eb" />

<img width="852" height="494" alt="299257279-aabd6309-ef85-4ed9-af1a-171dbd7c2505" src="https://github.com/user-attachments/assets/a7f1b861-f69a-4765-9a39-3c7ec960a144" />

Part 5: End-to-end Pipeline Testing

Once all the components are ready, we can create a Scheduled Trigger, which will allow the Data Factory Pipeline to be run once every day.

<img width="870" height="889" alt="299260487-d28f9c77-0027-4bb5-96f4-104109346f82" src="https://github.com/user-attachments/assets/f9586d46-b108-4c8a-8e09-1e7f5356a26b" />

Using this trigger makes it easy to automatically extract, load , transform the latest data. This data can be refreshed in Power BI from time to time.

- Before running the trigger:

- <img width="508" height="277" alt="299257583-51405c5f-331a-4bbd-83cf-439f91ca2525" src="https://github.com/user-attachments/assets/2af745fb-3977-43a2-a268-7862a2f1cbd6" />

- After running the trigger:

- 
<img width="369" height="236" alt="299258435-578aca35-89b1-4a31-b1e0-27fb7fd923ed" src="https://github.com/user-attachments/assets/6f456d3b-9559-4095-a655-47848259ca6f" />

End Notes

This project provides a great overview to many of Azure services such as Azure Data Factory, Azure Databricks, Azure Synapse Analytics.

- The resources we create in Azure can be secured by adding contributors into a Security Group. This feature is offered in Microsoft Entra ID (previously Azure Active Directory). Thus, whoever belonging to the Security group can access and contribute to the project freely.

- The Database is although small, made it easier to visualize the scope and working of various services at once.

- Another thing to note is that, project couldve been made smaller using only Azure Data factory but I added Databricks and Synapse Analytics to explore what these have to offer.

👤 Author

Kritika Inwati

Aspiring Data Analyst
