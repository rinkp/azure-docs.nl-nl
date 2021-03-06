---
title: Verbinding maken met Azure analyseservices met Power BI | Microsoft Docs
description: Informatie over het verbinding maken met een Azure Analysis Services-server met behulp van Power BI.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 03e654b3d03564d11db9156e6fd5793bbd78c85d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="connect-with-power-bi"></a>Verbinding maken met Power BI

Zodra u hebt een server in Azure gemaakt en geïmplementeerd een model in tabelvorm, zijn gebruikers in uw organisatie gereed om te koppelen en gebruiken van gegevens. 

> [!TIP]
> Zorg ervoor dat u de nieuwste versie van [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a>Verbinding maken in Power BI Desktop

1. Klik in Power BI Desktop op **gegevens ophalen** > **Azure** > **Azure Analysis Services-database**.

2. In **Server**, voer de naam van de server. Zorg ervoor dat de volledige URL; bijvoorbeeld: asazure://westcentralus.asazure.windows.net/advworks.

3. In **Database**, als u weet dat de naam van de tabellaire modeldatabase of perspectief die u verbinding maken met wilt, plakt u deze hier. Anders kunt u dit veld leeg laten en later een database of perspectief selecteren.

4. Selecteer een verbindingsoptie voor de en druk vervolgens op **Connect**. 

    Beide **Connect live** en **importeren** opties worden ondersteund. Echter, raden we dat u live-verbindingen gebruiken omdat de modus Import heeft enkele beperkingen; met name, kan prestaties van de server worden beïnvloed tijdens het importeren. Ook als het model is worden vernieuwd in de Power BI-service, de **toegang toestaan vanuit Power BI** instelling geldt alleen wanneer u ervoor kiest **Connect live**.

5. Als u wordt gevraagd, voert u de aanmeldingsreferenties. 

6. In **Navigator**, vouw de server uit en selecteer vervolgens het model of perspectief die u wilt verbinding maken met en klik vervolgens op **Connect**. Klik op een model of perspectief om alle objecten voor deze weergave weer te geven.

    Het model wordt geopend in Power BI Desktop met een leeg rapport in de rapportweergave. De veldenlijst bevat alle modelobjecten niet-verborgen. Verbindingsstatus wordt weergegeven in de rechterbenedenhoek.

## <a name="connect-in-power-bi-service"></a>Verbinding maken in Power BI (service)

1. Maak een Power BI Desktop-bestand met een actieve verbinding met het model op uw server.
2. In [Power BI](https://powerbi.microsoft.com), klikt u op **gegevens ophalen** > **bestanden**, zoek en selecteer vervolgens uw pbix-bestand.



## <a name="see-also"></a>Zie ook
[Verbinding maken met Azure analyseservices](analysis-services-connect.md)   
[Clientbibliotheken](analysis-services-data-providers.md)

