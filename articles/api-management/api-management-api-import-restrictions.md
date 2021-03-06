---
title: Beperkingen en bekende problemen in Azure API Management-API importeren | Microsoft Docs
description: Details van de bekende problemen en beperkingen voor het importeren in Azure API Management met de Open API, WSDL of WADL indelingen.
services: api-management
documentationcenter: ''
author: vladvino
manager: vlvinogr
editor: ''
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: apipm
ms.openlocfilehash: b33c95af94c436b1069658963692242d0f905554
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="api-import-restrictions-and-known-issues"></a>API importeren beperkingen en bekende problemen
## <a name="about-this-list"></a>Over deze lijst
Bij het importeren van een API kunt u tegenkomt enkele beperkingen of geven aan welke problemen die worden verholpen moeten voordat u met succes kunt importeren. Deze documenten artikel, ingedeeld door de importindeling van de API.

## <a name="open-api"> </a>Open API/Swagger
Als u uw document Open API importeren fouten ontvangt, controleert u of u hebt deze - met de ontwerpfunctie voor in de Azure portal (ontwerp - Front-End - API specificatie Editor openen), gevalideerd of met een derde hulpprogramma zoals <a href="http://www.swagger.io">Swagger Editor</a>.

* Alleen JSON-indeling voor OpenAPI wordt ondersteund.
* Schema's waarnaar wordt verwezen met **$ref** eigenschappen mogen niet andere **$ref** eigenschappen.
* **$ref** verwijzingen kunnen niet naar externe bestanden.
* **x-ms-paden** en **x servers** zijn de enige ondersteunde extensies.
* Aangepaste extensies zijn worden genegeerd tijdens het importeren en niet opgeslagen of bewaard voor exporteren.

> [!IMPORTANT]
> Zie dit [document](https://blogs.msdn.microsoft.com/apimanagement/2018/03/28/important-changes-to-openapi-import-and-export/) voor belangrijke informatie en tips die betrekking hebben op OpenAPI importeren.

## <a name="wsdl"> </a>WSDL
WSDL-bestanden worden gebruikt voor het genereren van SOAP Pass Through-API's of fungeren als de back-end van een SOAP-REST-API.
* **SOAP-bindingen** -alleen SOAP-bindingen van style 'document' en 'literal' codering worden ondersteund. Er is geen ondersteuning voor 'rpc'-stijl of SOAP-codering.
* **WSDL: import** -dit kenmerk wordt niet ondersteund. Klanten moeten de invoer samenvoegen in één document.
* **Berichten met meerdere onderdelen** -typen berichten worden niet ondersteund.
* **WCF-wsHttpBinding** -SOAP-services die zijn gemaakt met Windows Communication Foundation basicHttpBinding moeten gebruiken - wsHttpBinding wordt niet ondersteund.
* **MTOM** - Services met behulp van MTOM <em>kan</em> werken. Op dit moment is geen officiële ondersteuning geboden.
* **Recursie** -typen die zijn gedefinieerd recursief (bijvoorbeeld verwijzen naar een matrix van zichzelf) worden niet ondersteund door APIM.

## <a name="wadl"> </a>WADL
Er zijn momenteel geen bekende problemen van WADL importeren.
