---
title: Azure Data Lake Store-resources beheren in Azure Storage Explorer
description: Meer informatie over het openen en beheren van uw gegevens in Azure Data Lake Store en resources in Azure Storage Explorer
Keywords: Azure Data Lake Store, Azure Storage Explorer
services: Data Lake Store
documentationcenter: ''
author: jejiang
manager: DJ
editor: Jenny Jiang
ms.assetid: ''
ms.service: Data Lake Store
ms.custom: Azure Data Lake Store
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 02/05/2018
ms.author: jejiang
ms.openlocfilehash: 396452faf36bc43a185fd6492f952b7bfd9511cc
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="manage-azure-data-lake-store-resources-by-using-storage-explorer"></a>Azure Data Lake Store-resources beheren met behulp van Storage Explorer

[Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens, zoals tekstgegevens of binaire gegevens. U kunt overal toegang tot de gegevens krijgen via HTTP of HTTPS. Met Data Lake Store in Azure Storage Explorer kunt u gegevens en resources in Data Lake Store openen en beheren, evenals andere Azure-entiteiten, zoals blobs en wachtrijen. U kunt nu hetzelfde hulpprogramma gebruiken om uw verschillende Azure entiteiten op één plek te beheren.

Een ander voordeel is dat u geen abonnementsmachtiging nodig hebt voor het beheren van gegevens in Data Lake Store. In Storage Explorer kunt u het Data Lake Store-pad bijvoegen bij het **lokale en bijgevoegde** knooppunt als iemand toestemming verleent.

## <a name="prerequisites"></a>Vereisten
U moet de volgende vereiste zaken hebben om de stappen in dit artikel uit te voeren:

*   Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial).
*   Een Azure Data Lake Store-account. Zie [Aan de slag met Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal) voor instructies over hoe u zo’n account maakt.

## <a name="install-storage-explorer"></a>Storage Explorer installeren

Installeer de meest recente Azure Storage Explorer-bits vanaf de [productwebpagina](https://azure.microsoft.com/features/storage-explorer/). Windows-, Linux- en Mac-versies worden door de installatie ondersteund.

## <a name="connect-to-an-azure-subscription"></a>Verbinding maken met een Azure-abonnement

1. Selecteer in Storage Explorer het pictogram voor de invoegtoepassing aan de linkerkant.
       
   ![Pictogram voor de invoegtoepassing](./media/data-lake-store-in-storage-explorer/plug-in-icon.png)
 
2. Selecteer **Een Azure-account toevoegen** en selecteer vervolgens **Aanmelden**.

   ![Dialoogvenster Verbinding maken met Azure Storage](./media/data-lake-store-in-storage-explorer/connect-to-azure-subscription.png)

2. Voer in het dialoogvenster **Aanmelden bij uw account** uw Azure-referenties in.

    ![Dialoogvenster voor aanmelden bij Azure](./media/data-lake-store-in-storage-explorer/sign-in.png)

3. Selecteer uw abonnement in de lijst en selecteer vervolgens **Toepassen**.

    ![Abonnementsgegevens en knop Toepassen](./media/data-lake-store-in-storage-explorer/apply-subscription.png)

    Het deelvenster **VERKENNER** wordt bijgewerkt en de accounts in het geselecteerde abonnement worden weergegeven.

    ![Lijst met accounts](./media/data-lake-store-in-storage-explorer/account-list.png)

U hebt Azure Data Lake Store gekoppeld aan uw Azure-abonnement.

## <a name="connect-to-data-lake-store"></a>Verbinding maken met Data Lake Store
U kunt resources openen die niet voor uw abonnement bestaan als iemand u de URI voor de resources geeft. Vervolgens kunt u na het aanmelden verbinding maken met Data Lake Store door middel van de URI.
1. Open Storage Explorer.
2. Vouw **Lokaal en gekoppeld** uit in het linkerdeelvenster.
3. Klik met de rechtermuisknop op **Data Lake Store** en selecteer **Verbinding maken met Data Lake Store**.

      ![Verbinding maken met Data Lake Store in het snelmenu](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-uri-attach.png)

4. Voer de URI in. U gaat naar de locatie voor de URL die u zojuist hebt ingevoerd.

      ![Dialoogvenster Verbinding maken met Data Lake Store, met het tekstvak voor invoer van de URI](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

      ![Resultaat van het verbinding maken met Data Lake Store](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-attach-finish.png)

## <a name="view-an-azure-data-lake-store-accounts-contents"></a>De inhoud van een Azure Data Lake Store-account bekijken
De resources van een Azure Data Lake Store-account bevatten mappen en bestanden.

Volg de volgende stappen om de inhoud van een Data Lake Store-account weer te geven binnen Storage Explorer:

1. Open Storage Explorer.
2. Vouw in het linkerdeelvenster het abonnement met het Azure Data Lake Store-account uit dat u wilt bekijken.
3. Vouw **Data Lake Store** uit.
4. Klik met de rechtermuisknop op het knooppunt van het Azure Data Lake Store-account dat u wilt bekijken en selecteer **Openen**. U kunt ook dubbelklikken op het Data Lake Store-account om het te openen. 
   
   In het hoofdvenster wordt de inhoud van het Data Lake Store-account weergegeven.

   ![Hoofdvenster met een lijst mappen](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-toolbar-mainpane.png) 

## <a name="manage-resources-in-azure-data-lake-store"></a>Resources in Azure Data Lake Store beheren

U kunt Azure Data Lake Store-resources beheren door de volgende bewerkingen uit te voeren:
*   Blader door Data Lake Store-resources voor meerdere Azure Data Lake-accounts.  
*   Gebruik een verbindingsreeks om rechtstreeks verbinding te maken met Data Lake Store en het te beheren. 
*   Bekijk resources in Data Lake Store die door andere personen worden gedeeld via een ACL onder **Lokaal en gekoppeld**.
*   Voer CRUD-bewerkingen uit voor bestanden en mappen: bied ondersteuning voor recursieve mappen en meervoudig geselecteerde bestanden. 
*   Sleep een map en voeg deze toe voor snelle toegang van recent gebruikte locaties. Deze bewerking is analoog aan die van Verkenner op het bureaublad. 
*   Kopieer en open met één klik een Azure Data Lake-hyperlink in Storage Explorer. 
*   Geef het activiteitenlogboek weer in het deelvenster rechtsonder om de activiteitsstatus te bekijken.
*   Geef de mapstatistieken en de bestandseigenschappen weer.

## <a name="manage-resources-in-azure-storage-explorer"></a>Beheer resources in Azure Storage Explorer
Nadat u een Azure Data Lake Store-account hebt gemaakt, kunt u:

* Mappen en bestanden uploaden en downloaden, en resources op de lokale computer openen.
* Items vastmaken aan **Snelle toegang**, nieuwe mappen maken, URL's kopiëren en alles selecteren.
* Kopiëren en plakken, wijzigen, verwijderen, mapstatistieken ophalen, en vernieuwen.

De volgende items laten zien hoe u resources binnen een Azure Data Lake Store-account kunt beheren. Volg de stappen voor de taak die u wilt uitvoeren.

### <a name="upload-files"></a>Bestanden uploaden

1. Selecteer **Uploaden** op de werkbalk van het hoofdvenster en selecteer vervolgens **Bestanden uploaden** in de vervolgkeuzelijst.

   ![Menu-item Bestanden uploaden](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-files-menu.png) 

2. Selecteer in het dialoogvenster **Bestanden selecteren voor uploaden** de bestanden die u wilt uploaden.

   ![Dialoogvenster voor het uploaden van bestanden](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-files-dialog.png)

3. Selecteer **Openen** om te beginnen met uploaden.

### <a name="upload-a-folder"></a>Map uploaden

1. Selecteer **Uploaden** op de werkbalk van het hoofdvenster en selecteer vervolgens **Map uploaden** in de vervolgkeuzelijst.

   ![Menu-item Map uploaden](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-menu.png) 
     
2. Selecteer in het dialoogvenster **Map selecteren voor uploaden** een map die u wilt uploaden. Klik vervolgens op **Map selecteren**.

   ![Dialoogvenster voor het uploaden van mappen](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-dialog.png)      

   Het uploaden wordt gestart.

   ![Dialoogvenster waarin de voortgang van het uploaden wordt getoond](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-drag.png) 

> [!NOTE] 
> U kunt de mappen en bestanden rechtstreeks in een lokale computer slepen om het uploaden te starten. 
       
### <a name="download-folders-or-files-to-your-local-computer"></a>Mappen of bestanden downloaden naar uw lokale computer

1. Selecteer de mappen of bestanden die u wilt downloaden.
2. Selecteer **Downloaden** op de werkbalk van het hoofdvenster.
3. Geef in het dialoogvenster **Een map selecteren om de gedownloade bestanden in op te slaan** de locatie en de naam op.
4. Selecteer **Opslaan**.

### <a name="open-a-folder-or-file-from-your-local-computer"></a>Een bestand of map openen vanaf uw lokale computer

1. Selecteer de map of het bestand dat u wilt openen.
2. Selecteer **Openen** op de werkbalk van het hoofdvenster. Of klik met de rechtermuisknop op de geselecteerde map of het geselecteerde bestand en selecteer vervolgens **Openen** in het snelmenu.

Het bestand wordt gedownload en geopend met de toepassing die is gekoppeld aan het onderliggende bestandstype. Of de map wordt in het hoofdvenster geopend.

![Geopend bestand](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-open.png) 

### <a name="copy-folders-or-files-to-the-clipboard"></a>Mappen of bestanden kopiëren naar het klembord

1. Selecteer de mappen of bestanden die u wilt kopiëren.
2. Selecteer **Kopiëren** op de werkbalk van het hoofdvenster. Of klik met de rechtermuisknop op de geselecteerde map of het geselecteerde bestand en selecteer vervolgens **Kopiëren** in het snelmenu.
3. Blader in het linkerdeelvenster naar een ander Data Lake Store-account en dubbelklik hierop om het weer te geven in het hoofdvenster.
4. Selecteer **Plakken** op de werkbalk van het hoofdvenster om een kopie te maken. Of selecteer **Plakken** in het snelmenu van het doel.

![Selecties voor het kopiëren van een map](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-copy-paste.png)

> [!NOTE] 
> Kopieer- en plakbewerkingen voor opslagtypen worden niet ondersteund. U kunt mappen en bestanden in Data Lake Store kopiëren en ze in een ander Data Lake Store-account plakken. Maar u kunt mappen en bestanden in Data Lake Store *niet* kopiëren en plakken in Azure Blob Storage, of andersom.
> 
> Bij het kopiëren en plakken worden de mappen of bestanden gedownload naar de lokale computer en vervolgens geüpload naar het doel. Het hulpprogramma voert de actie *niet* uit in de back-end. Het kopiëren en plakken van grote bestanden gaat langzaam. Het geavanceerd kopiëren en verplaatsen van bestanden wordt momenteel geoptimaliseerd.

### <a name="delete-folders-or-files"></a>Mappen of bestanden verwijderen

1. Selecteer de mappen of bestanden die u wilt verwijderen.
2. Selecteer **Verwijderen** op de werkbalk van het hoofdvenster. Of klik met de rechtermuisknop op de geselecteerde map of het geselecteerde bestand en selecteer vervolgens **Verwijderen** in het snelmenu.
3. Selecteer **Ja** in het bevestigingsvenster.

![Selecties voor het verwijderen van een map](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-delete.png)

### <a name="pin-to-quick-access"></a>Vastmaken aan Snelle toegang

1. Selecteer de map die u wilt vastmaken.
2. Selecteer **Vastmaken aan Snelle toegang** op de werkbalk van het hoofdvenster.

   In het linkerdeelvenster wordt de geselecteerde map toegevoegd aan het knooppunt **Snelle toegang**.

   ![Selecties voor het vastmaken van een map aan Snelle toegang](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-quick-access.png)

Nadat u een map hebt vastgemaakt aan het knooppunt **Snelle toegang**, kunt u de resources makkelijk openen.

### <a name="use-deep-links"></a>Dieptekoppelingen gebruiken
Als u een URL hebt, kunt u de URL invoeren in het adrespad in Verkenner of in de browser. Vervolgens wordt Storage Explorer.exe automatisch uitgevoerd om naar de locatie van de URL te gaan.

![Dieptekoppeling in Verkenner](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-deep-link.png)


## <a name="next-steps"></a>Volgende stappen
* De [meest recente releaseopmerkingen en video's van Storage Explorer](http://www.storageexplorer.com) bekijken.
* Informatie over [het beheer van Azure Cosmos DB in Azure Storage Explorer](https://docs.microsoft.com/azure/cosmos-db/storage-explorer).
* [Aan de slag met Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).
* [Aan de slag met Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview).
* Bekijk een (Engelstalige) [YouTube-video over het gebruik van Azure Cosmos DB in Azure Storage Explorer](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be).
