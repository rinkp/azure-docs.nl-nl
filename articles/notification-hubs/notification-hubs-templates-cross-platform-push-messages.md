---
title: Sjablonen
description: Dit onderwerp worden de sjablonen voor Azure notification hubs.
services: notification-hubs
documentationcenter: .net
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: a41897bb-5b4b-48b2-bfd5-2e3c65edc37e
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 3e587bdf0efc7c5b416183640abb19286a5cff31
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="templates"></a>Sjablonen
## <a name="overview"></a>Overzicht
Sjablonen ingeschakeld een clienttoepassing om op te geven van de precieze indeling van de meldingen wil ontvangen. Met behulp van sjablonen, kan een app verschillende verschillende voordelen, waaronder de volgende voordelen:

* Een back-end platform networkdirect
* Aangepaste meldingen
* Onafhankelijkheid van de client-versie
* Eenvoudig lokalisatie

Deze sectie bevat twee gedetailleerde voorbeelden van hoe u sjablonen gebruikt om platform networkdirect-meldingen die gericht is op alle apparaten in verschillende platforms te verzenden en te personaliseren broadcast melding aan elk apparaat.

## <a name="using-templates-cross-platform"></a>Met behulp van sjablonen cross-platform
Er is de standaardmethode voor het verzenden van pushmeldingen te verzenden, voor elk bericht dat moet worden verzonden, een specifieke nettolading met platform notification services (WNS, APNS). De nettolading is bijvoorbeeld een waarschuwing om naar te verzenden APNS, een Json-object van de volgende notatie:

    {"aps": {"alert" : "Hello!" }}

Voor het verzenden van een vergelijkbaar pop-up bericht op een Windows Store-toepassing, is de XML-nettolading als volgt uit:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">Hello!</text>
        </binding>
      </visual>
    </toast>

U kunt vergelijkbare nettoladingen maken voor MPNS (Windows Phone) en (Android) GCM-platforms.

Deze vereiste zorgt ervoor dat de app-back-end voor het produceren van verschillende nettoladingen voor elk platform, waardoor het effectief de back-end die verantwoordelijk is voor een deel van de laag voor presentatie van de app. Sommige problemen bevatten te lokaliseren en grafische indelingen (vooral voor Windows Store-apps die meldingen voor verschillende soorten tegels bevatten).

De functie van de sjabloon Notification Hubs kunt een client-app voor het maken van speciale registraties, aangeroepen sjabloon registraties, waaronder, naast de reeks labels, een sjabloon. De functie van de sjabloon Notification Hubs kunt een client-app te koppelen van apparaten met behulp van sjablonen of u met werkt (voorkeur)-installaties of -registraties. Gezien de voorgaande voorbeelden van de nettolading, is de informatie alleen platformonafhankelijk het werkelijke waarschuwingsbericht (Hallo!). Een sjabloon is een verzameling instructies voor de Notification Hub voor het formatteren van een bericht platformonafhankelijk voor de registratie van die specifieke client-app. In het voorgaande voorbeeld is het bericht platformonafhankelijk één eigenschap: **bericht = Hallo!**.

De volgende afbeelding illustreert het proces:

![](./media/notification-hubs-templates/notification-hubs-hello.png)

De sjabloon voor de iOS-app clientregistratie is als volgt:

    {"aps": {"alert": "$(message)"}}

De bijbehorende sjabloon voor de client-app voor Windows Store is:

    <toast>
        <visual>
            <binding template=\"ToastText01\">
                <text id=\"1\">$(message)</text>
            </binding>
        </visual>
    </toast>

U ziet dat het werkelijke bericht is vervangen door de expressie $(bericht). Deze expressie Hiermee geeft u de Notification Hub wanneer deze een bericht naar deze bepaalde registratie verzendt voor het bouwen van een bericht dat volgt op deze en switches in de algemene waarde.

Als u met installatie-model werkt, bevat de installatie 'sjablonen' sleutel een JSON met meerdere sjablonen. Als u met registratie model werkt, kunt de clienttoepassing meerdere registraties maken om te kunnen gebruiken, meerdere sjablonen. bijvoorbeeld, een sjabloon voor waarschuwingsberichten en een sjabloon voor updates van de tegel. Client-toepassingen kunnen u ook systeemeigen registraties (registraties zonder sjabloon) en sjabloon registraties elkaar.

De Notification Hub verzendt een melding voor elke sjabloon zonder rekening te houden of ze tot dezelfde clientapp behoren. Dit gedrag kan worden gebruikt om te vertalen platformonafhankelijk meldingen naar meer meldingen. Bijvoorbeeld kan hetzelfde platformonafhankelijk bericht met de Notification Hub worden naadloos omgezet in een toast-melding en een update tegel zonder dat de back-end worden hiervan bewust zijn. Sommige platformen (bijvoorbeeld iOS) mogelijk meerdere meldingen samenvouwen op hetzelfde apparaat als ze worden verzonden in een korte periode.

## <a name="using-templates-for-personalization"></a>Met behulp van sjablonen voor persoonlijke instellingen
Een ander voordeel van met behulp van sjablonen is de mogelijkheid Notification Hubs gebruiken om uit te voeren per registratie personalisatie van meldingen. Neem bijvoorbeeld een app weer waarin een tegel met de voorwaarden weer op een specifieke locatie. Een gebruiker kan kiezen tussen c of Fahrenheit graden en een prognose enkele of vijf dagen. Met behulp van sjablonen, de installatie van elke client-app kunt registreren voor de vereiste indeling (1 dag Celsius, 1 dag Fahrenheit, 5 dagen c 5 dagen Fahrenheit), en de back-end een enkel bericht met de gegevens die zijn vereist voor het vervullen van deze sjablonen verzenden (bijvoorbeeld een prognose met c en Fahrenheit graden vijf dagen).

De sjabloon voor de prognose één dag met c temperaturen is als volgt:

    <tile>
      <visual>
        <binding template="TileWideSmallImageAndText04">
          <image id="1" src="$(day1_image)" alt="alt text"/>
          <text id="1">Seattle, WA</text>
          <text id="2">$(day1_tempC)</text>
        </binding>  
      </visual>
    </tile>

Het bericht verzonden naar de Notification Hub bevat de volgende eigenschappen:

<table border="1">

<tr><td>day1_image</td><td>day2_image</td><td>day3_image</td><td>day4_image</td><td>day5_image</td></tr>

<tr><td>day1_tempC</td><td>day2_tempC</td><td>day3_tempC</td><td>day4_tempC</td><td>day5_tempC</td></tr>

<tr><td>day1_tempF</td><td>day2_tempF</td><td>day3_tempF</td><td>day4_tempF</td><td>day5_tempF</td></tr>
</table><br/>

Met behulp van dit patroon, verzendt de back-end slechts één bericht zonder voor het opslaan van specifieke personalisatie opties voor de app-gebruikers. De volgende afbeelding ziet u dit scenario:

![](./media/notification-hubs-templates/notification-hubs-registration-specific.png)

## <a name="how-to-register-templates"></a>Het registreren van sjablonen
Als u wilt registreren met behulp van sjablonen met behulp van de installatie-model (voorkeur), of de registratie, Zie [registratie Management](notification-hubs-push-notification-registration-management.md).

## <a name="template-expression-language"></a>Expressie sjabloontaal
Sjablonen zijn beperkt tot indelingen van XML- of JSON-document. Bovendien kunt u alleen plaatsen expressies in bepaalde plaatsen; bijvoorbeeld, de kenmerken van knooppunt of de waarden voor XML tekenreeks eigenschapswaarden voor JSON.

De volgende tabel ziet u de taal die is toegestaan in sjablonen:

| Expressie | Beschrijving |
| --- | --- |
| $(prop) |Verwijzing naar een gebeurteniseigenschap met de opgegeven naam. De namen van eigenschappen zijn niet hoofdlettergevoelig. Deze expressie wordt omgezet in de tekstwaarde van de eigenschap of een lege tekenreeks als de eigenschap niet aanwezig is. |
| $(prop, n) |Zoals hierboven, maar de tekst expliciet is afgekapt op n tekens, bijvoorbeeld $(titel, 20) knipt de inhoud van de eigenschap title op 20 tekens bestaan. |
| . (prop, n) |Zoals hierboven, maar de tekst wordt voorafgegaan door drie punten omdat deze is afgekapt. De totale grootte van de afgekapte tekenreeks en het achtervoegsel overschrijdt niet n tekens. . (titel, 20) met een invoer-eigenschap van de resultaten in 'Is de titelregel ' **dit is de titel...** |
| %(Prop) |Vergelijkbaar zijn met $(name), behalve dat de uitvoer is de URI-codering. |
| #(prop) |In JSON-sjablonen gebruikt (bijvoorbeeld voor iOS en Android-sjablonen).<br><br>Deze functie werkt precies hetzelfde als $(prop) eerder hebt opgegeven, behalve wanneer in JSON-sjablonen (bijvoorbeeld een Apple-sjablonen). In dit geval, als deze functie wordt niet omgeven door '{','}' (bijvoorbeeld myJsonProperty: '#(naam)'), en dit resulteert in een getal in Javascript-indeling, bijvoorbeeld regexp: (0&#124;(&#91;1-9&#93;&#91;0-9&#93;*)) (\. &#91;0-9&#93;+)? ((e&#124;E) (+&#124;-)? &#91;0-9&#93;+)?, en vervolgens de JSON-uitvoer een getal is.<br><br>Bijvoorbeeld ' badge: '#(naam)', wordt de badge': 40 (en niet 40). |
| 'text' of 'text' |Een letterlijke waarde. Letterlijke waarden bevatten willekeurige tekst tussen enkele of dubbele aanhalingstekens. |
| Expr1 + expr2 |De samenvoegingsoperator lid te worden twee expressies in één tekenreeks. |

De expressies mag geen van de voorgaande formulieren.

Bij gebruik van de samenvoeging met de gehele expressie moet tussen {}. Bijvoorbeeld, {$(prop) + '-' + $(prop2)}. |

De volgende sjabloon is bijvoorbeeld niet een geldig XML-sjabloon:

    <tile>
      <visual>
        <binding $(property)>
          <text id="1">Seattle, WA</text>
        </binding>  
      </visual>
    </tile>


Zoals eerder uitgelegd wanneer u samenvoeging, moeten expressies tussen accolades worden geplaatst. Bijvoorbeeld:

    <tile>
      <visual>
        <binding template="ToastText01">
          <text id="1">{'Hi, ' + $(name)}</text>
        </binding>  
      </visual>
    </tile>

