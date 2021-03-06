---
title: 'Zelfstudie: Azure Active Directory-integratie met ThousandEyes | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ThousandEyes.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: jeedes
ms.openlocfilehash: d40ab6d2587f5d842ac98479a6db7609d8a9ce4d
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a>Zelfstudie: Azure Active Directory-integratie met ThousandEyes

In deze zelfstudie leert u hoe ThousandEyes integreren met Azure Active Directory (Azure AD).

ThousandEyes integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ThousandEyes heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij ThousandEyes (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ThousandEyes, moet u de volgende items:

- Een Azure AD-abonnement
- Een ThousandEyes eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand hier downloaden: [proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ThousandEyes uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-thousandeyes-from-the-gallery"></a>ThousandEyes uit de galerie toevoegen
Voor het configureren van de integratie van ThousandEyes in Azure AD, moet u ThousandEyes uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ThousandEyes uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **ThousandEyes**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_search.png)

5. Selecteer in het deelvenster resultaten **ThousandEyes**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met ThousandEyes op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in ThousandEyes is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in ThousandEyes tot stand worden gebracht.

Wijs in ThousandEyes, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met ThousandEyes, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker ThousandEyes](#creating-a-thousandeyes-test-user)**  - ThousandEyes die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing ThousandEyes configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met ThousandEyes, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **ThousandEyes** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_samlbase.png)

3. Op de **ThousandEyes domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_url.png)

    In de **aanmeldings-URL** textbox, typ een URL als: `https://app.thousandeyes.com/login/sso`

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_400.png)

6. Op de **ThousandEyes configuratie** sectie, klikt u op **configureren ThousandEyes** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_configure.png) 

7. In een ander browservenster, meld u aan bij uw **ThousandEyes** bedrijf site als beheerder.

8. Klik in het menu bovenaan op **instellingen**.
   
    ![Instellingen](./media/active-directory-saas-thousandeyes-tutorial/ic790066.png "instellingen")

9. Klik op **Account**
   
    ![Account](./media/active-directory-saas-thousandeyes-tutorial/ic790067.png "Account")

10. Klik op de **beveiliging & verificatie** tabblad.
   
    ![Beveiliging en verificatie](./media/active-directory-saas-thousandeyes-tutorial/ic790068.png "beveiliging en verificatie")

11. In de **Setup Single Sign-On** sectie, voert u de volgende stappen uit:
   
    ![Instellen van eenmalige aanmelding](./media/active-directory-saas-thousandeyes-tutorial/ic790069.png "eenmalige aanmelding instellen")
  
    a. Selecteer **eenmalige aanmelding inschakelen**.
  
    b. In **URL aanmeldingspagina** textbox plakken **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.
  
    c. In **pagina-URL voor afmelden** textbox plakken **Sign-Out URL** die u hebt gekopieerd vanuit Azure-portal.
  
    d. **ID-Provider verlener** textbox plakken **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure-portal.
  
    e. In **verificatiecertificaat**, klikt u op **bestand kiezen**, en vervolgens het certificaat dat u hebt gedownload vanuit Azure-portal uploaden.
  
    f. Klik op **Opslaan**.
 
> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-thousandeyes-test-user"></a>Een testgebruiker ThousandEyes maken

Om in te schakelen gebruikers van Azure AD aan te melden bij ThousandEyes, moeten ze worden ingericht in ThousandEyes.  
In het geval van ThousandEyes is inrichting een handmatige taak.

>[!NOTE]
>U kunt andere ThousandEyes gebruiker account hulpmiddelen voor het maken of API's geleverd door ThousandEyes om in te richten Azure Active Directory-gebruikersaccounts.

**Voor het inrichten van een gebruikersaccount aan ThousandEyes, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf ThousandEyes site als beheerder.

2. Klik op **instellingen**.
   
    ![Instellingen](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "instellingen")

3. Klik op **Account**.
   
    ![Account](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "Account")

4. Klik op de **Accounts gebr & uikers** tabblad.
   
    ![Accounts gebr & uikers](./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "Accounts gebr & uikers")

5. In de **gebruikers toevoegen & Accounts** sectie, voert u de volgende stappen uit:
   
    ![Gebruikersaccounts toevoegen](./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "gebruikersaccounts toevoegen")   
  
    a. In **naam** textbox, typ de naam van gebruiker, zoals **Britta Simon**.

    b. In **e** textbox, typ het e-mailadres van gebruiker, zoals **brittasimon@contoso.com**.
   
    b. Klik op **nieuwe gebruiker toevoegen aan het Account**.
      
     >[!NOTE]
     >De houder van Azure Active Directory-account ontvangt een e-mailbericht een koppeling om te bevestigen en activeren van het account waaronder.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan ThousandEyes.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen ThousandEyes, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **ThousandEyes**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel ThousandEyes in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing ThousandEyes.

Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_203.png

