---
title: Azure Application Insights transactie Diagnostics | Microsoft Docs
description: Application Insights-end-to-end-transactie diagnostische gegevens
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/19/2018
ms.author: mbullwin;sdash
ms.openlocfilehash: 7a4e4f74c02358fc117e0a66977ee3f0aef5b1dd
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="unified-cross-component-transaction-diagnostics"></a>Unified cross-onderdeel transactie diagnostische gegevens

*Deze ervaring is momenteel in preview en vervangt de bestaande diagnostische gegevens blades voor serverzijde aanvragen, afhankelijkheden en uitzonderingen.*

De preview introduceert een nieuwe eenduidige diagnostics-ervaring die automatisch serverzijde telemetrie van voor alle onderdelen van uw Application Insights bewaakt in één weergave correleert. Het maakt niet uit als u meerdere bronnen met afzonderlijke instrumentatiesleutels hebt. Application Insights detecteert de onderliggende relatie en kunt u eenvoudig onderzoeken het toepassingsonderdeel, afhankelijkheid of uitzondering die een transactie vertraging of een fout veroorzaakt.

## <a name="what-is-a-component"></a>Wat is een onderdeel?

Onderdelen zijn onafhankelijk implementeerbare onderdelen van uw toepassing gedistribueerd/microservices. Ontwikkelaars en operations teams hebben zichtbaarheid van de code-niveau of toegang tot de telemetrie die is gegenereerd door de onderdelen van deze toepassing.

* Onderdelen zijn anders dan 'waargenomen' externe afhankelijkheden, zoals SQL, enz. EventHub die uw team/organisatie mogelijk geen toegang tot (code of telemetrie).
* Onderdelen worden uitgevoerd op een willekeurig aantal exemplaren van de rol-server-container.
* Onderdelen kunnen worden afzonderlijke Application Insights-instrumentatiesleutels (zelfs als abonnementen verschillend zijn) of verschillende rollen die rapporteren aan een enkele Application Insights-instrumentatiesleutel. De nieuwe ervaring worden details weergegeven voor alle onderdelen, ongeacht hoe ze zijn ingesteld.

> [!NOTE]
> * **De koppelingen verwant item ontbreekt?** Alle van de telemetrie die is gerelateerd aan de serverzijde aanvraag, afhankelijkheid en uitzonderingen zijn in de [boven](#cross-component-transaction-chart) en [onder](#all-telemetry-related-to-the-selected-component-operation) secties van de linkerkant. 
> * De [boven](#cross-component-transaction-chart) sectie correleert de transactie voor alle onderdelen. Zorg dat alle onderdelen zijn uitgerust met de nieuwste Application Insights stabiele SDK's voor de beste resultaten. Als er verschillende Application Insights-bronnen, zorg ervoor dat u juiste rechten hebben om hun telemetrie weergeven.
> * De [onder](#all-telemetry-related-to-the-selected-component-operation) sectie van de linkerkant ziet **alle** telemetrie inclusief traceringen en gebeurtenissen in verband met de aanvraag van het geselecteerde onderdeel.

## <a name="enable-transaction-diagnostics-experience"></a>Ervaring van de transactie diagnostische gegevens inschakelen
Inschakelen "Unified details: E2E transactie diagnostische gegevens ' van de [previews lijst](app-insights-previews.md)

![Voorbeeld inschakelen](media/app-insights-e2eTxn-diagnostics/previews.png)

Dit voorbeeld is momenteel beschikbaar zijn voor de serverzijde aanvragen, afhankelijkheden en uitzonderingen. U hebt toegang tot de nieuwe ervaring van **zoekresultaten**, **prestaties**, of **fout** ervaringen te sorteren. De preview vervangt de bijbehorende klassieke details blades.

![Prestatievoorbeelden](media/app-insights-e2eTxn-diagnostics/performanceSamplesClickThrough.png)

## <a name="transaction-diagnostics-experience"></a>Transactie diagnostics ervaring
In deze weergave bevat drie belangrijke onderdelen: een grafiek transactie cross-onderdeel, een tijdreeks lijst met alle telemetrie van de bewerking van een specifiek onderdeel, en het deelvenster met details voor een geselecteerde telemetrie-item aan de linkerkant.

![Belangrijke delen](media/app-insights-e2eTxn-diagnostics/3partsCrossComponent.png)

## <a name="cross-component-transaction-chart"></a>Cross-onderdeel transactie-grafiek

Dit diagram biedt een tijdlijn met horizontale balken voor de duur van aanvragen en afhankelijkheden voor onderdelen. Eventuele uitzonderingen die worden verzameld, worden ook gemarkeerd op de tijdlijn.

* De eerste rij in deze grafiek vertegenwoordigt het ingangspunt de inkomende aanvraag voor het eerste onderdeel aangeroepen in deze transactie. De duur is de totale tijd voor de transactie te voltooien.
* Alle aanroepen naar externe afhankelijkheden zijn eenvoudige niet samenvouwbare rijen, met de pictogrammen van het afhankelijkheidstype.
* Aanroepen naar de andere onderdelen zijn samenvouwbare rijen. Elke rij overeenkomt met een specifieke bewerking aangeroepen op het onderdeel.
* Standaard wordt de aanvraag, afhankelijkheid of uitzondering die u aanvankelijk hebt geselecteerd op de grafiek weergegeven.
* Selecteer een rij om te zien de [meer informatie over het recht](#details-of-the-selected-telemetry). 

> [!NOTE]
Aanroepen naar de andere onderdelen hebben twee rijen: één rij vertegenwoordigt de uitgaande aanroep (afhankelijkheden) van het onderdeel van de aanroeper, en de andere rij komt overeen met de binnenkomende aanvraag op het onderdeel genoemd. Het toonaangevende pictogram en de afzonderlijke stijl van de duur van de balken helpen onderscheiden.

## <a name="all-telemetry-related-to-the-selected-component-operation"></a>Alle telemetrie die betrekking hebben op de geselecteerde onderdeel-bewerking

Elke rij in de grafiek cross-onderdeel transactie geselecteerd is gerelateerd aan een bewerking aangeroepen op een bepaald onderdeel. Deze bewerking geselecteerd onderdeel wordt doorgevoerd in de titel van het onderste gedeelte. Open in deze sectie om te zien van een tijdreeks platte van alle telemetrie die gerelateerd zijn aan die bepaalde bewerking. U kunt elk telemetrie-item selecteren in deze lijst om te zien overeenkomt [meer informatie over het recht](#details-of-the-selected-telemetry).

![Volgorde van de tijd van alle telemetrie](media/app-insights-e2eTxn-diagnostics/allTelemetryDrawerOpened.png)

## <a name="details-of-the-selected-telemetry"></a>Details van de geselecteerde telemetrie

Dit deelvenster geeft de details van de geselecteerde items uit de twee secties aan de linkerkant. 'Alles weergeven', worden alle van de standaard kenmerken die worden verzameld. Eventuele aangepaste kenmerken worden afzonderlijk onder de standaardset weergegeven. Klik op 'Open profiler traceringen' of 'Open foutopsporing momentopname' voor code niveau diagnostische gegevens in de bijbehorende details deelvensters.

![Uitzonderingsdetail](media/app-insights-e2eTxn-diagnostics/exceptiondetail.png)

## <a name="profiler-and-snapshot-debugger"></a>Profiler en momentopname-foutopsporingsprogramma

[Application Insights profiler](app-insights-profiler.md) of [momentopname foutopsporingsprogramma](app-insights-snapshot-debugger.md) met een code-niveau diagnostische gegevens van problemen met prestaties en de fout. Met deze ervaring, ziet u profiler traceringen of momentopnamen van elk onderdeel met één klik.

Als er kan geen Profiler werkt, contact op met **serviceprofilerhelp@microsoft.com**

Als er kan geen momentopname foutopsporingsprogramma werkt, contact op met **snapshothelp@microsoft.com**

![Foutopsporing-integratie](media/app-insights-e2eTxn-diagnostics/debugSnapshot.png)

## <a name="faq"></a>Veelgestelde vragen

*Op de grafiek één onderdeel weergegeven en de andere worden alleen weergegeven als externe afhankelijkheden zonder een detail van wat is er gebeurd binnen deze onderdelen.*

Mogelijke redenen:

* Zijn de andere onderdelen uitgerust met Application Insights?
* Gebruikt deze de meest recente stabiele Application Insights-SDK?
* Als deze onderdelen afzonderlijke Application Insights-resources zijn, hebt u vereiste toegang tot hun telemetrie?

Als u toegang hebt en de onderdelen zijn uitgerust met de nieuwste Application Insights-SDK's, laat ons weten via het kanaal bovenste rechts feedback.

*Ik heb alleen externe afhankelijkheden. Moet ik deze preview interesseren?*

Ja. De nieuwe ervaring verenigt alle gerelateerde serverzijde telemetrie in één weergave. De oudere detail blades worden vervangen door deze ervaring in de toekomst, dus try it out in en geef ons uw feedback. Hier ziet u hoe het eruitziet voor een transactie één onderdeel:

![Ervaring van één onderdeel](media/app-insights-e2eTxn-diagnostics/singleComponent.png)

*Ik zie dubbele rijen voor de afhankelijkheden. Wordt dit verwacht?*

Op dit moment zijn we de uitgaande afhankelijkheidsaanroep gescheiden van de binnenkomende aanvraag weergegeven. Het aanroepen van de twee zoek normaal gesproken identiek zijn met alleen de van de duurwaarde wordt retournetwerklatentie verschillende als gevolg van het netwerk. Het toonaangevende pictogram en de afzonderlijke stijl van de duur van de balken helpen onderscheiden. Deze presentatie van de gegevens verwarrend is? Geef ons uw feedback op prijs!

*Hoe zit klok Hiermee laat u overhellen over verschillende onderdeelexemplaren?*

Laat in het diagram transactie klok worden tijdlijnen aangepast. Hier ziet u de exacte tijdstempels in het detailvenster of met behulp van Analytics.

*Waarom ontbreekt de nieuwe ervaring voor de meeste van de verwante items query 's*

Dit is standaard. Alle verwante items voor alle onderdelen zijn al beschikbaar op de linkerkant (boven- en -secties). De nieuwe ervaring heeft twee verwante items die aan de linkerkant dekt niet: alle telemetrie van vijf minuten voor en na deze gebeurtenis en de tijdlijn van de gebruiker.

*Waarom heeft de nieuwe ervaring geen de functie die ik in het oudere blades populair?*

Geef ons feedback We willen uw problemen oplossen voordat deze ervaring is algemeen beschikbaar op dat moment de oudere weergaven wordt afgeschaft. Op dit moment kunt u het voorbeeld om terug te keren naar de oudere blades uitschakelen.

## <a name="known-issues"></a>Bekende problemen

* De fout-voorbeelden van de toepassingstoewijzing koppelen aan de oudere blades voor details.
* De autocluster gebaseerde inzichten in de zoekresultaten koppelen aan de oudere blades voor details.
* Werkitemintegratie is niet beschikbaar in de nieuwe ervaring.
