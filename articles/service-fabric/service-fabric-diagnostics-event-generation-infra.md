---
title: Azure Service Fabric-Platform niveau bewaking | Microsoft Docs
description: Meer informatie over platform niveau gebeurtenissen en logboeken die worden gebruikt om te bewaken en onderzoeken van Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: 31f23e3f8e792c6b61870c640f99ec3392a940d3
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="monitoring-the-cluster-and-platform"></a>Bewaking van het cluster en het platform

Het is belangrijk om te controleren op het niveau van het platform om te bepalen of uw hardware en het cluster gedragen zich zoals verwacht. Hoewel Service Fabric kunnen toepassingen die worden uitgevoerd tijdens een hardwarefout behouden, maar u toch wilt vaststellen of een fout optreedt in een toepassing of in de onderliggende infrastructuur. Ook moet u controleren clusters beter plannen van capaciteit, ondersteuning bij het nemen van beslissingen over het toevoegen of verwijderen van de hardware.

Service Fabric meerdere gestructureerde platform gebeurtenissen wordt als '[Service Fabric-gebeurtenissen](service-fabric-diagnostics-events.md), ' Aanmelden via de EventStore en verschillende kanalen out-of-the-box. 

De EventStore krijgt u toegang tot uw cluster gebeurtenissen op basis van per entiteit (entiteiten met inbegrip van de cluster, knooppunten, toepassingen, services, partities, replica's en containers) en beschrijft de gegevens via de REST-API's en de Service Fabric-clientbibliotheek. De EventStore gebruiken voor het bewaken van clusters ontwikkelen en testen, en voor het ophalen van een punt in tijd begrip van de status van uw productieclusters. Meer informatie over deze [EventStore overzicht](service-fabric-diagnostics-eventstore.md).

Service Fabric bevat ook dat het volgende logboek kanalen out-of-the-box voor het instellen van een pijplijn voor het bewaken van uw productieclusters:

* [**Operationele**](service-fabric-diagnostics-event-generation-operational.md)  
Op hoog niveau bewerkingen wordt uitgevoerd door de Service Fabric en het cluster, met inbegrip van gebeurtenissen voor een knooppunt dat oefening, een nieuwe toepassing wordt geïmplementeerd of een upgrade terugdraaien, enzovoort.

* **Operationele - gedetailleerde**  
Statusrapporten en beslissingen voor de taakverdeling.

* **Gegevens & Messaging**  
Kritieke logboeken en gebeurtenissen die worden gegenereerd in de berichtgeving (momenteel alleen de ReverseProxy) en gegevenspad (betrouwbare services-modellen).

* **& Messaging - gedetailleerde gegevens**  
Uitgebreide kanaal dat deze de niet-kritieke logboeken van gegevens en berichten in het cluster bevat (dit kanaal heeft een zeer groot aantal gebeurtenissen).

Naast deze zijn er twee gestructureerde EventSource kanalen die zijn opgegeven, evenals logboeken die worden verzameld voor ondersteuning.

* [Reliable Services-gebeurtenissen](service-fabric-reliable-services-diagnostics.md)  
Programmering model specifieke gebeurtenissen.

* [Reliable Actors-gebeurtenissen](service-fabric-reliable-actors-diagnostics.md)  
Programmering model specifieke gebeurtenissen en prestatiemeteritems.

* Ondersteuning voor logboeken  
Systeemlogboeken gegenereerd door de Service Fabric alleen moet worden gebruikt door ons bij om ondersteuning te bieden.

Deze verschillende kanalen betrekking hebben op de meeste van de platform logboekregistratie die wordt aanbevolen. Ter verbetering van de logboekregistratie platform, overweeg investeren in een beter inzicht in het model van de gezondheid en aangepaste statusrapporten toe te voegen en toe te voegen aangepaste **prestatiemeteritems** voor het bouwen van een realtime begrip van de impact van uw Services en toepassingen op het cluster.

Om te profiteren van deze logboeken, is het raadzaam tijdens het maken van het cluster ' diagnostische gegevens ' is ingeschakeld. Door het inschakelen van diagnostische gegevens, wanneer het cluster wordt geïmplementeerd, Windows Azure Diagnostics kan bevestigt de operationele, Reliable Services en Reliable actors kanalen, en de gegevens opslaan als verder uiteengezet in [aggregeren van gebeurtenissen met Azure Diagnostische gegevens](service-fabric-diagnostics-event-aggregation-wad.md).

## <a name="azure-service-fabric-health-and-load-reporting"></a>Azure Service Fabric-status en load reporting

Service Fabric heeft een eigen statusmodel die is beschreven in deze artikelen:

- [Inleiding tot Service Fabric-statuscontrole](service-fabric-health-introduction.md)
- [Servicestatus rapporteren en controleren](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
- [Aangepaste Service Fabric-statusrapporten toevoegen](service-fabric-report-health.md)
- [Service Fabric-statusrapporten weergeven](service-fabric-view-entities-aggregated-health.md)

Statuscontrole is essentieel voor meerdere aspecten van het besturingssysteem van een service. Statuscontrole is vooral belangrijk bij het Service Fabric een upgrade van de genoemde toepassing voert. Nadat elk upgradedomein van de service is bijgewerkt en beschikbaar voor uw klanten is, moet het upgradedomein statuscontroles doorgeven voordat de implementatie wordt verplaatst naar het volgende upgradedomein. Als de status van goede kan niet worden verkregen, is de implementatie teruggedraaid, zodat de toepassing bevindt zich in een bekende goede status. Sommige klanten kunnen worden getroffen voordat de services worden teruggedraaid, maar de meeste klanten kunnen een probleem won't problemen. Een resolutie vindt ook plaats relatief snel en zonder te wachten op actie van een menselijke operator. De meer statuscontroles die zijn opgenomen in uw code, de toleranter die implementatieproblemen uw service is.

Een ander aspect van de status van de service is metrische gegevens van de service reporting. Metrische gegevens zijn belangrijk in Service Fabric omdat ze worden gebruikt voor het gebruik van bronnen worden verdeeld. Metrische gegevens kan ook een indicator van de systeemstatus zijn. Bijvoorbeeld, moet u wellicht een toepassing die veel services heeft en elk exemplaar een aanvragen per tweede (RPS) metriek rapporten. Als een service van meer bronnen dan een andere service gebruikmaakt, verplaatst Service Fabric service-exemplaren rond het cluster, om te proberen te onderhouden zelfs bronnen beter worden benut. Zie voor een meer gedetailleerde uitleg van de werking van Resourcegebruik [brongebruik te beheren en te laden in Service Fabric met metrische gegevens](service-fabric-cluster-resource-manager-metrics.md).

Metrische gegevens ook kunt bieden u inzicht in hoe uw service wordt uitgevoerd. U kunt metrische gegevens gedurende een periode gebruiken om te controleren dat de service wordt uitgevoerd binnen de verwachte parameters. Bijvoorbeeld, als trends laat zien dat op 9: 00 uur op maandagochtend het gemiddelde RPS 1000, vervolgens u mogelijk instellen een statusrapport die u waarschuwt als de RPS 500 of meer dan 1500. Alles wat u mogelijk geen bezwaar, maar mogelijk waard om ervoor te zorgen dat uw klanten een geweldige ervaring hebben. Uw service kunt een set metrische gegevens die worden gerapporteerd voor de doeleinden van selectievakje, maar die niet van invloed op de resource-verdeling van het cluster definiëren. U doet dit door het gewicht metrische te ingesteld op nul. U wordt aangeraden alle metrische gegevens beginnen met een gewicht van nul en het gewicht niet verhogen tot u zeker weet dat u begrijpt hoe weging van de metrische gegevens is van invloed op resourceverdeling voor uw cluster.

> [!TIP]
> Gebruik niet te veel gewogen metrische gegevens. Het kan lastig zijn om te begrijpen waarom service-exemplaren worden verplaatst rond voor netwerktaakverdeling. Enkele metrische gegevens kan worden achterhaald!

Alle gegevens die kan duiden op de status en prestaties van uw toepassing is geschikt is voor de metrische gegevens en de gezondheid van rapporten. Een CPU-prestatiemeteritem kan aangeven hoe uw knooppunt wordt gebruikt, maar deze niet zien of een bepaalde service is in orde, omdat er meerdere services op één knooppunt kunnen worden uitgevoerd. Metrische gegevens, zoals RPS, items verwerkt, maar alle latentie van aanvraag kan wijzen op de status van een specifieke service.

## <a name="service-fabric-support-logs"></a>Logboeken voor service Fabric-ondersteuning

Als u contact op met Microsoft ondersteuning voor hulp bij uw Azure Service Fabric-cluster moet, zijn bijna altijd Ondersteuningslogboeken vereist. Als uw cluster wordt gehost in Azure, worden logboeken automatisch geconfigureerd en verzameld als onderdeel van het maken van een cluster. De logboeken worden opgeslagen in een speciaal opslagaccount in de resourcegroep voor uw cluster. Het storage-account beschikt niet over een vaste naam, maar in het account, ziet u de blob-containers en tabellen met namen die met beginnen *fabric*. Zie voor meer informatie over het instellen van logboekverzamelingen voor een zelfstandige cluster [maken en beheren van een zelfstandige Azure Service Fabric-cluster](service-fabric-cluster-creation-for-windows-server.md) en [configuratie-instellingen voor een zelfstandige Windows cluster](service-fabric-cluster-manifest.md). Voor zelfstandige Service Fabric-exemplaren, moeten de logboeken naar een lokale bestandsshare worden verzonden. U bent **vereist** hebben deze logboeken voor ondersteuning, maar ze zijn niet bedoeld om te worden gebruikt door personen buiten het Microsoft customer support team.

## <a name="measuring-performance"></a>Meten van de prestaties

Prestaties van de meting van het cluster helpt u begrijpen hoe overweg kan laden en het station beslissingen rond het schalen van uw cluster (Zie voor meer informatie over het schalen van een cluster [op Azure](service-fabric-cluster-scale-up-down.md), of [lokale](service-fabric-cluster-windows-server-add-remove-nodes.md)). Prestatiegegevens is ook handig in vergelijking met acties die u of uw toepassingen en services kunnen hebt uitgevoerd, als analyseren in Logboeken in de toekomst. 

Zie voor een lijst van te verzamelen bij gebruik van Service Fabric prestatiemeteritems [prestatiemeters in Service Fabric](service-fabric-diagnostics-event-generation-perf.md)

Hier zijn twee algemene manieren waarop u kunt instellen voor het verzamelen van prestatiegegevens voor het cluster:

* **Met behulp van een agent**  
Dit is de beste manier om het verzamelen van prestaties van een machine Aangezien agents hebben meestal een lijst met mogelijke prestaties metrische gegevens die kunnen worden verzameld en is een relatief gemakkelijk proces voor het kiezen van de metrische gegevens die u wilt verzamelen of deze te wijzigen. Meer informatie over [de OMS configureren voor Service Fabric](service-fabric-diagnostics-event-analysis-oms.md) en [instellen van de OMS-Agent voor Windows](../log-analytics/log-analytics-windows-agent.md) artikelen voor meer informatie over de OMS-agent is een dergelijke monitoring agent die kunnen worden opgepikt prestaties gegevens voor het cluster virtuele machines en geïmplementeerde containers.

* **Configuratie van diagnostische gegevens prestatiemeteritems schrijven naar een tabel**  
Voor clusters op Azure, betekent dit dat wijzigen van de configuratie van Azure Diagnostics om op te halen de juiste prestatiemeteritems van de virtuele machines in het cluster en het inschakelen van deze docker-statistieken kunnen worden opgepikt als u geen containers wilt implementeren. Meer informatie over het configureren van [prestatiemeters in af](service-fabric-diagnostics-event-aggregation-wad.md) in Service Fabric voor het instellen van het verzamelen van prestatiemeteritems.

## <a name="next-steps"></a>Volgende stappen

Uw logboeken en gebeurtenissen moeten worden samengevoegd voordat ze kunnen worden verzonden naar een willekeurig platform analyse. Meer informatie over [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) en [af](service-fabric-diagnostics-event-aggregation-wad.md) om enkele van de aanbevolen opties beter te begrijpen.
