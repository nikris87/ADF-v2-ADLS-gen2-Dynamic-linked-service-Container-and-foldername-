# ADF-v2-ADLS-gen2-Dynamic-linked-service-Container-and-foldername-
parameterize the ADLS account connection , container and folder name by editing json object

## ADF supports [these](https://docs.microsoft.com/en-us/azure/data-factory/parameterize-linked-services#supported-data-stores) sources to parameterize the linked service via UI . However forconnector like ADLS gen2 can be achieved by editing json code.
Below are the step by step approach and also I exported my example and attached here you can import same to your ADF.


### Step1 :

### Create three parameter at Pipeline level:
![Pipelineparameter](https://github.com/nikris87/ADF-v2-ADLS-gen2-Dynamic-linked-service-Container-and-foldername-/blob/main/Pipelineparameter_Step1.jpg) 

### Step2:
### Assuming you are using MSI auth.

Once you are in linked service page goto advanced section and use below shared json snippet and adjust name and default account accordingly:
 
![Editor](https://github.com/nikris87/ADF-v2-ADLS-gen2-Dynamic-linked-service-Container-and-foldername-/blob/main/linkedserviceadvanceeditor_Step2.jpg)

#### {
    "name": "ADLSgen2_Dynamic",
    "properties": {
        "parameters": {
            "storagename": {
                "type": "String",
                "defaultValue": "https://*****.dfs.core.windows.net"
            }
        },
        "annotations": [],
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "@linkedService().storagename"
        }
    },
    "type": "Microsoft.DataFactory/factories/linkedservices"
}



### Step3 :
### In the dataset level create three parameter as below (This will catch value from Pipeline level parameter)

![Datasetparameter](https://github.com/nikris87/ADF-v2-ADLS-gen2-Dynamic-linked-service-Container-and-foldername-/blob/main/Datasetparameter_Step3.jpg) 

### Step4:

### In the Dataset Linked service choose the Linked service that you created in Step2.

 As shown in below picture ,you will get option to pass Storage account name , This parameter is prompting based on the Storage name parameter you created in Step2.

 You can map Dataset parameters to storage name, container and Path.

 ![LS](https://github.com/nikris87/ADF-v2-ADLS-gen2-Dynamic-linked-service-Container-and-foldername-/blob/main/Datasetproperty_Step4.jpg)


### Step5:
### In the actual Copy activity , under sink Choose dataset that you created in previous step and that will prompt for the parameter , map the pipeline level parameter to these dataset level parameter as shown below

![Step5](https://github.com/nikris87/ADF-v2-ADLS-gen2-Dynamic-linked-service-Container-and-foldername-/blob/main/Copyactivity_Step5.jpg)
 
 
 ### ADF Template available [here](https://github.com/nikris87/ADF-v2-ADLS-gen2-Dynamic-linked-service-Container-and-foldername-/blob/main/Dynamic_LinkedService.zip) for Download
