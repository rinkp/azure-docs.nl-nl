---
title: Extern delen van Office 365 en Azure Active Directory B2B-samenwerking | Microsoft Docs
description: Komen aan bod die resources delen met externe partners met behulp van O365 en Azure Active Directory B2B-samenwerking.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/24/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: d55d587022b763a6890c098dd0a1b9bef2a3b7fb
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a>Extern delen van Office 365 en Azure Active Directory B2B-samenwerking

Extern delen in Office 365 (OneDrive, SharePoint Online, uniforme, groepen, enzovoort) en Azure Active Directory (Azure AD) B2B-samenwerking zijn technisch hetzelfde. Alle extern delen (met uitzondering van OneDrive/SharePoint Online), inclusief gasten in Office 365-groepen, wordt de Azure AD B2B-samenwerking uitnodiging API's al gebruikt voor het delen.

OneDrive/SharePoint Online heeft een uitnodiging voor een afzonderlijke-manager. Ondersteuning voor extern delen in OneDrive/SharePoint Online gestart voordat de ondersteuning die Azure AD zijn ontwikkeld. Na verloop van tijd OneDrive/SharePoint Online extern delen, is enkele functies samengevoegd en veel miljoenen gebruikers die gebruikmaken van het product van de ingebouwde patroon delen. Er zijn echter enkele subtiele verschillen tussen hoe extern delen OneDrive/SharePoint Online werkt en de werking van Azure AD B2B-samenwerking:

- OneDrive/SharePoint Online gebruikers toegevoegd aan de map nadat gebruikers hebben hun uitnodigingen ingewisseld. Dus voordat inwisseling er geen de gebruiker in Azure AD-portal. Als een andere site in de tussentijd nodigt van een gebruiker, wordt een nieuwe uitnodiging gegenereerd. Wanneer u Azure AD B2B-samenwerking, worden gebruikers echter toegevoegd onmiddellijk op uitnodiging zodat ze overal worden weergegeven.

- De ervaring van de inwisselcode in OneDrive/SharePoint Online er anders uit de ervaring in Azure AD B2B-samenwerking. Nadat een gebruiker een uitnodiging wordt ingewisseld, worden de ervaringen lijken.

- Azure AD B2B-samenwerking uitgenodigd gebruikers kunnen worden verzameld van OneDrive/SharePoint Online-dialoogvensters voor delen. OneDrive/SharePoint Online uitgenodigd gebruikers ook weergegeven in Azure AD wanneer ze hun uitnodigingen inwisselen.

- Voor het beheren van extern delen in OneDrive/SharePoint Online met Azure AD B2B-samenwerking ingesteld OneDrive/SharePoint Online extern delen instelling **dat alleen delen met externe gebruikers al in de map**. Gebruikers kunnen gaat u naar extern gedeelde sites en kiest uit externe deelnemers die de beheerder heeft toegevoegd. De beheerder kan de externe deelnemers via de B2B-samenwerking uitnodiging API's kunt toevoegen.

![De OneDrive/SharePoint Online extern delen van de instelling](media/o365-external-user/odsp-sharing-setting.png)

## <a name="next-steps"></a>Volgende stappen

* [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
* [Een B2B-samenwerking-gebruiker toe te voegen aan een rol](add-guest-to-role.md)
* [B2B-samenwerking uitnodigingen delegeren](delegate-invitations.md)
* [Dynamische groepen en B2B-samenwerking](use-dynamic-groups.md)