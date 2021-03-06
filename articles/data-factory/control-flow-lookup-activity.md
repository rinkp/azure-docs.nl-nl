---
title: De activiteit opzoeken in Azure Data Factory | Microsoft Docs
description: Informatie over het gebruik van lookup-activiteit voor het opzoeken van een waarde van een externe bron. Er kan naar deze uitvoer worden verwezen door volgende activiteiten.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: shlo
ms.openlocfilehash: b6c2e2b685855455550612abb58ada6a694bbdff
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="lookup-activity-in-azure-data-factory"></a>De activiteit opzoeken in Azure Data Factory

Lookup activiteit kan worden gebruikt voor het ophalen van een gegevensset uit een van de ADF-ondersteunde gegevensbron.  Worden kan gebruikt in het volgende scenario:
- Dynamisch bepalen welke objecten (bestanden, tabellen, enzovoort) om te werken op een volgende activiteit in plaats van hard-coding van de objectnaam

Lookup-activiteit kunt lezen en de inhoud van een configuratiebestand, een configuratietabel of het resultaat van het uitvoeren van een query of een opgeslagen procedure.  De uitvoer van Lookup-activiteit worden gebruikt in een latere kopiëren of transformatieactiviteit als het een waarde singleton of in een ForEach-activiteit gebruikt als het is een matrix van kenmerken.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Azure Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt die algemeen beschikbaar is (GA), raadpleegt u [Documentatie van versie 1 van Data Factory](v1/data-factory-introduction.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

De volgende gegevensbronnen worden ondersteund voor het opzoeken van. Het maximale aantal rijen kan worden geretourneerd door het opzoeken van de activiteit is **5000**, tot aan **2MB** in grootte. En de maximumduur voor de activiteit opzoeken voordat de time-out is momenteel een uur.

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores-for-lookup-activity.md)]

## <a name="syntax"></a>Syntaxis

```json
{
    "name": "LookupActivity",
    "type": "Lookup",
    "typeProperties": {
        "source": {
            "type": "<source type>"
            <additional source specific properties (optional)>
        },
        "dataset": { 
            "referenceName": "<source dataset name>",
            "type": "DatasetReference"
        },
        "firstRowOnly": false
    }
}
```

## <a name="type-properties"></a>Type-eigenschappen
Naam | Beschrijving | Type | Vereist?
---- | ----------- | ---- | --------
Gegevensset | Biedt de dataset-verwijzing voor de zoekopdracht. Details van de sectie 'Eigenschappen van gegevensset' in elke bijbehorende connector artikel ophalen. | Sleutel-waardepaar | Ja
source | Bevat Broneigenschappen van gegevensset-specifieke, hetzelfde zijn als de bron voor kopiëren-activiteit. Details van de sectie 'activiteitseigenschappen kopiëren' in elke bijbehorende connector artikel ophalen. | Sleutel-waardepaar | Ja
firstRowOnly | Geeft aan of alleen de eerste rij of alle rijen retourneren. | Boole-waarde | Nee. De standaardwaarde is `true`.

**Houd rekening met de volgende punten:**

1. De bronkolom met ByteArray type wordt niet ondersteund.
2. Structuur wordt niet ondersteund in de gegevenssetdefinitie van de. Voor tekst format-bestanden in het bijzonder kunt u de rij met kolomkoppen de kolomnaam op te geven.
3. Als bron voor het opzoeken een JSON-bestanden is, de `jsonPathDefinition` instellen voor opnieuw vormgeven van de JSON-object wordt niet ondersteund, het gehele objecten wordt opgehaald.

## <a name="use-the-lookup-activity-result-in-a-subsequent-activity"></a>Gebruik het resultaat van de activiteit opzoeken in een volgende activiteit

Het resultaat van de zoekactie wordt geretourneerd als de `output` sectie van het resultaat van uitgevoerde activiteit.

* **Wanneer `firstRowOnly` is ingesteld op `true` (standaard)**, de indeling van de uitvoer is zoals wordt weergegeven in de volgende code. Het resultaat van de zoekactie is onder een vaste `firstRow` sleutel. Als u het resultaat van de volgende activiteit, gebruikt u het patroon van `@{activity('MyLookupActivity').output.firstRow.TableName}`.

    ```json
    {
        "firstRow":
        {
            "Id": "1",
            "TableName" : "Table1"
        }
    }
    ```

* **Wanneer `firstRowOnly` is ingesteld op `false`** , de indeling van de uitvoer is zoals wordt weergegeven in de volgende code. Een `count` veld geeft aan hoeveel records worden geretourneerd en gedetailleerde waarden worden weergegeven onder een vaste `value` matrix. In dat geval wordt de activiteit opzoeken meestal gevolgd door een [Foreach-activiteit](control-flow-for-each-activity.md). U kunt doorgeven de `value` -matrix naar de ForEach-activiteit `items` veld met behulp van het patroon van `@activity('MyLookupActivity').output.value`. Voor toegang tot elementen in de `value` matrix kunnen de volgende syntaxis: `@{activity('lookupActivity').output.value[zero based index].propertyname}`. Hier volgt een voorbeeld: `@{activity('lookupActivity').output.value[0].tablename}`.

    ```json
    {
        "count": "2",
        "value": [
            {
                "Id": "1",
                "TableName" : "Table1"
            },
            {
                "Id": "2",
                "TableName" : "Table2"
            }
        ]
    } 
    ```

## <a name="example"></a>Voorbeeld
In dit voorbeeld kopieert de kopieeractiviteit gegevens van een SQL-tabel in uw Azure SQL Database-exemplaar naar Azure Blob-opslag. De naam van de SQL-tabel is opgeslagen in een JSON-bestand in Blob-opslag. De activiteit opzoeken, zoekt u de naam van de tabel tijdens runtime. Deze aanpak kunt JSON dynamisch worden gewijzigd zonder dat u moet implementeren pijplijnen of gegevenssets. 

In dit voorbeeld demonstreert lookup voor alleen de eerste rij. Zie voor lookup voor alle rijen en om te koppelen van de resultaten met ForEach-activiteit, de voorbeelden in [meerdere tabellen in bulksgewijs kopiëren met behulp van Azure Data Factory](tutorial-bulk-copy.md).

### <a name="pipeline"></a>Pijplijn
Deze pipeline bevat twee activiteiten: *lookup* en *kopie*. 

- De activiteit lookup is geconfigureerd voor gebruik van LookupDataset die naar een locatie in Azure Blob-opslag verwijst. De activiteit opzoeken leest de naam van de SQL-tabel uit een JSON-bestand op deze locatie. 
- De kopieeractiviteit wordt de uitvoer van de activiteit lookup (naam van de SQL-tabel). De eigenschap tableName in de bron-gegevensset (SourceDataset) is geconfigureerd voor gebruik van de uitvoer van de activiteit opzoeken. De kopieeractiviteit kopieert gegevens van de SQL-tabel naar een locatie in Azure Blob-opslag die is opgegeven door de eigenschap SinkDataset. 


```json
{
    "name": "LookupPipelineDemo",
    "properties": {
        "activities": [
            {
                "name": "LookupActivity",
                "type": "Lookup",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "dataset": { 
                        "referenceName": "LookupDataset", 
                        "type": "DatasetReference" 
                    }
                }
            },
            {
                "name": "CopyActivity",
                "type": "Copy",
                "typeProperties": {
                    "source": { 
                        "type": "SqlSource", 
                        "sqlReaderQuery": "select * from @{activity('LookupActivity').output.firstRow.tableName}" 
                    },
                    "sink": { 
                        "type": "BlobSink" 
                    }
                },                
                "dependsOn": [ 
                    { 
                        "activity": "LookupActivity", 
                        "dependencyConditions": [ "Succeeded" ] 
                    }
                 ],
                "inputs": [ 
                    { 
                        "referenceName": "SourceDataset", 
                        "type": "DatasetReference" 
                    } 
                ],
                "outputs": [ 
                    { 
                        "referenceName": "SinkDataset", 
                        "type": "DatasetReference" 
                    } 
                ]
            }
        ]
    }
}
```

### <a name="lookup-dataset"></a>Lookup-gegevensset
De gegevensset lookup verwijst naar de *sourcetable.json* bestand in de map voor het opzoeken van Azure Storage dat opgegeven door het type AzureStorageLinkedService. 

```json
{
    "name": "LookupDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "lookup",
            "fileName": "sourcetable.json",
            "format": {
                "type": "JsonFormat",
                "filePattern": "SetOfObjects"
            }
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="source-dataset-for-the-copy-activity"></a>Gegevensset voor de kopieeractiviteit bron
De bron-gegevensset maakt gebruik van de uitvoer van de activiteit opzoeken, die de naam van de SQL-tabel. De kopieeractiviteit kopieert gegevens van deze SQL-tabel naar een locatie in Azure Blob-opslag die opgegeven door de sink-gegevensset. 

```json
{
    "name": "SourceDataset",
    "properties": {
        "type": "AzureSqlTable",
        "typeProperties":{
            "tableName": "@{activity('LookupActivity').output.firstRow.tableName}"
        },
        "linkedServiceName": {
            "referenceName": "AzureSqlLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="sink-dataset-for-the-copy-activity"></a>Gegevensset voor de kopieeractiviteit sink
De kopieeractiviteit kopieert gegevens van de SQL-tabel om de *filebylookup.csv* bestand de *csv* map in de Azure-opslag die opgegeven door de eigenschap AzureStorageLinkedService. 

```json
{
    "name": "SinkDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "csv",
            "fileName": "filebylookup.csv",
            "format": {
                "type": "TextFormat"                                                                    
            }
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service
Dit opslagaccount bevat de JSON-bestand met de namen van de SQL-tabellen. 

```json
{
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "value": "DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<StorageAccountKey>",
                "type": "SecureString"
            }
        }
    },
        "name": "AzureStorageLinkedService"
}
```

### <a name="azure-sql-database-linked-service"></a>Een gekoppelde Azure SQL Database-service
Dit exemplaar van Azure SQL Database bevat de gegevens moeten worden gekopieerd naar Blob storage. 

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": {
                "value": "Server=<server>;Initial Catalog=<database>;User ID=<user>;Password=<password>;",
                "type": "SecureString"
            }
        }
    }
}
```

### <a name="sourcetablejson"></a>SourceTable.JSON

#### <a name="set-of-objects"></a>Set van objecten

```json
{
  "Id": "1",
  "tableName": "Table1",
}
{
   "Id": "2",
  "tableName": "Table2",
}
```

#### <a name="array-of-objects"></a>Matrix met objecten

```json
[ 
    {
        "Id": "1",
          "tableName": "Table1",
    }
    {
        "Id": "2",
        "tableName": "Table2",
    }
]
```

## <a name="next-steps"></a>Volgende stappen
Zie andere controlestroomactiviteiten die door Data Factory worden ondersteund: 

- [Uitvoeren van de Pipeline-activiteit](control-flow-execute-pipeline-activity.md)
- [Voor elke activiteit](control-flow-for-each-activity.md)
- [Ophalen van metagegevens-activiteit](control-flow-get-metadata-activity.md)
- [Activiteit Web](control-flow-web-activity.md)
