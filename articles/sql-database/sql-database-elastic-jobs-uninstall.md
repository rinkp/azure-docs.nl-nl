---
title: Het hulpprogramma voor elastische database taken verwijderen
description: Informatie over het verwijderen van de elastische database taken onderdelen met de Azure portal van PowerShell.
services: sql-database
manager: craigg
author: stevestein
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: f5d0579cbb5f787ce08e2a2cea58d3c39a967970
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/20/2018
---
# <a name="uninstall-elastic-database-jobs-components"></a>Elastische Database taken onderdelen verwijderen
**Elastische Database taken** onderdelen kunnen worden verwijderd met de Azure-portal of PowerShell.

## <a name="uninstall-elastic-database-jobs-components-using-the-azure-portal"></a>Elastische taken databaseonderdelen met de Azure portal verwijderen
1. Open de [Azure Portal](https://portal.azure.com/).
2. Navigeer naar het abonnement dat bevat **elastische Database taken** onderdelen, namelijk het abonnement in welke elastische Database taken onderdelen zijn geïnstalleerd.
3. Klik op **Bladeren** en klik op **resourcegroepen**.
4. Selecteer de resourcegroep met de naam '__ElasticDatabaseJob'.
5. De resourcegroep verwijderen.

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a>Elastische taken databaseonderdelen met behulp van PowerShell verwijderen
1. Start een Microsoft Azure PowerShell-opdrachtvenster en navigeer naar de submap van de hulpprogramma's onder de map Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x: Type **cd extra**.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x* > cd-hulpprogramma's
2. De.\UninstallElasticDatabaseJobs.ps1 PowerShell-script uitvoeren.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools > deblokkeren bestand.\UninstallElasticDatabaseJobs.ps1 PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools >.\UninstallElasticDatabaseJobs.ps1

Of gewoon, voer het volgende script, ervan uitgaande dat standaard waarden waar gebruikt bij installatie van de onderdelen:

        $ResourceGroupName = "__ElasticDatabaseJob"
        Switch-AzureMode AzureResourceManager

        $resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
        if(!$resourceGroup)
        {
            Write-Host "The Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
            return
        }

        Write-Host "Removing the Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
        Remove-AzureResourceGroup -Name $ResourceGroupName -Force
        Write-Host "Completed removing the Azure Resource Group: $ResourceGroupName.  Elastic database job compoennts are now uninstalled."

## <a name="next-steps"></a>Volgende stappen
Zie opnieuw installeren van de elastische Database taken [installeren van de service van de taak elastische Database](sql-database-elastic-jobs-service-installation.md)

Zie voor een overzicht van de elastische Database taken [elastische Database taken overzicht](sql-database-elastic-jobs-overview.md).

<!--Image references-->


