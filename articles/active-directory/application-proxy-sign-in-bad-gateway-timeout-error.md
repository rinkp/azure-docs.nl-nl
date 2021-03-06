---
title: Geen toegang tot deze zakelijke toepassing een fout bij het gebruik van een toepassing toepassingsproxy | Microsoft Docs
description: Klik hier voor meer informatie over het oplossen van algemene problemen met toepassingen voor Azure AD-toepassingsproxy.
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: b9a643dab98fa685bb4873705c177a834af831ce
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="cant-access-this-corporate-application-error-when-using-an-application-proxy-application"></a>"Geen toegang tot deze zakelijke toepassing" Fout bij het gebruik van een toepassing toepassingsproxy

In dit artikel helpt u bij het oplossen van veelvoorkomende problemen voor de fout 'deze zakelijke app kan niet worden geopend' op een Azure AD-toepassingsproxy-toepassing.

## <a name="overview"></a>Overzicht
Wanneer u deze fout ziet, moet u de statuscode vinden op de foutpagina weergegeven. Deze code is waarschijnlijk een van de volgende statuscodes:

-   **Time-out van gateway**: Er kan geen bereiken van de connector de toepassingsproxy-service. Deze fout wijst meestal op een probleem met de connector-toewijzing, connector zelf, of de netwerken regels rond de connector.

-   **Ongeldige Gateway**: de connector kan geen verbinding worden de back-end-toepassing is. Deze fout kan duiden op een onjuiste configuratie van de toepassing.

-   **Verboden**: de gebruiker is niet geautoriseerd voor toegang tot de toepassing. Deze fout kan optreden wanneer de gebruiker is niet toegewezen aan de toepassing in Azure Active Directory of als op de back-end van de gebruiker heeft geen machtiging voor toegang tot de toepassing.

Voor de code, zoekt u naar de tekst in de linkerbenedenhoek van het foutbericht voor het veld ' Status '. Kijk ook eventuele extra tips aan de onderkant van de pagina.

   ![De time-outfout gateway](./media/application-proxy/connection-problem.png)

Zie voor meer informatie over het oplossen van de hoofdoorzaak van deze fouten en meer informatie over de voorgestelde oplossingen, de overeenkomstige sectie hieronder.

## <a name="gateway-timeout-errors"></a>De time-outfouten gateway

Er is een time-out van gateway treedt op wanneer de service probeert te bereiken van de connector en kan niet binnen de time-venster. Deze fout wordt meestal veroorzaakt door een toepassing die is toegewezen aan de groep van een Connector met geen connectors werken of bepaalde poorten die nodig zijn door de Connector zijn niet openen.


## <a name="bad-gateway-errors"></a>Ongeldige Gateway-fouten

Een ongeldige gateway-fout geeft aan dat de connector kan geen verbinding worden de back-end-toepassing. Zorg ervoor dat u de juiste toepassing hebt gepubliceerd. Veelvoorkomende fouten die ervoor zorgen deze fout dat zijn:

-   Een typefout gemaakt of een fout in de interne URL

-   De hoofdmap van de toepassing publiceren niet. Bijvoorbeeld, publiceren <http://expenses/reimbursement> maar tijdens het toegang <http://expenses>

-   Problemen met de configuratie van de Kerberos-beperkt delegatie (KCD)

-   Problemen met de back-end-toepassing

## <a name="forbidden-errors"></a>Niet-toegestane fouten

Als u een niet-toegestane fout ziet, is de gebruiker niet is toegewezen aan de toepassing. Deze fout kan zijn in Azure Active Directory of op de back-end-toepassing.

Als u wilt weten hoe u gebruikers toewijzen aan de toepassing in Azure, Zie de [configuratiedocumentatie](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal#add-a-test-user).

Als u bevestigen dat de gebruiker is toegewezen aan de toepassing in Azure, controleert u de Gebruikersconfiguratie van de in de back-end-toepassing. Als u Kerberos-beperkte overdracht/geïntegreerde Windows-verificatie gebruikt, raadpleegt u de pagina KCD oplossen voor richtlijnen.

## <a name="check-the-applications-internal-url"></a>Interne URL van de toepassing controleren

Als een eerste stap van de snelle een extra controle uit en los van de interne URL door het openen van de toepassing via **bedrijfstoepassingen**, selecteren de **toepassingsproxy** menu. Controleer of dat de interne URL van uw on-premises netwerk gebruikt voor toegang tot de toepassing is.

## <a name="check-the-application-is-assigned-to-a-working-connector-group"></a>Controleer dat de toepassing is toegewezen aan een werkende Connector groep

Om te controleren of de toepassing wordt toegewezen aan een werkende Connector groep:

1.  De toepassing in de portal openen door te gaan naar **Azure Active Directory**, te klikken op **bedrijfstoepassingen**, klikt u vervolgens **alle toepassingen.** Open de toepassing en selecteer vervolgens **toepassingsproxy** in het menu links.

2.  Bekijk het groepsveld van Connector. Als er geen actieve connectors in de groep, ziet u een waarschuwing. Als er geen waarschuwingen, verplaatsen bij 'Controleer of alle vereiste poorten zijn goedgekeurde lijst'.

3.  Als de verkeerde groep Connector wordt weergegeven, gebruikt u de vervolgkeuzelijst selecteert u de juiste groep en Controleer eventuele waarschuwingen niet meer weergegeven. Als de beoogde Connector-groep wordt weergegeven, klikt u op de waarschuwing voor het openen van de pagina met Connector management.

4.  Hier kunt zijn er een aantal manieren om in te zoomen meer:

  * Een actieve Connector verplaatsen naar de groep: als er een actieve Connector die moet deel uitmaken van deze groep en regel zicht naar de back-end-doeltoepassing heeft, kunt u de Connector verplaatsen naar de groep toegewezen. Om dit te doen, klikt u op de Connector. Gebruik de vervolgkeuzelijst selecteert u de juiste groep en klik op opslaan in het veld 'Connector groep'.

  * Download een nieuwe Connector voor de groep: op deze pagina kunt u Haal de koppeling naar [downloaden van een nieuwe Connector](https://download.msappproxy.net/Subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/Connector/Download). De Connector installeren op een machine met directe regel zicht naar de back-end-toepassing. Typicall, de Connector is geïnstalleerd op dezelfde server als de toepassing. Gebruik het downloaden van de Connector-koppeling voor het downloaden van een connector op de doelcomputer. Vervolgens klikt u op de Connector en de 'Connector groep gebruiken ' vervolgkeuzelijst om ervoor te zorgen dat deze aan de juiste groep behoort.

  * Een inactieve Connector onderzoeken: als u een connector als inactief ziet, is het niet bereiken van de service. Deze fout wordt meestal veroorzaakt door een aantal vereiste poorten wordt geblokkeerd. U lost dit probleem, verplaatsen naar "Controleer of alle vereiste poorten zijn wilt plaatsen."

Nadat u test deze stappen om te controleren of de toepassing is toegewezen aan een groep met Connectors, werkt de toepassing opnieuw. Als deze nog steeds niet werkt, gaat u verder naar de volgende sectie.

## <a name="check-all-required-ports-are-whitelisted"></a>Controleer alle vereiste poorten zijn goedgekeurde lijst

Om te controleren of alle vereiste poorten zijn geopend, Zie de documentatie over het openen van poorten. Als de vereiste poorten geopend zijn, verplaatsen naar de volgende sectie.

## <a name="check-for-other-connector-errors"></a>Controleer ook op andere fouten Connector

Als geen van de bovenstaande het probleem is opgelost, wordt de volgende stap is op zoek naar problemen of fouten met de Connector zelf. Ziet u een aantal veelvoorkomende fouten in de [oplossen document](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#connector-errors). 

U kunt ook zoeken rechtstreeks op de logboeken van de Connector eventuele fouten te identificeren. Veel van de foutberichten delen specifieke aanbevelingen voor oplossingen. Zie de logboeken om de [connectors documentatie](manage-apps/application-proxy-connectors.md#under-the-hood).

## <a name="additional-resolutions"></a>Aanvullende oplossingen

Als u de bovenstaande het probleem niet oplossen, moet u er een paar verschillende mogelijke oorzaken zijn. Het probleem identificeren:

Als uw toepassing is geconfigureerd voor gebruik van geïntegreerde Windows-verificatie (IWA), test u de toepassing zonder eenmalige aanmelding. Als dat niet het geval is, wordt verplaatst naar de volgende alinea. Als u wilt controleren van de toepassing zonder eenmalige aanmelding, opent u uw toepassing via **bedrijfstoepassingen,** en Ga naar de **Single Sign-On** menu. Wijzig de vervolgkeuzelijst van geïntegreerde Windows-verificatie' in "Azure AD single sign-on uitgeschakeld". 

Nu open een browser en probeert te krijgen tot de toepassing opnieuw. U moet worden gevraagd om verificatie en toegang tot de toepassing. Als u geverifieerd bent, wordt het probleem is met de configuratie van Kerberos-beperkt delegatie (KCD) waarmee de eenmalige aanmelding. Zie de pagina KCD oplossen voor meer informatie.

Als u doorgaat naar de fout te zien, gaat u naar de machine waarop de Connector is geïnstalleerd, open een browser en probeert te krijgen tot de interne URL voor de toepassing gebruikt. De Connector fungeert als een andere client door dezelfde machine. Als u de toepassing niet kan bereiken, moet u onderzoeken waarom dat de machine is niet bereiken van de toepassing of gebruiken van een connector op een server die toegang tot de toepassing is.

Als u de toepassing van deze machine op zoek naar problemen of fouten met de Connector zelf bereiken kunt. Ziet u een aantal veelvoorkomende fouten in de [oplossen document](active-directory-application-proxy-troubleshoot.md#connector-errors). U kunt ook zoeken rechtstreeks op de logboeken van de Connector eventuele fouten te identificeren. Veel van onze foutberichten mogelijk meer specifieke aanbevelingen voor oplossingen delen. Zie voor meer informatie over het weergeven van de logboeken, [onze documentatie connectors](manage-apps/application-proxy-connectors.md#under-the-hood).

## <a name="next-steps"></a>Volgende stappen
[Azure AD-toepassingsproxy connectors begrijpen](manage-apps/application-proxy-connectors.md)
