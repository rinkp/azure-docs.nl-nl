---
title: Een Azure Service Fabric-toepassing met continue integratie implementeren (Team Services) | Microsoft Docs
description: In deze zelfstudie leert u continue integratie en implementatie in te stellen voor een Service Fabric-toepassing met behulp van Visual Studio-teamservices.  Een toepassing implementeren in een Service Fabric-cluster in Azure.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/13/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 5b61b7f89c127b297f058082d86952f2a45d766a
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="tutorial-deploy-an-application-with-cicd-to-a-service-fabric-cluster"></a>Zelfstudie: een toepassing met CI/CD implementeren in een Service Fabric-cluster
Deze zelfstudie is deel vier van een reeks en beschrijft het instellen van continue integratie en implementatie voor een Azure Service Fabric-toepassing met behulp van Visual Studio Team Services.  Er is een bestaande Service Fabric-toepassing vereist. De toepassing die in [Een .NET-toepassing bouwen](service-fabric-tutorial-create-dotnet-app.md) is gemaakt, wordt als voorbeeld gebruikt.

In deel drie van de serie leert u het volgende:

> [!div class="checklist"]
> * Broncodebeheer aan uw project toevoegen
> * Een build-definitie in Team Services maken
> * Een release-definitie in Team Services maken
> * Automatisch een upgrade en een toepassing implementeren

In deze zelfstudiereeks leert u het volgende:
> [!div class="checklist"]
> * [Een .NET Service Fabric-toepassing bouwen](service-fabric-tutorial-create-dotnet-app.md)
> * [De toepassing implementeren in een extern cluster](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [Een HTTPS-eindpunt toevoegen aan een front-end-service van ASP.NET Core](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
> * CI/CD configureren met behulp van Visual Studio Team Services
> * [Controle en diagnostische gegevens voor de toepassing instellen](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Vereisten
Voor u met deze zelfstudie begint:
- Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Installeer Visual Studio 2017](https://www.visualstudio.com/) en installeer de workloads **Azure-ontwikkeling** en **ASP.NET-ontwikkeling en webontwikkeling**.
- [Installeer de Service Fabric-SDK](service-fabric-get-started.md)
- Maak een Windows Service Fabric-cluster in Azure, bijvoorbeeld door [deze zelfstudie te volgen](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
- Maak een [Team Services-account](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).

## <a name="download-the-voting-sample-application"></a>De voorbeeldtoepassing om te stemmen downloaden
Als u in [deel één van deze zelfstudiereeks](service-fabric-tutorial-create-dotnet-app.md) niet de voorbeeldtoepassing om te stemmen hebt gemaakt, kunt u deze downloaden. Voer in een opdrachtvenster de volgende opdracht uit om de voorbeeld-app-opslagplaats te klonen op de lokale computer.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="prepare-a-publish-profile"></a>Een publicatieprofiel voorbereiden
U hebt [een toepassing gemaakt](service-fabric-tutorial-create-dotnet-app.md) en [de toepassing in Azure geïmplementeerd](service-fabric-tutorial-deploy-app-to-party-cluster.md). U kunt nu continue integratie instellen.  Bereid eerst een publicatieprofiel binnen uw toepassing voor, dat kan worden gebruikt door het implementatieproces dat in Team Services wordt uitgevoerd.  Het publicatieprofiel moet worden geconfigureerd zodat het cluster dat u eerder hebt gemaakt als doel kan dienen.  Start Visual Studio en open een bestaand Service Fabric-toepassingsproject.  Klik in **Solution Explorer** met de rechtermuisknop op de toepassing en selecteer **Publish...**.

Kies een doelprofiel in het toepassingsproject, dat voor de werkstroom voor continue integratie kan worden gebruikt, bijvoorbeeld Cloud.  Geef het eindpunt voor de clusterverbinding op.  Schakel het selectievakje bij **Upgrade the Application** in, zodat voor elke implementatie in Team Services een upgrade voor de toepassing wordt uitgevoerd.  Klik op de hyperlink **Save** om de instellingen op te slaan in het publicatieprofiel. Klik vervolgens op **Cancel** om het dialoogvenster te sluiten.  

![Push-profiel][publish-app-profile]

## <a name="share-your-visual-studio-solution-to-a-new-team-services-git-repo"></a>Uw Visual Studio-oplossing delen met een nieuwe Git-opslagplaats van Team Services
Deel de bronbestanden van uw toepassing met een teamproject in Team Services zodat u builds kunt genereren.  

Maak een nieuwe Git-opslagplaats voor uw project door op de statusbalk in de linkeronderhoek van Visual Studio **Add to Source Control** -> **Git** te selecteren. 

Ga naar de **Push**-weergave in **Team Explorer** en selecteer onder **Push to Visual Studio Team Services** de knop **Publish Git Repo**.

![Git-opslagplaats pushen][push-git-repo]

Controleer uw e-mail en selecteer uw account in de vervolgkeuzelijst **Team Services Domain**. Voer de naam van de opslagplaats in en selecteer **Publish repository**.

![Git-opslagplaats pushen][publish-code]

Als u de opslagplaats pusht, wordt er een nieuw teamproject voor uw account gemaakt met dezelfde naam als de lokale opslagplaats. Als u de repro in een bestaand teamproject wilt maken, klikt u naast de naam van de **opslagplaats** op **Advanced** en selecteert u een teamproject. U kunt uw code op het web weergeven door **See it on the web** te selecteren.

## <a name="configure-continuous-delivery-with-vsts"></a>Continue levering configureren met VSTS
Een definitie van een Team Services-build beschrijft een werkstroom die bestaat uit een reeks build-stappen die achtereenvolgens worden uitgevoerd. Maak een definitie van een build die een Service Fabric-toepassingspakket maakt en andere artefacten, om deze in een Service Fabric-cluster te implementeren. Meer informatie over [Definities van Team Services-builds](https://www.visualstudio.com/docs/build/define/create). 

Een definitie van een Team Services-release beschrijft een werkstroom waarmee een toepassingspakket in een cluster wordt geïmplementeerd. Als de definities van de build en de release samen worden gebruikt, wordt hiermee de hele werkstroom uitgevoerd, te beginnen met bronbestanden en te eindigen met het uitvoeren van een toepassing in uw cluster. Meer informatie over [release-definities ](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition) van Team Services.

### <a name="create-a-build-definition"></a>Een build-definitie maken
Open een webbrowser en ga naar uw nieuwe teamproject op: [https://&lt;myaccount&gt;.visualstudio.com/Voting/Voting%20Team/_git/Voting](https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting). 

Selecteer het tabblad **Build & Release**, vervolgens **Builds** en **+ New definition**.  Selecteer in **Select a template** de sjabloon **Azure Service Fabric Application** en klik op **Apply**. 

![Build-sjabloon kiezen][select-build-template] 

Voer in **Tasks** Hosted VS2017 in als de **Agent queue**. 

![Taken selecteren][save-and-queue]

Schakel onder **Triggers** continue integratie in door **Trigger status** in te stellen.  Selecteer **Save and queue** om handmatig een build te starten.  

![Triggers selecteren][save-and-queue2]

Hiermee worden ook triggers gebouwd na pushen of inchecken. Als u de voortgang van de build wilt controleren, schakelt u over naar het tabblad **Builds**.  Als u hebt gecontroleerd dat de build correct wordt uitgevoerd, definieert u een release-definitie waarmee uw toepassing in een cluster wordt geïmplementeerd. 

### <a name="create-a-release-definition"></a>Release-definitie maken  

Selecteer het tabblad **Build & Release**, vervolgens **Releases** en **+ New definition**.  Selecteer in **Select a template** de sjabloon **Azure Service Fabric Deployment** in de lijst en vervolgens **Apply**.  

![Release-sjabloon kiezen][select-release-template]

Selecteer **Tasks**->**Environment 1** en vervolgens **+New** om een nieuwe clusterverbinding toe te voegen.

![Clusterverbinding toevoegen][add-cluster-connection]

Selecteer in de weergave **Add new Service Fabric Connection** **Certificate Based**- of **Azure Active Directory**-verificatie.  Geef de verbindingsnaam mysftestcluster op en het clustereindpunt tcp://mysftestcluster.southcentralus.cloudapp.azure.com:19000 (of het eindpunt van het cluster waarin de implementatie wordt uitgevoerd). 

Voor verificatie op basis van verificatie voegt u de **vingerafdruk voor servercertificaat** toe van het servercertificaat waarmee het cluster is gemaakt.  Voeg in het **clientcertificaat** de basis-64-codering van het certificaatbestand van de client toe. Zie het pop-upitem met de Help voor dat veld voor informatie over hoe u de basis-64-representatie van het certificaat ophaalt. Voeg ook het **wachtwoord** voor het certificaat toe.  U kunt het cluster- of het servercertificaat gebruiken als u geen apart clientcertificaat hebt. 

Voor referenties van Azure Active Directory voegt u de **vingerafdruk voor het servercertificaat** van het servercertificaat waarmee het cluster is gemaakt, en de referenties die u wilt gebruiken om verbinding te maken met het cluster, toe aan de velden **Username** en **Password**. 

Klik op **Add** om de clusterverbinding op te slaan.

Voeg vervolgens een build-artefact toe aan de pijplijn, zodat met de releasedefinitie de uitvoer van de build kan worden gevonden. Selecteer **Pipeline** en **Artifacts**->**+Add**.  Selecteer in **Source (Build definition)** de build-definitie die u eerder hebt gemaakt.  Klik op **Add** om het build-artefact op te slaan.

![Artefact toevoegen][add-artifact]

Schakel een trigger voor continue implementatie in, zodat automatisch een release wordt gemaakt als de build wordt voltooid. Klik op het bliksempictogram in het artefact, schakel de trigger in en klik op **Save** om de release-definitie op te slaan.

![Trigger inschakelen][enable-trigger]

Selecteer **+Release** -> **Create Release** -> **Create** om handmatig een release te maken.  Controleer of de implementatie is gelukt en de toepassing in het cluster wordt uitgevoerd.  Open een browser en ga naar [http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/).  Noteer de versie van de toepassing (in dit voorbeeld 1.0.0.20170616.3). 

## <a name="commit-and-push-changes-trigger-a-release"></a>Wijzigingen doorvoeren en pushen, een release activeren
Als u wilt controleren of de pijplijn voor continue integratie functioneert, checkt u enkele codewijzigingen aan Team Services in.    

Terwijl u de code schrijft, worden de wijzigingen automatisch in Visual Studio bijgehouden. Voer wijzigingen door in de lokale Git-opslagplaats door het pictogram voor wijzigingen in behandeling (![In behandeling][pending]) te selecteren op de statusbalk rechtsonder.

Voeg aan de weergave **Changes** in Team Explorer een bericht toe waarin u de update beschrijft en voer de wijzigingen door.

![Alles doorvoeren][changes]

Selecteer het pictogram voor de statusbalk met niet-gepubliceerde wijzigingen (![Unpublished changes][unpublished-changes]) of de synchronisatieweergave in Team Explorer. Selecteer **Push** om de code bij te werken in Team Services/TFS.

![Wijzigingen pushen][push]

Als u de wijzigingen naar Team Services pusht, wordt er automatisch een build geactiveerd.  Als de build-definitie wordt voltooid, wordt er automatisch een release gemaakt en wordt de toepassing in het cluster bijgewerkt.

Als u de voortgang van de build wilt controleren, schakelt u over naar het tabblad **Builds** in **Team Explorer** in Visual Studio.  Als u hebt gecontroleerd dat de build correct wordt uitgevoerd, definieert u een release-definitie waarmee uw toepassing in een cluster wordt geïmplementeerd.

Controleer of de implementatie is gelukt en de toepassing in het cluster wordt uitgevoerd.  Open een browser en ga naar [http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/).  Noteer de versie van de toepassing (in dit voorbeeld 1.0.0.20170815.3).

![Service Fabric Explorer][sfx1]

## <a name="update-the-application"></a>De toepassing bijwerken
Breng in de toepassing wijzigingen aan de code aan.  Sla de wijzigingen op en voer ze door aan de hand van de voorgaande stappen.

Zodra de upgrade van de toepassing wordt uitgevoerd, kunt u de voortgang ervan volgen in Service Fabric Explorer:

![Service Fabric Explorer][sfx2]

Het kan enkele minuten duren voordat de toepassing is bijgewerkt. Als het bijwerken is voltooid, wordt de volgende versie door de toepassing uitgevoerd.  In dit voorbeeld 1.0.0.20170815.4.

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Broncodebeheer aan uw project toevoegen
> * Een build-definitie maken
> * Release-definitie maken
> * Automatisch een upgrade en een toepassing implementeren

Ga door naar de volgende zelfstudie:
> [!div class="nextstepaction"]
> [Controle en diagnostische gegevens voor de toepassing instellen](service-fabric-tutorial-monitoring-aspnet.md) 


<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishCode.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectBuildTemplate.png
[save-and-queue]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SaveAndQueue.png
[save-and-queue2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SaveAndQueue2.png
[select-release-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectReleaseTemplate.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddClusterConnection.png
[add-artifact]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddArtifact.png
[enable-trigger]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/EnableTrigger.png
[sfx1]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX1.png
[sfx2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX2.png
[sfx3]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX3.png
[pending]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Pending.png
[changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Changes.png
[unpublished-changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/UnpublishedChanges.png
[push]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Push.png
[continuous-delivery-with-VSTS]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/VSTS-Dialog.png
[new-service-endpoint]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpoint.png
[new-service-endpoint-dialog]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpointDialog.png
