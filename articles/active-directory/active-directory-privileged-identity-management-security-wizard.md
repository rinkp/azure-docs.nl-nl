---
title: De beveiligingswizard voor Azure AD Privileged Identity Management
description: De eerste keer dat u de extensie Azure Active Directory Privileged Identity Management, u krijgt een beveiligingswizard. In dit artikel beschrijft de stappen voor het gebruik van de wizard.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.component: users-groups-roles
ms.date: 02/27/2017
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: 4b3856d74b1109b20a1ff9f93b76ee36b66ee312
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="using-the-security-wizard-in-azure-ad-privileged-identity-management"></a>Gebruik de beveiligingswizard in Azure AD Privileged Identity Management 
Als u de eerste persoon Azure Privileged Identity Management (PIM) uitvoeren voor uw organisatie, u krijgt een wizard. De wizard helpt u het beveiligingsrisico van bevoegde identiteiten en hoe u PIM gebruikt om te beperken die risico's begrijpen. U hoeft niet te Breng wijzigingen in bestaande roltoewijzingen in de wizard als u liever dit later doen.

## <a name="what-to-expect"></a>Wat u kunt verwachten
Voordat uw organisatie wordt gestart met behulp van PIM, alle roltoewijzingen permanent zijn: de gebruikers zich altijd in deze rollen zelfs als ze geen momenteel hun bevoegdheden nodig.  De eerste stap van de wizard ziet u een lijst met verhoogde rollen en hoeveel gebruikers zijn momenteel in deze rollen. U kunt inzoomen een bepaalde rol voor meer informatie over gebruikers als een of meer van deze niet bekend bent.

De tweede stap van de wizard hebt u de mogelijkheid om te wijzigen van roltoewijzingen van beheerder.  

> [!WARNING]
> Het is belangrijk dat u ten minste één globale beheerder en meer dan een beheerder met bevoorrechte rol met een organisatieaccount (dus niet door een Microsoft-account hebt). Als er slechts één beheerder met bevoorrechte rol, kunnen de organisatie zich niet PIM beheren als dit account wordt verwijderd.
> Houd er ook roltoewijzingen permanente als een gebruiker heeft een Microsoft-account (een account dat ze gebruiken om aan te melden bij Microsoft-services zoals Skype en Outlook.com). Als u van plan bent MFA voor activering voor die rol is vereist, wordt die gebruiker vergrendeld.
> 
> 

Nadat u wijzigingen hebt aangebracht, wordt de wizard niet meer weergegeven. De volgende keer dat u of een andere beheerder met bevoorrechte rol gebruiken PIM, ziet u de PIM-dashboard.  

* Als u wilt toevoegen of verwijderen van gebruikers uit functies of wijzig de toewijzingen van permanente naar in aanmerking komende, leest u meer bij [toevoegen of verwijderen van de rol van een gebruiker](active-directory-privileged-identity-management-how-to-add-role-to-user.md).
* Als u wilt meer gebruikers om toegang te geven voor het beheren van PIM, meer informatie op [geeft toegang tot het beheer in PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

