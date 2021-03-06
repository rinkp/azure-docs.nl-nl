---
title: 'Zelfstudie: Azure Active Directory-integratie met Yodeck | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Yodeck.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b2c8dccb-eeb0-4f4d-a24d-8320631ce819
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: jeedes
ms.openlocfilehash: ba48c5d14775004e22dd67a2932c8969e6dfdd6d
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-yodeck"></a>Zelfstudie: Azure Active Directory-integratie met Yodeck

In deze zelfstudie leert u hoe Yodeck integreren met Azure Active Directory (Azure AD).

Yodeck integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Yodeck heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Yodeck (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Yodeck, moet u de volgende items:

- Een Azure AD-abonnement
- Een Yodeck eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Yodeck uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-yodeck-from-the-gallery"></a>Yodeck uit de galerie toevoegen
Voor het configureren van de integratie van Yodeck in Azure AD, moet u Yodeck uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Yodeck uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Yodeck**, selecteer **Yodeck** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Yodeck in de lijst met resultaten](./media/active-directory-saas-yodeck-tutorial/tutorial_yodeck_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Yodeck op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Yodeck is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Yodeck tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Yodeck, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Yodeck](#create-a-yodeck-test-user)**  - Yodeck die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Yodeck configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Yodeck, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Yodeck** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.

    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-yodeck-tutorial/tutorial_yodeck_samlbase.png)

3. Op de **Yodeck domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![URL's en Yodeck domein eenmalige aanmelding informatie](./media/active-directory-saas-yodeck-tutorial/tutorial_yodeck_url.png)

    In de **id (entiteit-ID)** textbox, typ de URL: `https://app.yodeck.com/api/v1/account/metadata/`

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![URL's en Yodeck domein eenmalige aanmelding informatie](./media/active-directory-saas-yodeck-tutorial/tutorial_yodeck_url1.png)

    In de **aanmeldings-URL** textbox, typ de URL: `https://app.yodeck.com/login`

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-yodeck-tutorial/tutorial_yodeck_certificate.png)

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-yodeck-tutorial/tutorial_general_400.png)
    
7. In een ander browservenster, meld u aan bij uw bedrijf Yodeck site als beheerder.

8. Klik op **gebruikersinstellingen** vorm de rechterbovenhoek van de pagina en selecteer de optie **Accountinstellingen**.

    ![Yodeck configuratie](./media/active-directory-saas-yodeck-tutorial/configure1.png)

9. Selecteer **SAML** en voer de volgende stappen uit:

    ![Yodeck configuratie](./media/active-directory-saas-yodeck-tutorial/configure2.png)

    a. Selecteer **importeren van URL**.

    b. In de **URL** textbox, plak de **App-Url voor federatieve metagegevens** waarde, die u hebt gekopieerd van de Azure-portal en klik op **importeren**.
    
    c. Na het importeren van **App-Url voor federatieve metagegevens**, de overige velden automatisch gevuld.

    d. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-yodeck-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-yodeck-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-yodeck-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-yodeck-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-yodeck-test-user"></a>Een testgebruiker Yodeck maken

Om Azure AD-gebruikers zich aanmelden bij Yodeck, moeten ze worden ingericht in Yodeck.
In het geval van Yodeck is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Yodeck site als beheerder.

2. Klik op **gebruikersinstellingen** vorm de rechterbovenhoek van de pagina en selecteer de optie **gebruikers**.

    ![Werknemer toevoegen](./media/active-directory-saas-yodeck-tutorial/user1.png)

3. Klik op **+ User** openen de **Gebruikersdetails** tabblad.

    ![Werknemer toevoegen](./media/active-directory-saas-yodeck-tutorial/user2.png)

4. Op de **Gebruikersdetails** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/active-directory-saas-yodeck-tutorial/user3.png)

    a. In de **voornaam** textbox type de voornaam van de gebruiker, zoals **Britta**.

    b. In de **achternaam** textbox type de naam van de laatste gebruiker, zoals **Simon**.

    c. In de **e** textbox, typ het e-mailadres van gebruiker, zoals **brittasimon@contoso.com**.

    d. Selecteer juiste **accountmachtigingen** optie volgens de vereisten van uw organisatie.
    
    e. Klik op **Opslaan**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Yodeck.

![Toewijzen van de gebruikersrol][200]

**Britta Simon om aan te wijzen Yodeck, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen **Yodeck**.

    ![De koppeling Yodeck in de lijst met toepassingen](./media/active-directory-saas-yodeck-tutorial/tutorial_yodeck_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Yodeck in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Yodeck.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-yodeck-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-yodeck-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-yodeck-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-yodeck-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-yodeck-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-yodeck-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-yodeck-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-yodeck-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-yodeck-tutorial/tutorial_general_203.png

