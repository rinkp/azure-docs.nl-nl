---
title: Veelgestelde vragen over Azure IoT-oplossing accelerators | Microsoft Docs
description: Veelgestelde vragen over IoT-oplossing accelerators
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: fcaf0c2d4a8be1358e4510524c8b15e063c647bf
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="frequently-asked-questions-for-iot-solution-accelerators"></a>Veelgestelde vragen over IoT-oplossing accelerators

Zie ook de [verbonden Factory-specifieke Veelgestelde vragen over](iot-suite-faq-cf.md) en de [remote monitoring-specifieke Veelgestelde vragen over](iot-suite-faq-rm-v2.md) .

### <a name="where-can-i-find-the-source-code-for-the-solution-accelerators"></a>Waar vind ik de broncode voor de oplossing accelerators

De broncode is opgeslagen in de volgende GitHub-opslagplaatsen:

* [Externe controle oplossingsverbetering (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Externe controle oplossingsverbetering (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)
* [Voorspeld onderhoud oplossingsverbetering](https://github.com/Azure/azure-iot-predictive-maintenance)
* [Verbonden Factory oplossingsverbetering](https://github.com/Azure/azure-iot-connected-factory)

### <a name="what-sdks-can-i-use-to-develop-device-clients-for-the-solution-accelerators"></a>Welke SDK's kan ik gebruiken voor het ontwikkelen van apparaatclients voor de oplossing accelerators?

Vindt u koppelingen naar het apparaat in andere taal (C, .NET, Java, Node.js, Python) IoT SDK's in de [Microsoft Azure IoT SDK's](https://github.com/Azure/azure-iot-sdks) GitHub-opslagplaatsen.

Als u van het apparaat DevKit gebruikmaakt, vindt u bronnen en voorbeelden in de [IoT DevKit SDK](https://github.com/Microsoft/devkit-sdk) GitHub-opslagplaats.

### <a name="is-the-new-microservices-architecture-available-for-all-the-three-solution-accelerators"></a>Is de nieuwe microservices architectuur beschikbaar voor alle drie oplossing-accelerators?

Alleen de oplossing voor externe controle wordt op dit moment wordt de architectuur microservices gebruikt als ze het breedste scenario omvatten.

### <a name="what-advantages-does-the-new-open-sourced-microservices-based-architecture-provide-in-the-new-update"></a>Welke voordelen biedt de nieuwe open-source microservices gebaseerde architectuur in de nieuwe update?

Architectuur van de afgelopen twee jaar aanzienlijk zich heeft ontwikkeld. Microservices is ontstaan als een geweldige patroon voor het bereiken van de schaal en flexibiliteit, zonder verlies van ontwikkelingssnelheid. Deze architectuur patroon wordt gebruikt in verschillende Microsoft-services intern met betrouwbaar en schaalbaarheid resultaten. We wilt deze in de praktijk learning zodat onze klanten van deze profiteren opslaan.

### <a name="is-the-new-solution-accelerator-available-in-the-same-geographic-region-as-the-existing-solution"></a>De nieuwe oplossingsverbetering is beschikbaar in dezelfde geografische regio als de bestaande oplossing?

Ja, de nieuwe externe controle is beschikbaar in dezelfde geografische regio's.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-solution-accelerator-in-azureiotsuitecom"></a>Wat is het verschil tussen het verwijderen van een resourcegroep in de Azure portal en te klikken op verwijderen op een oplossingsverbetering op azureiotsuite.com?

* Als u verwijdert de oplossingsverbetering in [azureiotsuite.com](https://www.azureiotsuite.com/), verwijdert u alle resources die zijn ingericht toen u de oplossingsverbetering gemaakt. Als u aanvullende bronnen toegevoegd aan de resourcegroep, wordt deze resources worden ook verwijderd.
* Als u de resourcegroep in verwijdert de [Azure-portal](https://portal.azure.com), verwijdert u alleen de resources in die resourcegroep. U moet ook de Azure Active Directory-toepassing die is gekoppeld aan de oplossingsverbetering verwijderen.

### <a name="can-i-continue-to-leverage-my-existing-investments-in-azure-iot-solution-accelerators"></a>Kan ik mijn bestaande investeringen in Azure IoT-oplossing accelerators gebruiken doorgaan?

Ja. Een oplossing die tegenwoordig blijft werken in uw Azure-abonnement en de broncode blijft beschikbaar in GitHub.

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Hoeveel exemplaren van de IoT Hub kan ik inrichten in een abonnement?

Standaard kunt u inrichten [10 IoT hubs per abonnement](../azure-subscription-service-limits.md#iot-hub-limits). Kunt u een [Azure-ondersteuningsticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) deze limiet te verhogen. Als gevolg hiervan, kunt omdat elke oplossing accelerator voorziet in een nieuwe IoT Hub, u alleen inrichten maximaal 10 oplossing accelerators in een bepaald abonnement.

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Hoeveel exemplaren van Azure DB die Cosmos kan ik inrichten in een abonnement?

Vijftig. Kunt u een [Azure-ondersteuningsticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) deze limiet te verhogen, maar standaard kunt u alleen 50 exemplaren van de Cosmos-database per abonnement inrichten.

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Hoeveel gratis Bing Kaarten-API's kan ik inrichten met een abonnement?

Twee. U kunt slechts twee interne transacties niveau 1 Bing-kaarten voor Enterprise plannen in een Azure-abonnement maken. De oplossing voor externe controle is standaard ingericht met het interne transacties niveau 1-plan. Als gevolg hiervan kunt u met een abonnement waaraan geen wijzigingen zijn aangebracht maximaal twee externe bewakingsoplossingen inrichten.

### <a name="can-i-create-a-solution-accelerator-if-i-have-microsoft-azure-for-dreamspark"></a>Kan ik een oplossingsverbetering maken als ik Microsoft Azure voor DreamSpark heb?

> [!NOTE]
> Microsoft Azure voor DreamSpark is nu bekend als Microsoft Imagine voor studenten.

Op dit moment kunt u een oplossingsverbetering met geen maken een [Microsoft Azure voor DreamSpark](https://azure.microsoft.com/pricing/member-offers/imagine/) account. U kunt echter maken een [gratis proefaccount voor Azure](https://azure.microsoft.com/free/) maken binnen een paar minuten waarmee u kunt een oplossingsverbetering.

### <a name="can-i-create-a-solution-accelerator-if-i-have-cloud-solution-provider-csp-subscription"></a>Kan ik een oplossingsverbetering maken als ik Cloud Solution Provider (CSP) abonnement?

U kunt op dit moment een oplossingsverbetering maken met een abonnement Cloud Solution Provider (CSP). U kunt echter maken een [gratis proefaccount voor Azure](https://azure.microsoft.com/free/) maken binnen een paar minuten waarmee u kunt een oplossingsverbetering.

### <a name="how-do-i-delete-an-aad-tenant"></a>Hoe verwijder ik een AAD-tenant?

Zie het blogbericht van Eric Golpe van [overzicht van het verwijderen van een Azure AD-Tenant](http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx).

### <a name="next-steps"></a>Volgende stappen

U kunt ook een aantal andere functies en mogelijkheden van de IoT-oplossing accelerators verkennen:

* [De mogelijkheden van de externe controle oplossingsverbetering verkennen](iot-suite-remote-monitoring-explore.md)
* [Overzicht van voorspeld onderhoud oplossing accelerator](iot-suite-predictive-overview.md)
* [Overzicht van verbonden Factory oplossing accelerator](iot-suite-connected-factory-overview.md)
* [Beveiliging van een compleet nieuwe IoT](securing-iot-ground-up.md)