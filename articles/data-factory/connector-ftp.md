---
title: Gegevens kopiëren van een FTP-server met behulp van Azure Data Factory | Microsoft Docs
description: Ontdek hoe u gegevens kopiëren van een FTP-server naar een ondersteunde sink gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2018
ms.author: jingwang
ms.openlocfilehash: 7e64d5ac33f96c06ddca29dd4ee001896c7cd131
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="copy-data-from-ftp-server-by-using-azure-data-factory"></a>Gegevens kopiëren van de FTP-server met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-ftp-connector.md)
> * [Versie 2 - Preview](connector-ftp.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van een FTP-server. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [FTP-connector in V1](v1/data-factory-ftp-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens kopiëren van FTP-server met een ondersteunde sink-gegevensarchief. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze FTP-connector:

- Het kopiëren van bestanden met behulp van **Basic** of **anoniem** verificatie.
- Kopiëren van bestanden als-is of bij het parseren van bestanden met de [ondersteunde bestandsindelingen en compressiecodecs](supported-file-formats-and-compression-codecs.md).

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor FTP.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor FTP-gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **FtpServer**. | Ja |
| host | Geef de naam of IP-adres van de FTP-server. | Ja |
| poort | Geef de poort waarop de FTP-server luistert.<br/>Toegestane waarden zijn: integer, standaardwaarde is **21**. | Nee |
| enableSsl | Geef op of FTP gebruiken via een SSL/TLS-kanaal.<br/>Toegestane waarden zijn: **true** (standaard), **false**. | Nee |
| enableServerCertificateValidation | Geef op of validatie van het servercertificaat SSL inschakelen wanneer u FTP via SSL/TLS-kanaal.<br/>Toegestane waarden zijn: **true** (standaard), **false**. | Nee |
| authenticationType | Geef het verificatietype.<br/>Toegestane waarden zijn: **Basic**, **anoniem** | Ja |
| Gebruikersnaam | De gebruiker die toegang tot de FTP-server heeft opgeven. | Nee |
| wachtwoord | Geef het wachtwoord voor de gebruiker (gebruikersnaam). Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Nee |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Azure integratie Runtime of Self-hosted integratie Runtime gebruiken (indien de gegevensopslag bevindt zich in een particulier netwerk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

>[!NOTE]
>De FTP-connector ondersteunt toegang tot FTP-server zonder versleuteling of een expliciete SSL/TLS-versleuteling; Deze biedt geen ondersteuning voor impliciete SSL/TLS-versleuteling.

**Voorbeeld 1: anonieme verificatie gebruikt**

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "<ftp server>",
            "port": 21,
            "enableSsl": true,
            "enableServerCertificateValidation": true,
            "authenticationType": "Anonymous"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voorbeeld 2: met behulp van basisverificatie**

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "<ftp server>",
            "port": 21,
            "enableSsl": true,
            "enableServerCertificateValidation": true,
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de FTP-gegevensset.

Stel de eigenschap type van de gegevensset om gegevens te kopiëren van FTP, **FileShare**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **bestandsshare** |Ja |
| folderPath | Pad naar de map. Wildcard-filter wordt niet ondersteund. Bijvoorbeeld: map/submap / |Ja |
| fileName | **Naam of het jokerteken filter** voor de bestanden onder de opgegeven 'folderPath'. Als u een waarde voor deze eigenschap niet opgeeft, wordt de gegevensset verwijst naar alle bestanden in de map. <br/><br/>Voor het filter toegestane jokertekens zijn: `*` (meerdere tekens) en `?` (willekeurig teken).<br/>-Voorbeeld 1: `"fileName": "*.csv"`<br/>-Voorbeeld 2: `"fileName": "???20180427.txt"`<br/>Gebruik `^` Escape als uw werkelijke bestandsnaam bevat jokertekens of deze escape-teken in. |Nee |
| Indeling | Als u wilt **kopiëren van bestanden als-is** overslaan tussen bestandsgebaseerde winkels (binaire kopiëren), de sectie indeling in de definities van beide invoer en uitvoer gegevensset.<br/><br/>Als u wilt parseren van bestanden met een specifieke indeling, de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**,  **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [Json-indeling](supported-file-formats-and-compression-codecs.md#json-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [parketvloeren indeling](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee (alleen voor scenario binaire kopiëren) |
| Compressie | Geef het type en de compressie van de gegevens. Zie voor meer informatie [ondersteunde bestandsindelingen en compressiecodecs](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus: **optimale** en **snelst**. |Nee |
| useBinaryTransfer | Geef op of de binaire overdrachtsmodus gebruiken. De waarden zijn true voor binaire modus (standaard) en false voor ASCII. |Nee |

>[!TIP]
>Geef alle bestanden onder een map wilt kopiëren, **folderPath** alleen.<br>Geef voor het kopiëren van één bestand met een bepaalde naam **folderPath** met maponderdeel en **fileName** met bestandsnaam.<br>Wilt kopiëren van een subset van de bestanden in een map, geeft **folderPath** met maponderdeel en **fileName** met jokertekenfilter.

>[!NOTE]
>Als u met de eigenschap 'fileFilter' voor bestandsfilter, nog steeds wordt ondersteund als-is, terwijl u gebruik van de nieuwe filter mogelijkheid is toegevoegd aan 'bestandsnaam' voortaan worden voorgesteld.

**Voorbeeld:**

```json
{
    "name": "FTPDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<FTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "myfile.csv.gz",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de FTP-bron.

### <a name="ftp-as-source"></a>Als bron voor FTP

Om gegevens te kopiëren van FTP, stelt u het brontype in de kopieerbewerking naar **FileSystemSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **FileSystemSource** |Ja |
| Recursieve | Hiermee wordt aangegeven of de gegevens recursief is gelezen uit de submappen of alleen uit de opgegeven map. Opmerking Wanneer recursieve is ingesteld op true en sink is bestandsgebaseerde opslag, lege map/subbewerkingen-folder niet worden gekopieerd/gemaakt op de sink.<br/>Toegestane waarden zijn: **true** (standaard), **false** | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromFTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<FTP input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).