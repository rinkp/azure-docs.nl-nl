---
title: 'Zelfstudie: Azure Active Directory-integratie met Dropbox voor bedrijven | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Dropbox voor bedrijven.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: jeedes
ms.openlocfilehash: aa00a88f8325345b1b45d7d0971a03590bce1029
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Zelfstudie: Azure Active Directory-integratie met Dropbox voor bedrijven

In deze zelfstudie leert u hoe Dropbox voor bedrijven integreren met Azure Active Directory (Azure AD).

Dropbox voor bedrijven integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Dropbox voor bedrijven heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Dropbox voor bedrijven (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Dropbox voor bedrijven, moet u de volgende items:

- Een Azure AD-abonnement
- Een Dropbox voor eenmalige aanmelding Business abonnement ingeschakeld

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Dropbox voor bedrijven uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-dropbox-for-business-from-the-gallery"></a>Dropbox voor bedrijven uit de galerie toevoegen
Voor het configureren van de integratie van Dropbox voor bedrijven met Azure AD, moet u Dropbox voor bedrijven uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Dropbox voor bedrijven uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Dropbox voor bedrijven**, selecteer **Dropbox voor bedrijven** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Dropbox voor bedrijven in de lijst met resultaten](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Dropbox voor bedrijven op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Dropbox voor bedrijven is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Dropbox voor bedrijven tot stand worden gebracht.

Wijs in Dropbox voor bedrijven, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Dropbox voor bedrijven, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een Dropbox voor zakelijke testgebruiker](#create-a-dropbox-for-business-test-user)**  - Dropbox voor bedrijven die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw dropbox geplaatst voor Business-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Dropbox voor bedrijven, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Dropbox voor bedrijven** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_samlbase.png)

3. Op de **Dropbox voor Business-domein en URL's** sectie, voert u de volgende stappen uit:

    ![Dropbox voor Business-domein en URL's eenmalige aanmelding informatie](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_url1.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://www.dropbox.com/sso/<id>`

    b. In de **id** textbox in een waarde: `Dropbox`

    > [!NOTE] 
    > De voorgaande aanmeldings-URL-waarde is geen echte waarde. U kunt de waarde wordt bijgewerkt met de werkelijke aanmeldings-URL, die verderop in de zelfstudie wordt beschreven. Neem contact op met [Dropbox voor Business Client ondersteuningsteam](https://www.dropbox.com/business/contact) de waarde op te halen. 
 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_400.png)

6. Op de **Dropbox voor zakelijke configuratie** sectie, klikt u op **Dropbox configureren voor bedrijven** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Dropbox voor bedrijven-configuratie](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_configure.png) 

7. Eenmalige aanmelding configureren op **Dropbox voor bedrijven** zijde, gaat u op uw Dropbox voor bedrijven-tenant.

    a. Meld u aan op uw Dropbox voor bedrijven-tenant. 
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769509.png "eenmalige aanmelding configureren")
   
    b. Klik in het navigatievenster aan de linkerkant op **beheerconsole**. 
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769510.png "eenmalige aanmelding configureren")
   
    c. Op de **beheerconsole**, klikt u op **verificatie** in het navigatiedeelvenster links. 
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769511.png "eenmalige aanmelding configureren")
   
    d. In de **eenmalige aanmelding** sectie **eenmalige aanmelding inschakelen**, en klik vervolgens op **meer** uit te breiden in deze sectie.  
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769512.png "eenmalige aanmelding configureren")
   
    e. Kopieer de URL naast **kunnen gebruikers zich aanmelden door te voeren van het e-mailadres of kunnen ze rechtstreeks naar** en plak deze in de **aanmeldings-URL** textbox van **Dropbox voor Business-domein en URL's** sectie op Azure-portal. 
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769513.png)
    
8. In de **eenmalige aanmelding** sectie van de **verificatie** pagina, voert u de volgende stappen uit: 
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "eenmalige aanmelding configureren")
   
    a. Klik op **vereist**.
   
    b. In de de **aanmelden URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL** die u hebt gekopieerd uit de Azure portal.

    c. Klik op **certificaat kiezen**, en blader vervolgens naar uw **Base64-gecodeerde certificaatbestand**.

    d. Klik op **wijzigingen opslaan** om de configuratie op uw DropBox voor zakelijke tenant te voltooien.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-dropbox-for-business-test-user"></a>Een Dropbox voor zakelijke testgebruiker maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in Dropbox voor bedrijven. Dropbox voor bedrijven ondersteunt just-in-time-inrichting, die standaard is ingeschakeld.

Er is geen actie-item voor u in deze sectie. Als een gebruiker niet al in Dropbox voor bedrijven bestaat, wordt een nieuw gemaakt wanneer u probeert te krijgen tot Dropbox voor bedrijven.

>[!Note]
>Als u wilt een gebruiker handmatig maken, neem contact op met [Dropbox voor Business Client ondersteuningsteam](https://www.dropbox.com/business/contact) 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Dropbox voor bedrijven.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Dropbox voor bedrijven, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Dropbox voor bedrijven**.

    ![De Dropbox voor bedrijven-koppeling in de lijst met toepassingen](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u klikt op de Dropbox voor bedrijven-tegel in het deelvenster toegang, krijgt u de aanmeldingspagina van uw Dropbox voor Business-toepassing.
 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_203.png

