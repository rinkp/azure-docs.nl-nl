---
title: Een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps| Microsoft Docs
description: Informatie over het toevoegen van een bestaande aangepaste DNS-domeinnaam (vanity-domein) aan een web-app, back-end voor een mobiele app of een API-app in Azure App Service.
keywords: app service, azure app service, domeintoewijzing, domeinnaam, bestaand domein, hostnaam
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: ''
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/23/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 431268082b24d23289188f5422cd596dc5f37d30
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="tutorial-map-an-existing-custom-dns-name-to-azure-web-apps"></a>Zelfstudie: Een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps

[Azure Web Apps](app-service-web-overview.md) biedt een uiterst schaalbare webhostingservice met self-patchfunctie. Deze zelfstudie toont u hoe u een bestaande aangepaste DNS-naam toewijst aan Azure Web Apps.

![Navigatie naar Azure-app in de portal](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een subdomein toewijzen (bijvoorbeeld `www.contoso.com`) met behulp van een CNAME-record
> * Een hoofddomein toewijzen (bijvoorbeeld `contoso.com`) met behulp van een A-record
> * Een wildcard-domein toewijzen (bijvoorbeeld `*.contoso.com`) met behulp van een CNAME-record
> * Domeintoewijzing met scripts automatiseren

U kunt ofwel een **CNAME-record** of een **A-record** gebruiken voor het toewijzen van een aangepaste DNS-naam aan App Service. 

> [!NOTE]
> Het is raadzaam dat u een CNAME voor alle aangepaste DNS-namen gebruikt, behalve een hoofddomein (bijvoorbeeld `contoso.com`).

Zie voor het migreren van een live site en de DNS-domeinnaam naar App Service, [Een actieve DNS-naam migreren naar Azure App Service](app-service-custom-domain-name-migrate.md).

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

* [Maak een App Service-app](/azure/app-service/), of gebruik een app die u hebt gemaakt voor een andere zelfstudie.
* Koop een domeinnaam en controleer of u toegang hebt tot het DNS-register voor uw domeinprovider (zoals GoDaddy).

  Als u bijvoorbeeld DNS-vermeldingen voor `contoso.com` en `www.contoso.com` wilt toevoegen, moet u de DNS-instellingen voor het hoofddomein van `contoso.com` kunnen configureren.

  > [!NOTE]
  > Als u geen bestaande domeinnaam heeft, denk dan na over het [aanschaffen van een domein met de Azure portal](custom-dns-web-site-buydomains-web-app.md). 

## <a name="prepare-the-app"></a>De app voorbereiden

Om een aangepaste DNS-naam toe te wijzen aan een web-app, moet het [App Service-plan](https://azure.microsoft.com/pricing/details/app-service/) van de web-app een betaalde categorie zijn (**Shared**, **Basic**, **Standard** of  **Premium**). In deze stap zorgt u ervoor dat de App Service-app zich in de ondersteunde prijscategorie bevindt.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Open de [Azure-portal](https://portal.azure.com) en meld u aan met uw Azure-account.

### <a name="navigate-to-the-app-in-the-azure-portal"></a>Navigeer naar de app in de Azure portal

Selecteer in het linkermenu **App Services** en selecteer de naam van uw app.

![Navigatie naar Azure-app in de portal](./media/app-service-web-tutorial-custom-domain/select-app.png)

U ziet de beheerpagina van de App Service-app.  

<a name="checkpricing"></a>

### <a name="check-the-pricing-tier"></a>Controleer de prijscategorie

Schuif in de navigatiebalk links van de app-pagina naar de sectie **Instellingen** en selecteer **Opschalen (App Service-plan)**.

![Menu Opschalen](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

De huidige laag van de app wordt gemarkeerd door een blauwe rand. Controleer of de app zich niet in de **Gratis**-categorie bevindt. Aangepaste DNS wordt niet ondersteund in de **Gratis**-categorie. 

![Controleer prijscategorie](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Als het App Service-plan niet **Gratis** is, sluit dan de pagina **Uw prijscategorie kiezen** en ga door met [Een CNAME-record toewijzen](#cname).

<a name="scaleup"></a>

### <a name="scale-up-the-app-service-plan"></a>Het App Service-plan opschalen

Selecteer een van de betaalde categorieën (**Shared**, **Basic**, **Standard**, of **Premium**). 

Klik op **Selecteren**.

![Controleer prijscategorie](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Wanneer u de volgende melding ziet, is de schaalbewerking voltooid.

![Schaalbewerking bevestigen](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<a name="cname"></a>

## <a name="map-a-cname-record"></a>Een CNAME-record toewijzen

In het voorbeeld van de zelfstudie voegt u toe een CNAME-record toe voor het subdomein `www` (bijvoorbeeld `www.contoso.com`).

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-cname-record"></a>De CNAME-record maken

Voeg een CNAME-record toe om een subdomein toe te wijzen aan de standaard hostnaam van de app (`<app_name>.azurewebsites.net`, waarbij `<app_name>` de naam is van uw app).

Voeg voor het voorbeelddomein `www.contoso.com` een CNAME-record, dat is toegewezen aan de naam `www`, toe aan `<app_name>.azurewebsites.net`.

Nadat u de CNAME toevoegt, lijkt de pagina DNS-records op het volgende voorbeeld:

![Navigatie naar Azure-app in de portal](./media/app-service-web-tutorial-custom-domain/cname-record.png)

### <a name="enable-the-cname-record-mapping-in-azure"></a>De toewijzing van het CNAME-record in Azure inschakelen

Selecteer in het linkernavigatievenster van de app-pagina in de Azure portal **Aangepaste domeinen**. 

![Menu voor aangepaste domeinen](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Voeg op de pagina **Aangepaste domeinen** van de app de volledig gekwalificeerde aangepaste DNS-naam (`www.contoso.com`) toe aan de lijst.

Selecteer het pictogram **+** naast **Hostnaam toevoegen**.

![Hostnaam toevoegen](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Typ de volledig gekwalificeerde domeinnaam in waarvoor u een CNAME-record zoals `www.contoso.com` heeft toegevoegd. 

Selecteer **Valideren**.

De knop **Hostnaam toevoegen** wordt geactiveerd. 

Zorg ervoor dat **Hostnaam recordtype** is ingesteld op **CNAME (www.example.com of elk subdomein)**.

Selecteer **Hostnaam toevoegen**.

![DNS-naam toevoegen aan de app](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Het kan even duren voor de nieuwe hostnaam wordt weergegeven op de pagina **Aangepaste domeinen** van de app. Vernieuw de browser voor om de gegevens bij te werken.

![CNAME-record toegevoegd](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

Als u een stap hebt gemist of eerder ergens een typefout hebt gemaakt, ziet u een verificatie-foutmelding aan de onderkant van de pagina.

![Verificatiefout](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

<a name="a"></a>

## <a name="map-an-a-record"></a>Toewijzen van een A-record

In het voorbeeld van de zelfstudie voegt u toe een A-record toe voor het hoofddomein (bijvoorbeeld `contoso.com`). 

<a name="info"></a>

### <a name="copy-the-apps-ip-address"></a>Het IP-adres van de app kopiëren

Als u een A-record wilt toewijzen, heeft u het externe IP-adres van de app nodig. U vindt dit IP-adres op pagina **Aangepaste domeinen** van de app in de Azure-portal.

Selecteer in het linkernavigatievenster van de app-pagina in de Azure portal **Aangepaste domeinen**. 

![Menu voor aangepaste domeinen](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Op de pagina **Aangepaste domeinen** kopieert u het IP-adres van de app.

![Navigatie naar Azure-app in de portal](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-a-record"></a>Een A-record maken

Als u een A-record wilt toewijzen aan een app, vereist App Service **twee** DNS-records:

- Een **A**-record toewijzen aan het IP-adres van de app.
- Een **TXT**-record toewijzen aan de standaardhostnaam `<app_name>.azurewebsites.net` van de app. App Service gebruikt dit record alleen tijdens de configuratie, om te controleren of u de eigenaar bent van het aangepaste domein. Nadat uw aangepaste domein is gevalideerd en geconfigureerd in App Service, kunt u dit TXT-record verwijderen. 

Maak voor het domeinvoorbeeld `contoso.com` de A- en TXT-records overeenkomstig de volgende tabel (`@` vertegenwoordigt doorgaans het hoofddomein). 

| Recordtype | Host | Waarde |
| - | - | - |
| A | `@` | IP-adres uit [Het IP-adres van de app kopiëren](#info) |
| TXT | `@` | `<app_name>.azurewebsites.net` |

Wanneer de records worden toegevoegd, lijkt de pagina met DNS-records op het volgende voorbeeld:

![Pagina met DNS-records](./media/app-service-web-tutorial-custom-domain/a-record.png)

<a name="enable-a"></a>

### <a name="enable-the-a-record-mapping-in-the-app"></a>De toewijzing van de A-record in de app inschakelen

Voeg op de pagina **Aangepaste domeinen** van de app in de Azure portal de volledig gekwalificeerde aangepaste DNS-naam (bijvoorbeeld `contoso.com`) toe aan de lijst.

Selecteer het pictogram **+** naast **Hostnaam toevoegen**.

![Hostnaam toevoegen](./media/app-service-web-tutorial-custom-domain/add-host-name.png)

Typ de volledig gekwalificeerde domeinnaam in waarvoor u een A-record zoals `contoso.com` heeft toegevoegd.

Selecteer **Valideren**.

De knop **Hostnaam toevoegen** wordt geactiveerd. 

Zorg ervoor dat **Hostnaam recordtype** is ingesteld op **A-record (voorbeeld.com)**.

Selecteer **Hostnaam toevoegen**.

![DNS-naam toevoegen aan de app](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

Het kan even duren voor de nieuwe hostnaam wordt weergegeven op de pagina **Aangepaste domeinen** van de app. Vernieuw de browser voor om de gegevens bij te werken.

![A-record toegevoegd](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

Als u een stap hebt gemist of eerder ergens een typefout hebt gemaakt, ziet u een verificatie-foutmelding aan de onderkant van de pagina.

![Verificatiefout](./media/app-service-web-tutorial-custom-domain/verification-error.png)

<a name="wildcard"></a>

## <a name="map-a-wildcard-domain"></a>Een wildcard-domein toewijzen

In het voorbeeld van de zelfstudie, wijst u een [wildcard-DNS-naam](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (bijvoorbeeld `*.contoso.com`) toe aan de App Service-app door een CNAME-record toe te voegen. 

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-cname-record"></a>Het CNAME-record maken

Voeg een CNAME-record toe om een wildcard-naam toe te wijzen aan de standaard hostnaam van de app (`<app_name>.azurewebsites.net`).

Voor het `*.contoso.com` domeinvoorbeeld zal het CNAME-record de naam `*` toewijzen naar `<app_name>.azurewebsites.net`.

Wanneer de CNAME wordt toegevoegd, lijkt de pagina met DNS-records op het volgende voorbeeld:

![Navigatie naar Azure-app in de portal](./media/app-service-web-tutorial-custom-domain/cname-record-wildcard.png)

### <a name="enable-the-cname-record-mapping-in-the-app"></a>De toewijzing van het CNAME-record in de app inschakelen

U kunt nu elk subdomein dat overeenkomt met de wildcard-naam aan de app toevoegen (bijvoorbeeld `sub1.contoso.com` en `sub2.contoso.com` komen overeen met `*.contoso.com`). 

Selecteer in het linkernavigatievenster van de app-pagina in de Azure portal **Aangepaste domeinen**. 

![Menu voor aangepaste domeinen](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Selecteer het pictogram **+** naast **Hostnaam toevoegen**.

![Hostnaam toevoegen](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Typ een FQDN-naam die overeenkomt met het wildcard-domein (bijvoorbeeld `sub1.contoso.com`), en selecteer vervolgens **Valideren**.

De knop **Hostnaam toevoegen** wordt geactiveerd. 

Zorg ervoor dat **Hostnaam recordtype** is ingesteld op **CNAME-record (www.example.com of elk subdomein)**.

Selecteer **Hostnaam toevoegen**.

![DNS-naam toevoegen aan de app](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname-wildcard.png)

Het kan even duren voor de nieuwe hostnaam wordt weergegeven op de pagina **Aangepaste domeinen** van de app. Vernieuw de browser voor om de gegevens bij te werken.

Selecteer opnieuw het pictogram **+** om een andere hostnaam toe te voegen die overeenkomt met het wildcard-domein. Bijvoorbeeld: voeg `sub2.contoso.com` toe.

![CNAME-record toegevoegd](./media/app-service-web-tutorial-custom-domain/cname-record-added-wildcard2.png)

## <a name="test-in-browser"></a>Testen in browser

Blader naar de DNS-namen die u eerder hebt geconfigureerd (bijvoorbeeld `contoso.com`, `www.contoso.com`, `sub1.contoso.com`, en `sub2.contoso.com`).

![Navigatie naar Azure-app in de portal](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

## <a name="resolve-404-error-web-site-not-found"></a>404-fout "Website niet gevonden" oplossen

Als u een HTTP 404 (niet gevonden)-fout ontvangt bij het bladeren naar de URL van uw aangepaste domein, moet u controleren of uw domein wordt omgezet naar uw app IP-adres met <a href="https://www.whatsmydns.net/" target="_blank">WhatsmyDNS.net</a>. Als dit niet gebeurt, kan dit een van de volgende oorzaken hebben:

- Bij het geconfigureerde aangepaste domein ontbreekt een A-record en/of een CNAME-record.
- De browserclient heeft het oude IP-adres van uw domein in de cache. Leeg de cache en test DNS-omzetting opnieuw. Op een Windows-computer, leegt u de cache met `ipconfig /flushdns`.

<a name="virtualdir"></a>

## <a name="direct-default-url-to-a-custom-directory"></a>Standaard-URL naar een aangepaste map omleiden

Standaard stuurt App Service webaanvragen naar de hoofdmap van uw app-code. Bepaalde web-frameworks starten echter niet in de hoofdmap. Bijvoorbeeld: [Laravel](https://laravel.com/) start in de submap `public`. Om door te gaan met het DNS-voorbeeld `contoso.com`, is een dergelijke app toegankelijk op `http://contoso.com/public`, maar u moet in plaats daarvan eigenlijk `http://contoso.com` naar de map `public` sturen. Deze stap heeft geen betrekking op DNS-omzetting, maar het aanpassen van de virtuele map.

Om dit te doen, selecteert u **Toepassingsinstellingen** in de linkernavigatiebalk van uw web-app-pagina. 

Aan de onderkant van de pagina verwijst de virtuele hoofdmap `/` standaard naar `site\wwwroot`. Dit is de hoofdmap van uw app-code. Wijzig deze om in plaats daarvan bijvoorbeeld te verwijzen naar de `site\wwwroot\public` en sla de wijzigingen op. 

![Virtuele map aanpassen](./media/app-service-web-tutorial-custom-domain/customize-virtual-directory.png)

Nadat de bewerking is voltooid, moet uw app de juiste pagina op het hoofdpad (bijvoorbeeld http://contoso.com)) retourneren.

## <a name="automate-with-scripts"></a>Automatiseren met scripts

U kunt het beheer van aangepaste domeinen met scripts automatiseren met behulp van de [Azure CLI](/cli/azure/install-azure-cli) of [Azure PowerShell](/powershell/azure/overview). 

### <a name="azure-cli"></a>Azure-CLI 

De volgende opdracht voegt een geconfigureerde aangepaste DNS-naam toe aan een App Service-app. 

```bash 
az webapp config hostname add \
    --webapp-name <app_name> \
    --resource-group <resource_group_name> \ 
    --hostname <fully_qualified_domain_name> 
``` 

Zie voor meer informatie [Een aangepast domein toewijzen aan een web-app](scripts/app-service-cli-configure-custom-domain.md). 

### <a name="azure-powershell"></a>Azure PowerShell 

De volgende opdracht voegt een geconfigureerde aangepaste DNS-naam toe aan een App Service-app. 

```PowerShell  
Set-AzureRmWebApp `
    -Name <app_name> `
    -ResourceGroupName <resource_group_name> ` 
    -HostNames @("<fully_qualified_domain_name>","<app_name>.azurewebsites.net") 
```

Zie voor meer informatie [Een aangepast domein toewijzen aan een web-app](scripts/app-service-powershell-configure-custom-domain.md).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een subdomein toewijzen met behulp van een CNAME-record
> * Een subdomein toewijzen met behulp van een A-record
> * Een wildcard-domein toewijzen met behulp van een CNAME-record
> * Domeintoewijzing met scripts automatiseren

Ga door naar de volgende zelfstudie om te leren hoe u een aangepast SSL-certificaat aan een web-app kunt toewijzen.

> [!div class="nextstepaction"]
> [Een bestaand aangepast SSL-certificaat binden aan Azure Web Apps](app-service-web-tutorial-custom-ssl.md)
