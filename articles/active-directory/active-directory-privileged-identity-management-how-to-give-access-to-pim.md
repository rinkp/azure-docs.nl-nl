---
title: Het toegang geven tot Privileged Identity Management - Azure | Microsoft Docs
description: Informatie over het toevoegen van functies voor gebruikers met de extensie Azure Active Directory Privileged Identity Management zodat ze PIM kunnen beheren.
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
ms.openlocfilehash: f7e08e35ce4575715a72b0880d038ce0db766b66
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="giving-access-to-manage-azure-ad-privileged-identity-management"></a>Geeft toegang tot het beheer van Azure AD Privileged Identity Management
De hoofdbeheerder die Azure AD Privileged Identity Management (PIM) voor een organisatie automatisch kunt roltoewijzingen en toegang tot PIM ophalen. Niemand anders opgehaald schrijftoegang standaard, met inbegrip van andere globale beheerders. Andere globale beheerders, Beveiligingsbeheerders en beveiliging lezers hebben alleen-lezen toegang tot Azure AD PIM. Als u wilt toegang geven tot PIM, de eerste gebruiker toewijzen anderen de **beheerder met bevoorrechte rol** rol. Deze toewijzing moet worden uitgevoerd in PIM zelf en kan niet worden gewijzigd via PowerShell of andere portals.

> [!NOTE]
> Het beheren van Azure AD PIM Azure MFA is vereist. Omdat Microsoft-accounts niet voor Azure MFA registreren, kan niet een gebruiker die zich met een Microsoft-account aanmeldt toegang tot de Azure AD PIM.
> 
> 

Zorg ervoor dat er altijd ten minste twee gebruikers in een beheerdersrol bevoorrechte rol als een gebruiker is vergrendeld of hun account is verwijderd.

## <a name="give-another-user-access-to-manage-pim"></a>Geef een andere gebruikerstoegang tot het beheer van PIM
1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) en selecteer de **Azure AD Privileged Identity Management** app op het dashboard.
2. Selecteer **bevoorrechte rollen beheren** > **beheerder met bevoorrechte rol** > **toevoegen**.
   
    ![Toevoegen van bevoorrechte rol administrators - schermafbeelding][1]
3. Stap 1 is al voltooid op de blade van de beheerde gebruikers toevoegen. Selecteer stap 2, **Selecteer gebruikers** en zoek naar de gebruiker die u wilt toevoegen.
   
    ![Selecteer de gebruikers - schermafbeelding][2]
4. Selecteer de gebruiker in de zoekresultaten en klik op **gedaan**.
5. Klik op **OK** opslaan van uw selectie. De gebruiker die u hebt geselecteerd, wordt weergegeven in de lijst met bevoorrechte rol beheerders.
   
   * Wanneer u een nieuwe rol aan iemand toewijst, worden ze automatisch ingesteld als in aanmerking komende om de rol te activeren. Als u wilt dat ze in de functie permanent te maken, klikt u op de gebruiker in de lijst. Selecteer **zorg toeg** in het menu van de gebruiker informatie.
6. Verzenden van de gebruiker een koppeling naar [aan de slag met Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).

## <a name="remove-another-users-access-rights-for-managing-pim"></a>Verwijder de toegangsrechten van een andere gebruiker voor het beheren van PIM
Voordat u uit de rol van bevoorrechte rol administrator verwijderen, zorg er altijd er wordt nog steeds twee gebruikers die zijn toegewezen.

1. Klik in het dashboard PIM op de rol **beheerder met bevoorrechte rol**.  De lijst met gebruikers die momenteel in die rol wordt weergegeven.
2. Klik op de gebruiker in de lijst met gebruikers.
3. Klik op **verwijderen**.  Een bevestigingsbericht wordt weergegeven.
4. Klik op **Ja** om de gebruiker verwijderen uit de rol.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
