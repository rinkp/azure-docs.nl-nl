---
title: Implementatiegids Selfservice voor wachtwoordherstel - Azure Active Directory
description: Tips voor een geslaagde implementatie van Azure AD-self-service voor wachtwoordherstel
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: get-started-article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: 4d3e07c6c395645ef34b1707f33a4e37a20bf05d
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="how-to-successfully-roll-out-self-service-password-reset"></a>Selfservice voor wachtwoordherstel implementeren

De meeste klanten voeren de volgende stappen uit voor een goede implementatie van de functionaliteit van de selfservice voor wachtwoordherstel (SSPR) van Azure Active directory (Azure AD):

> [!VIDEO https://www.youtube.com/embed/OZn5btP6ZXw]

1. [Schakel wachtwoordherstel in in uw directory](quickstart-sspr.md).
2. [Configureer on-premises Active Directory-machtigingen voor write-back van wachtwoord](howto-sspr-writeback.md#active-directory-permissions).
3. [Configureer write-back van wachtwoord](howto-sspr-writeback.md#configure-password-writeback) om wachtwoorden van Azure AD terug te schrijven naar uw on-premises directory.
4. [Wijs de vereiste licenties toe en verifieer ze](concept-sspr-licensing.md).
5. Bepaal of u een geleidelijke implementatie wilt uitvoeren. Als u selfservice voor wachtwoordherstel geleidelijk wilt implementeren, kunt u de toegang beperken tot bepaalde gebruikers, zodat u het programma met een specifieke groep kunt testen. Voor implementatie in een specifieke groep stelt u de schakeloptie **Selfservice voor wachtwoordherstel is ingeschakeld** in op **Geselecteerd** en selecteert u de beveiligingsgroep waarvoor u wachtwoordherstel mogelijk wilt maken.  Het nesten van beveiligingsgroepen wordt hier ondersteund.
6. Vul de [verificatiegegevens](howto-sspr-authenticationdata.md) in die uw gebruikers nodig hebben om zich te kunnen registreren, zoals hun zakelijke telefoonnummer, mobiele telefoonnummer en een alternatief e-mailadres.
7. [Pas de Azure Active Directory-aanmeldingservaring aan met de huisstijl van uw bedrijf](concept-sspr-customization.md).
8. Leer uw gebruikers hoe ze de selfservice voor wachtwoordherstel moeten gebruiken. Geef ze instructies voor hoe ze zich kunnen registreren en hun wachtwoord kunnen herstellen.
9. Bepaal of u de registratie verplicht wilt maken. U kunt ervoor kiezen registratie op elk gewenst moment af te dwingen. U kunt ook afdwingen dat gebruikers hun verificatiegegevens na een bepaalde periode opnieuw moeten bevestigen.
10. Gebruik de rapportagefunctionaliteit. Na verloop van tijd kunt u de registratie van gebruikers en het gebruik controleren met de [rapportagefunctionaliteit van Azure AD](howto-sspr-reporting.md).
11. Schakel wachtwoordherstel in. Wanneer u klaar bent, schakelt u wachtwoordherstel in voor alle gebruikers door de schakeloptie **Selfservice voor wachtwoordherstel is ingeschakeld** in te stellen op **Alle**. 

   > [!NOTE]
   > Als u deze optie wijzigt van een geselecteerde groep naar iedereen, worden bestaande authenticatiegegevens die een gebruiker als onderdeel van een testgroep heeft geregistreerd, niet ongeldig. Gebruikers die geconfigureerd zijn en geldige authenticatiegegevens geregistreerd hebben, blijven functioneren.

12. [Bied Windows 10-gebruikers de mogelijkheid tot wachtwoordherstel op het aanmeldingsscherm](tutorial-sspr-windows.md).

   > [!IMPORTANT]
   > Test de selfservice voor wachtwoordherstel als gebruiker en niet als beheerder, omdat Microsoft sterke verificatievereisten afdwingt voor Azure-accounts van beheerders. Zie ons [artikel over wachtwoordbeleid](concept-sspr-policy.md#administrator-password-policy-differences) voor meer informatie over het wachtwoordbeleid voor beheerders.

## <a name="email-based-rollout"></a>Implementatie op basis van e-mail

Veel klanten vinden een e-mailcampagne, met eenvoudig te gebruiken instructies, de eenvoudigste manier om gebruikers selfservice voor wachtwoordherstel te laten gebruiken. [We hebben drie eenvoudige e-mailberichten gemaakt die u als sjabloon kunt gebruiken om te helpen bij uw implementatie](https://www.microsoft.com/download/details.aspx?id=56768):

* **Binnenkort beschikbaar**: Een e-mailsjabloon die in de weken of dagen vóór implementatie moet worden gebruikt om gebruikers te laten weten dat ze iets moeten gaan doen.
* **Nu beschikbaar**: Een e-mailsjabloon die moet worden gebruikt op de dag van de lancering om gebruikers ertoe over te halen zich te registreren en hun verificatiegegevens te bevestigen. Als de gebruikers zich nu registreren, kunnen ze selfservice voor wachtwoordherstel gebruiken wanneer het nodig is.
* **Herinnering voor aanmelding**: Een e-mailsjabloon die enkele weken of dagen na de implementatie moet worden gebruikt om gebruikers eraan te herinneren dat ze zich moeten registreren en hun verificatiegegevens moeten bevestigen.

![E-mail][Email]

## <a name="create-your-own-password-portal"></a>Uw eigen wachtwoordportal maken

Veel klanten kiezen ervoor een webpagina te hosten en een DNS-basisvermelding te maken, zoalshttps://passwords.contoso.com. Ze vullen deze pagina met koppelingen naar de volgende informatie:

* [Azure AD-portal voor wachtwoordherstel - https://aka.ms/sspr](https://aka.ms/sspr)
* [Azure AD-registratieportal voor wachtwoordherstel https://aka.ms/ssprsetup](https://aka.ms/ssprsetup)
* [Azure AD-portal voor wachtwoordwijziging https://account.activedirectory.windowsazure.com/ChangePassword.aspx](https://account.activedirectory.windowsazure.com/ChangePassword.aspx)
* Overige organisatiespecifieke informatie

In e-mailberichten of flyers die u verstuurt, kunt u een in huisstijl opgemaakte, gemakkelijk te onthouden URL opnemen die gebruikers kunnen raadplegen wanneer ze de services moeten gebruiken. We hebben een [voorbeeldpagina voor wachtwoordherstel](https://github.com/ajamess/password-reset-page) gemaakt die u kunt gebruiken en aanpassen aan de behoeften van uw organisatie.

## <a name="step-by-step-deployment-plan"></a>Stapsgewijs implementatieplan

De Azure Active Directory-productgroep heeft een [stapsgewijs implementatieplan](https://aka.ms/SSPRDeploymentPlan) gemaakt, dat organisaties in combinatie met de documentatie kunnen gebruiken die te vinden is op deze site om een bedrijfsscenario te maken en de implementatie te plannen van selfservice voor wachtwoordherstel.

## <a name="use-enforced-registration"></a>Gedwongen registratie gebruiken

Als u wilt dat uw gebruikers zich registreren voor wachtwoordherstel, kunt u hen dwingen zich te registreren wanneer ze zich aanmelden via Azure AD. U kunt deze optie inschakelen via het deelvenster **Wachtwoordherstel** van uw directory door de optie **Vereisen dat gebruikers zich bij aanmelding registreren?** te kiezen op het tabblad **Registratie**.

Beheerders kunnen gebruikers dwingen zich opnieuw te registreren na een bepaalde periode. Ze kunnen de optie **Aantal dagen waarna gebruikers wordt gevraagd om de verificatiegegevens opnieuw te bevestigen** instellen op een waarde van 0 tot 730 dagen.

Nadat u deze optie hebt ingeschakeld, zien gebruikers bij aanmelding een bericht dat hen informeert dat de beheerder vereist dat ze hun verificatiegegevens controleren.

## <a name="populate-authentication-data"></a>Verificatiegegevens invullen

U moet [de verificatiegegevens voor uw gebruikers invullen](howto-sspr-authenticationdata.md). Op die manier hoeven gebruikers zich niet te registreren voor wachtwoordherstel voordat ze SSPR kunnen gebruiken. Zolang gebruikers de verificatiegegevens hebben gedefinieerd die voldoen aan het beleid voor wachtwoordherstel dat u hebt gedefinieerd, kunnen gebruikers hun wachtwoord opnieuw instellen.

## <a name="disable-self-service-password-reset"></a>Selfservice voor wachtwoordherstel uitschakelen

Selfservice voor wachtwoordherstel kan eenvoudig worden uitgeschakeld. Open uw Azure AD-tenant en ga naar **Wachtwoord opnieuw instellen** > **Eigenschappen** en selecteer vervolgens **Geen** onder **Selfservice voor wachtwoord opnieuw instellen is ingeschakeld**.

## <a name="next-steps"></a>Volgende stappen

* [Uw wachtwoord opnieuw instellen of wijzigen](../active-directory-passwords-update-your-own-password.md)
* [Registreren voor de selfservice voor wachtwoordherstel](../active-directory-passwords-reset-register.md)
* [Hebt u een vraag over licenties?](concept-sspr-licensing.md)
* [Welke gegevens worden gebruikt door selfservice voor wachtwoordherstel en welke gegevens moet u voor uw gebruikers invullen?](howto-sspr-authenticationdata.md)
* [Wat zijn de beleidsopties bij selfservice voor wachtwoordherstel?](concept-sspr-policy.md)
* [Wat is Wachtwoord terugschrijven en waarom is dit van belang?](howto-sspr-writeback.md)
* [Hoe maak ik rapporten van activiteit in selfservice voor wachtwoordherstel?](howto-sspr-reporting.md)
* [Wat zijn alle opties in selfservice voor wachtwoordherstel en wat houden ze in?](concept-sspr-howitworks.md)
* [Ik denk dat er iets misgaat. Hoe los ik problemen in selfservice voor wachtwoordherstel op?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag die nog niet is beantwoord](active-directory-passwords-faq.md)

[Email]: ./media/howto-sspr-deployment/sspr-emailtemplates.png "Deze e-mailsjablonen aan de behoeften van uw organisatie aanpassen"
