---
title: Bewaking en diagnose voor Windows Containers in Azure Service Fabric | Microsoft Docs
description: In deze zelfstudie stelt u bewaking en diagnose in voor Windows Container, georganiseerd in Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/20/2017
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: 9024036c5340e9afb2369feedde140d84e880265
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="tutorial-monitor-windows-containers-on-service-fabric-using-log-analytics"></a>Zelfstudie: Windows-containers in Service Fabric bewaken met behulp van Log Analytics

Dit is deel twee van een zelfstudie. U wordt begeleid bij het instellen van Log Analytics voor het bewaken van uw Windows-containers die in Service Fabric worden georganiseerd.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Log Analytics configureren voor uw Service Fabric-cluster
> * Een Log Analytics-werkruimte gebruiken om logboeken te bekijken en te doorzoeken vanuit uw containers en knooppunten
> * De OMS-agent configureren om metrische gegevens uit containers en knooppunten te halen

## <a name="prerequisites"></a>Vereisten
Voordat u aan deze zelfstudie begint, dient u eerst:
- Een cluster op Azure te hebben of [er een maken met behulp van deze zelfstudie](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
- [Een containertoepassing erin te implementeren](service-fabric-host-app-in-a-container.md)

## <a name="setting-up-log-analytics-with-your-cluster-in-the-resource-manager-template"></a>Log Analytics voor uw cluster instellen in de Resource Manager-sjabloon

Als u in het eerste deel van deze zelfstudie de [opgegeven sjabloon](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Tutorial) hebt gebruikt, dient deze de volgende aanvullingen voor een generieke Service Fabric Azure Resource Manager-sjabloon te bevatten. Als u over een eigen cluster beschikt die u voor het bewaken van containers met Log Analytics wilt instellen, gaat u als volgt te werk:
* Breng de volgende wijzigingen in de Resource Manager-sjabloon aan.
* Implementeer de sjabloon met PowerShell om uw cluster bij te werken door [de sjabloon te implementeren](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm). Azure Resource Manager weet dat de resource bestaat, zodat de resource als een upgrade wordt uitgebracht.

### <a name="adding-log-analytics-to-your-cluster-template"></a>Log Analytics aan de clustersjabloon toevoegen

Breng de volgende wijzigingen in *template.json* aan:

1. Voeg de locatie en de naam van de Log Analytics-werkruimte toe aan de sectie *parameters*:
    
    ```json
    "omsWorkspacename": {
      "type": "string",
      "defaultValue": "[toLower(concat('sf',uniqueString(resourceGroup().id)))]",
      "metadata": {
        "description": "Name of your Log Analytics Workspace"
      }
    },
    "omsRegion": {
      "type": "string",
      "defaultValue": "East US",
      "allowedValues": [
        "West Europe",
        "East US",
        "Southeast Asia"
      ],
      "metadata": {
        "description": "Specify the Azure Region for your Log Analytics workspace"
      }
    }
    ```

    Als u de waarde die voor een van beide is gebruikt, wilt wijzigen, voegt u dezelfde parameters toe aan *template.parameters.json* en wijzigt u de waarden die u daar hebt gebruikt.

2. Voeg de naam van de oplossing en de oplossing toe aan *variables*: 
    
    ```json
    "omsSolutionName": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "omsSolution": "ServiceFabric"
    ```

3. Voeg de OMS MMA toe als een extensie van de virtuele machine. Zoek de resource voor virtuele-machineschaalsets: *resources* > *"apiVersion": "[variables('vmssApiVersion')]"*. Voeg onder *properties* > *virtualMachineProfile* > *extensionProfile* > *extensions* de volgende beschrijving van de extensie toe onder de extensie *ServiceFabricNode*: 
    
    ```json
    {
        "name": "[concat(variables('vmNodeType0Name'),'OMS')]",
        "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "workspaceId": "[reference(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')), '2015-11-01-preview').customerId]"
            },
            "protectedSettings": {
                "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')),'2015-11-01-preview').primarySharedKey]"
            }
        }
    },
    ```

4. Voeg de Log Analytics-werkruimte toe als afzonderlijke resource. Voeg in *resources*, na de resource voor de virtuele-machineschaalsets, het volgende toe:
    
    ```json
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[parameters('omsWorkspacename')]",
        "type": "Microsoft.OperationalInsights/workspaces",
        "properties": {
            "sku": {
                "name": "Free"
            }
        },
        "resources": [
            {
                "apiVersion": "2015-11-01-preview",
                "name": "[concat(variables('applicationDiagnosticsStorageAccountName'),parameters('omsWorkspacename'))]",
                "type": "storageinsightconfigs",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]",
                    "[concat('Microsoft.Storage/storageAccounts/', variables('applicationDiagnosticsStorageAccountName'))]"
                ],
                "properties": {
                    "containers": [ ],
                    "tables": [
                        "WADServiceFabric*EventTable",
                        "WADWindowsEventLogsTable",
                        "WADETWEventTable"
                    ],
                    "storageAccount": {
                        "id": "[resourceId('Microsoft.Storage/storageaccounts/', variables('applicationDiagnosticsStorageAccountName'))]",
                        "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('applicationDiagnosticsStorageAccountName')),'2015-06-15').key1]"
                    }
                }
            },
            {
                "apiVersion": "2015-11-01-preview",
                "name": "System",
                "type": "datasources",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
                ],
                "kind": "WindowsEvent",
                "properties": {
                    "eventLogName": "System",
                    "eventTypes": [
                        {
                            "eventType": "Error"
                        },
                        {
                            "eventType": "Warning"
                        },
                        {
                            "eventType": "Information"
                        }
                    ]
                }
            }
        ]
    },
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[variables('omsSolutionName')]",
        "type": "Microsoft.OperationsManagement/solutions",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('OMSWorkspacename'))]"
        ],
        "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        },
        "plan": {
            "name": "[variables('omsSolutionName')]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('omsSolution'))]",
            "promotionCode": ""
        }
    },
    ```

[Hier](https://github.com/ChackDan/Service-Fabric/blob/master/ARM%20Templates/Tutorial/azuredeploy.json) vindt u een voorbeeldsjabloon (gebruikt in deel één van deze zelfstudie) die al deze wijzigingen bevat en die u zo nodig kunt raadplegen. Dankzij deze wijzigingen wordt er een Log Analytics-werkruimte aan de resourcegroep toegevoegd. De werkruimte wordt geconfigureerd zodat Service Fabric-platformgebeurtenissen kunnen worden opgehaald uit de opslagtabellen die met de [Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md)-agent zijn geconfigureerd. De OMS-agent (Microsoft Monitoring Agent) is ook aan elk knooppunt in het cluster toegevoegd als een extensie van de virtuele machine. Dit betekent dat als het cluster wordt geschaald, de agent automatisch op elke machine wordt geconfigureerd en aan dezelfde werkruimte gekoppeld.

Implementeer de sjabloon met de nieuwe wijzigingen om het huidige cluster bij te werken. U ziet de Log Analytics-resources in uw resourcegroep wanneer deze is voltooid. Als het cluster klaar is, implementeert u er de containertoepassing in. In de volgende stap, wordt het bewaken van de containers ingesteld.

## <a name="add-the-container-monitoring-solution-to-your-log-analytics-workspace"></a>Container Monitoring Solution toevoegen aan de Log Analytics-werkruimte

Als u de Container-oplossing in uw werkruimte wilt instellen, zoekt u naar *Container Monitoring Solution* en maakt u een Containers-resource (onder de categorie Bewaking + Management).

![Containers-oplossing toevoegen](./media/service-fabric-tutorial-monitoring-wincontainers/containers-solution.png)

Als u er door de *Log Analytics-werkruimte* om wordt gevraagd, selecteert u de werkruimte die in uw resourcegroep is gemaakt en klikt u op **Maken**. Hierdoor wordt een *Container Monitoring Solution* aan de werkruimte toegevoegd en wordt automatisch het verzamelen van docker-logboeken en -statistieken gestart. 

Ga terug naar uw *resourcegroep*, waar u nu de pas toegevoegde bewakingsoplossing moet kunnen zien. Als u erin klikt, moet op de startpagina het aantal containerinstallatiekopieën dat wordt uitgevoerd, worden weergegeven. 

*Houd er rekening mee dat er 5 exemplaren van de fabrikam-container uit [deel 2](service-fabric-host-app-in-a-container.md) van de zelfstudie zijn uitgevoerd*

![Startpagina containeroplossing](./media/service-fabric-tutorial-monitoring-wincontainers/solution-landing.png)

Als u op **Container Monitor Solution** klikt, komt u terecht op een dashboard met meer details. Hier kunt u door meerdere panelen schuiven en query's in Log Analytics uitvoeren. 

*Vanaf september 2017 wordt de oplossing af en toe bijgewerkt. Negeer eventuele foutmeldingen over Kubernetes-gebeurtenissen, terwijl meerdere orchestrators in dezelfde oplossing worden geïntegreerd.*

Aangezien de agent docker-logboeken ophaalt, worden *stdout* en *stderr* standaard weergegeven. Als u naar rechts schuift, ziet u de voorraad, de status en de metrische gegevens met betrekking tot containerinstallatiekopieën. Ook ziet u query's die u kunt uitvoeren voor meer nuttige gegevens. 

![Dashboard Containeroplossing](./media/service-fabric-tutorial-monitoring-wincontainers/container-metrics.png)

Als u op een van deze panelen klikt, komt u terecht bij de Log Analytics-query die de weergegeven waarde genereert. Wijzig de query in *\** om alle verschillende soorten logboeken te bekijken die worden opgehaald. Hier kunt u query's uitvoeren of filteren op de prestaties en logboeken van de container, of Service Fabric-platformgebeurtenissen bekijken. De agenten zenden ook continu een heartbeat van elk knooppunt uit. Hieraan kunt u zien dat er nog steeds gegevens worden verzameld van alle computers als de clusterconfiguratie wordt gewijzigd.   

![Containerquery](./media/service-fabric-tutorial-monitoring-wincontainers/query-sample.png)

## <a name="configure-oms-agent-to-pick-up-performance-counters"></a>OMS-agent configureren voor het ophalen van prestatiemeteritems

Een ander voordeel van het gebruik van de OMS-agent is de mogelijkheid om de prestatiemeteritems te wijzigen die u wilt ophalen via de OMS-gebruikersinterface. U hoeft dan de Azure diagnoseagent niet te configureren en geen op een Resource Manager-sjabloon gebaseerde upgrade uit te voeren. Als u dit wilt doen, klikt u op **OMS-portal** op de startpagina van uw Container Monitoring- of Service Fabric-oplossing.

![OMS-portal](./media/service-fabric-tutorial-monitoring-wincontainers/oms-portal.png)

Hierna komt u terecht in de werkruimte in de OMS-portal, waar u uw oplossingen kunt zien, aangepaste dashboards kunt maken en de OMS-agent kunt configureren. 
* Klik op het **tandwiel** in de rechterbovenhoek van het scherm om het menu *Instellingen* te openen.
* Klik op **Verbonden bronnen** > **Windows Servers** om te controleren of er *5 Windows-computers verbonden* zijn.
* Klik op **Gegevens** > **Windows-prestatiemeteritems** om nieuwe prestatiemeteritems te zoeken en toe te voegen. Hier ziet u een lijst met aanbevelingen van Log Analytics voor prestatiemeteritems die u kunt verzamelen, evenals de optie om andere tellers te zoeken. Klik op **De geselecteerde prestatiemeteritems toevoegen** om de voorgestelde metrische gegevens te gaan zoeken.

    ![Prestatiemeteritems](./media/service-fabric-tutorial-monitoring-wincontainers/perf-counters.png)

Terug in Azure Portal **vernieuwt** u binnen enkele minuten de Container Monitoring-oplossing en komen de gegevens van *Computerprestaties* binnen. Hierdoor krijgt u inzicht in het gebruik van uw resources. U kunt deze metrische gegevens ook gebruiken om de juiste beslissingen te maken over het schalen van het cluster of om te bevestigen dat een cluster de belasting op de juiste wijze verdeelt.

*Opmerking: controleer of de tijdfilters goed zijn ingesteld voor deze metrische gegevens.* 

![Prestatiemeteritems 2](./media/service-fabric-tutorial-monitoring-wincontainers/perf-counters2.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Log Analytics configureren voor uw Service Fabric-cluster
> * Een Log Analytics-werkruimte gebruiken om logboeken te bekijken en te doorzoeken vanuit uw containers en knooppunten
> * De Log Analytics-agent configureren om metrische gegevens uit containers en knooppunten te halen

Nu u bewaking hebt ingesteld voor uw containertoepassing, kunt u het volgende doen:

* De Log Analytics instellen voor een Linux-cluster, volgens soortgelijke stappen als hierboven. Raadpleeg [Deze sjabloon](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux) om wijzigingen in de Resource Manager-sjabloon aan te brengen.
* Log Analytics configureren om [automatische waarschuwingen](../log-analytics/log-analytics-alerts.md) in te stellen voor detectie en diagnostiek.
* De lijst met [aanbevolen prestatiemeteritems](service-fabric-diagnostics-event-generation-perf.md) van Service Fabric onderzoeken om deze voor uw clusters te configureren.
* Vertrouwd raken met de functies [zoeken in logboeken en query's uitvoeren](../log-analytics/log-analytics-log-searches.md) die als onderdeel van Log Analytics worden aangeboden.