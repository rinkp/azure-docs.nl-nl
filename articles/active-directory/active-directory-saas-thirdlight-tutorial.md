---
title: 'Zelfstudie: Azure Active Directory-integratie met ThirdLight | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ThirdLight.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 168aae9a-54ee-4c2b-ab12-650a2c62b901
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 95b5fb195f0f08eaa218536caf727c78826e1bc8
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-thirdlight"></a>Zelfstudie: Azure Active Directory-integratie met ThirdLight

In deze zelfstudie leert u hoe ThirdLight integreren met Azure Active Directory (Azure AD).

ThirdLight integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ThirdLight heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij ThirdLight (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ThirdLight, moet u de volgende items:

- Een Azure AD-abonnement
- Een ThirdLight eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ThirdLight uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-thirdlight-from-the-gallery"></a>ThirdLight uit de galerie toevoegen
Voor het configureren van de integratie van ThirdLight in Azure AD, moet u ThirdLight uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ThirdLight uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **ThirdLight**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_search.png)

5. Selecteer in het deelvenster resultaten **ThirdLight**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met ThirdLight op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in ThirdLight is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in ThirdLight tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in ThirdLight.

Om te configureren en testen van Azure AD eenmalige aanmelding met ThirdLight, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker ThirdLight](#creating-a-thirdlight-test-user)**  - ThirdLight die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing ThirdLight configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met ThirdLight, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **ThirdLight** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_samlbase.png)

3. Op de **ThirdLight domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.thirdlight.com/`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.thirdlight.com/saml/sp`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden met de werkelijke aanmeldings-URL en Identiifer bijwerken. Neem contact op met [ThirdLight Client ondersteuningsteam](https://www.thirdlight.com/support) ophalen van deze waarden. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thirdlight-tutorial/tutorial_general_400.png)

6. In een ander browservenster, meld u aan bij uw bedrijf ThirdLight site als beheerder.

7. Ga naar **configuratie \> Systeembeheer**, en klik vervolgens op **SAML2**.
   
    ![Systeembeheer](./media/active-directory-saas-thirdlight-tutorial/ic805843.png "Systeembeheer")

8. Voer de volgende stappen uit in de configuratiesectie SAML2:
   
    ![Eenmalige aanmelding SAML](./media/active-directory-saas-thirdlight-tutorial/ic805844.png "SAML eenmalige aanmelding")   

     a. Selecteer **eenmalige aanmelding SAML2 inschakelen**.
 
     b. Als **bron voor IdP metagegevens**, selecteer **IdP-metagegevens laden van XML**.
 
     c. Openen van het metagegevensbestand van de gedownloade, de inhoud kopieert en plakt u deze in de **IdP metagegevens-XML** textbox. 
     
     d. Klik op **instellingen opslaan SAML2**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van Britta Simon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-thirdlight-test-user"></a>Een testgebruiker ThirdLight maken

Om Azure AD-gebruikers zich aanmelden bij ThirdLight, moeten ze worden ingericht in ThirdLight.  
In het geval van ThirdLight is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **ThirdLight** bedrijf site als beheerder.

2. Ga naar **gebruikers** tabblad.

3. Selecteer **gebruikers en groepen**.

4. Klik op **nieuwe gebruiker toevoegen** knop.

5. Voer **de gebruikersnaam, naam of beschrijving, E-mail, kiest u een vooraf ingestelde of groep van nieuwe leden** van een geldige AAD-account dat u inrichten wilt.

6. Klik op **Create**.

>[!NOTE]
>U kunt andere Thirdlight gebruiker account hulpmiddelen voor het maken of API's die is geleverd door Thirdlight aan inrichten AAD-gebruikersaccounts. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan ThirdLight.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen ThirdLight, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **ThirdLight**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel ThirdLight in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing ThirdLight.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_203.png

