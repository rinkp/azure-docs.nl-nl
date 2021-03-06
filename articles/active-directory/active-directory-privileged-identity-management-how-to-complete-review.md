---
title: Het uitvoeren van een onderzoek toegang | Microsoft Docs
description: Nadat u een revisie toegang in Azure AD Privileged Identity Management gestart, informatie over het voltooien en de resultaten bekijken
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.component: users-groups-roles
ms.date: 06/06/2017
ms.author: curtand
ms.custom: pim
ms.openlocfilehash: c1314724d8094f171e4c8468d854a9d4ae0c85d7
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-complete-an-access-review-in-azure-ad-privileged-identity-management"></a>Het uitvoeren van een revisie toegang in Azure AD Privileged Identity Management
Bevoorrechte rol beheerders kunnen bekijken voor bevoorrechte toegang eenmaal een [beveiligingsbeoordeling is gestart](active-directory-privileged-identity-management-how-to-start-security-review.md). Azure AD Privileged Identity Management (PIM) stuurt automatisch een e-mailbericht gebruikers om te controleren van de toegang te vragen. Als een gebruiker niet een e-mailbericht ontvangt is, kunt u ze de instructies in verzenden [het uitvoeren van een beveiligingsbeoordeling](active-directory-privileged-identity-management-how-to-perform-security-review.md).

Nadat de periode van beveiliging controleren of alle gebruikers klaar bent met hun eigen bekijken, de stappen in dit artikel voor het beheren van de controle en de resultaten te zien.

## <a name="manage-security-reviews"></a>Beoordelingen van beveiliging beheren
1. Ga naar de [Azure-portal](https://portal.azure.com/) en selecteer de **Azure AD Privileged Identity Management** toepassing op uw dashboard.
2. Selecteer de **toegang tot beoordelingen** sectie van het dashboard.
3. Selecteer de controle van toegang dat u wilt beheren.

Op de toegang-revisie detail blade zijn er een aantal opties voor het beheren van deze evaluatie.

![PIM toegang controleren knoppen - schermafbeelding][1]

### <a name="remind"></a>Herinneren
Als een onderzoek toegang is ingesteld, zodat de gebruikers zelf, Controleer de **herinnering sturen** knop verstuurt een melding. 

### <a name="stop"></a>Stoppen
Alle beoordelingen van toegang tot een einddatum hebben, maar u kunt de **stoppen** knop vroeg voltooid. Als gebruikers zich nog niet op dit tijdstip zijn gecontroleerd, ze niet mogelijk om te na het stoppen van de controle. U kunt een beoordeling niet opnieuw starten nadat deze gestopt.

### <a name="apply"></a>Toepassen
Nadat een revisie access is voltooid, omdat u de einddatum is bereikt of handmatig gestopt omdat de **toepassen** knop implementeert de uitkomst van de evaluatie. Als een gebruiker toegang is geweigerd in de evaluatie, is dit de stap die hun roltoewijzing wordt verwijderd.  

### <a name="export"></a>Exporteren
Als u de resultaten van de beveiligingsbeoordeling handmatig toepassen wilt, kunt u de controle exporteren. De **exporteren** knop downloaden van een CSV-bestand wordt gestart. U kunt de resultaten in Excel of andere programma's die CSV-bestanden beheren.

### <a name="delete"></a>Verwijderen
Als u niet geïnteresseerd in de revisie verdere bent, verwijderen. De **verwijderen** knop verwijdert u de controle van de PIM-toepassing.

> [!IMPORTANT]
> U wordt niet ophalen van een waarschuwing alvorens deze wordt verwijderd, dus wees zeker dat u wilt verwijderen van deze evaluatie. 

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
