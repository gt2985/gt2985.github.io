---
title: "Salesforce Org-2-Org Migrations"
categories:
  - Technology
  - Salesforce
tags:
  - Migration
  - Attachments
  - Jitterbit
---

## Salesforce Org Migrations
Recently I have been working on a salesforce org migration. Although I`m migrating from one salesforce instance to another salesforce another. From the sounds of it, it apears to be a simple activity and it certainly is so to an extent. A few things that you will need to think about regarding the migration

### 

Choice of Tools

There are many tools and related documentation easily available, for e.g. salesforce dataloader, dataloader.io, jitterbit dataloader, etc. I ended up using jitterbit data loader for the inclusive ability of carrying out some simple data transformations within the tool itself. It allows one to save the source and target destinations alongwith the credentials. It also enables one to organise the exports and imports. One huge advantage that I found is that everything is saved on the cloud with your jitterbit account, which makes it easy for you to access the details anywhere. Plus is also enaables you to schedule exports or imports at anytime. I`m sure there are other tools which enable the same functionality as well. Although this just felt like the best option to me.

###

Data Import Considerations

1. **Import Order**
One of the main challenges when importing the data into the new instance is to understand the objects involved and how do they depend on each other. It usually helps to generate a data model and object relationships. This enables to visualize the relationships and sort out the order in which you should import these objects. 

2. **Maintaining inter object relationships**
This can be achieved again in multiple ways. One simple way that salesforce recommends is using VLOOKUPs and update the existing ids of the referenced object with the new id. For eg. If you are trying to import Opportunity Products(OpportunityLineItem), you need to make sure that the Accounts, Products, Pricebooks and PricebookEntries and Opportunities are available. Once you have these objects imported, you can export the corresponding ids and replace it using VLOOKUPs in the excel/csv containing the Opportunity Product data import set.  

Another way to achieve this, is to add an externalId field to the objects that you are importing. After that, you can easily sepcify the externalId field that you want to be used to refer objects with each other.

3. **Proxy Objects**
For all the objects, to which we can add a new custom field it is easy to relate the objects together and maintain a cross reference with the previous instance upload. But for objects like, Notes, Tasks, Events, etc it is not possible to add a custom field. In this case  we are left with two options, Firstly, we can use the reliable VLOOKUPs. Secondly, another good approach is to generate some proxy objects and import the data into those. Once, you have done that you can write some apex to import the data, find the right reference object and use it to create the right object. This can be easily done by writing a batch job. 

4. **Attachments**
If you have already tried exporting or importing attachments, you would know that it is tricker than all the other objects. Attachment object saves the binary object into the table in the body field. Thus, when attachment records are exported, binay data will be exported in the csv file. 

Although, to import attachments, we need to provide the csv file with all field details and path to the actual attachmet file.
To make this import easier we can use a little script and ask jitterbit to export attachment records, in a way where it is much easier for us to import it into the new salesforce org.

A. Create a new query
B. Specify the query
C. Create a new target file, name it as AttachmentFile
D. For the body field mapping edit the code to something like below, 
	<trans>
		$fn = root$transaction.response$body$queryResponse$result$records.Attachment$Name$;
		WriteFile("			<TAG>Targets/Files/AttachmentFile</TAG>",Base64Decode(root$transaction.response$body$queryResponse$result$records.Attachment$Body$),$fn);
		FlushFile("<TAG>Targets/Files/AttachmentFile</TAG>");
	</trans>

Now when you run the query the attachments will go into the folder.

In a few weeks, I`ll try and create some proxy object which can possibly be reused to use the approach detailed above.
