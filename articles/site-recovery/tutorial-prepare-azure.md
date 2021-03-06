---
title: Resources maken voor gebruik met Azure Site Recovery | Microsoft Docs
description: Informatie over het voorbereiden van Azure voor replicatie van on-premises machines met behulp van Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 05/02/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 852c854de9feb9bcc98fc89aa9340b93f2c4e8d3
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="prepare-azure-resources-for-replication-of-on-premises-machines"></a>Azure-resources voorbereiden voor replicatie van lokale machines

 [Azure Site Recovery](site-recovery-overview.md) draagt bij aan uw strategie voor zakelijke continuïteit en noodherstel (BCDR) door te zorgen dat uw zakelijke apps actief blijven tijdens geplande en ongeplande uitval. Site Recovery beheert en orkestreert noodherstel van on-premises machines en virtuele Azure-machines (VM's), met inbegrip van replicatie, failover en herstel.

Deze zelfstudie laat zien hoe u Azure-onderdelen voorbereidt wanneer u on-premises virtuele machines (Hyper-V of VMware) of fysieke Windows- of Linux-servers wilt repliceren naar Azure. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Controleren of uw Azure-account replicatiemachtigingen heeft.
> * Een Azure-opslagaccount maken. Gegevens repliceren die erin zijn opgeslagen.
> * Maak een Recovery Services-kluis.
> * Een Azure-netwerk instellen. Wanneer de Azure VM's zijn gemaakt na de failover, worden ze gekoppeld aan dit Azure-netwerk.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij [Azure Portal](http://portal.azure.com).

## <a name="verify-account-permissions"></a>Accountmachtigingen controleren

Als u net pas uw gratis Azure-account hebt gemaakt, bent u de beheerder van uw abonnement. Als u niet de abonnementsbeheerder bent, neemt u contact op met de beheerder om de machtigingen te krijgen die u nodig hebt. Om replicatie in te kunnen schakelen voor een nieuwe virtuele machine, moet u de volgende machtigingen hebben:

- Het maken van een VM in de geselecteerde resourcegroep.
- Het maken van een VM in het geselecteerde virtuele netwerk.
- Schrijven naar het geselecteerde opslagaccount.

U kunt deze taken alleen uitvoeren als aan uw account de ingebouwde rol van Inzender voor virtuele machines is toegewezen. En als u Site Recovery-bewerkingen wilt beheren in een kluis, moet aan uw account ook de ingebouwde rol van Site Recovery-inzender zijn toegewezen.

## <a name="create-a-storage-account"></a>Create a storage account

Installatiekopieën van gerepliceerde machines worden bewaard in Azure Storage. Azure VM's worden gemaakt vanuit de opslag wanneer u een failover van on-premises naar Azure uitvoert.

1. Selecteer in het menu van [Azure Portal](https://portal.azure.com) **Nieuw** > **Opslag** > **Opslagaccount**.
2. Voer in **Opslagaccount maken** een naam voor het account in. Voor deze zelfstudies gebruiken we de naam **contosovmsacct1910171607**. De naam moet uniek zijn in Azure, tussen de 3 en 24 tekens lang zijn en mag alleen cijfers en kleine letters bevatten.
3. Selecteer bij **implementatiemodel** **Resource Manager**.
4. Selecteer bij **Soort account** **Algemeen gebruik**. Selecteer bij **Prestaties** **Standard**. Selecteer niet blob-opslag.
5. Selecteer bij **Replicatie** de standaardoptie **Geografisch redundante opslag met leestoegang** voor opslagredundantie.
6. Selecteer bij **Abonnement** het abonnement waarin u het nieuwe opslagaccount wilt maken.
7. Geef bij **Resourcegroep** een nieuwe resourcegroep op. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Gebruik voor deze zelfstudies de naam **ContosoRG**.
8. Selecteer bij **Locatie** de geografische locatie voor het opslagaccount. Het opslagaccount moet zich in dezelfde regio bevinden als de Recovery Services-kluis. Gebruik voor deze zelfstudie de regio **West-Europa**.

   ![Create a storage account](media/tutorial-prepare-azure/create-storageacct.png)

9. Selecteer **Maken** om het opslagaccount te maken.

## <a name="create-a-vault"></a>Een kluis maken

1. Selecteer in Azure Portal **Een resource maken** > **Controle en beheer** > **Backup en Site Recovery**.
2. Voer in **Naam** een beschrijvende naam in om de kluis aan te duiden. Gebruik voor deze zelfstudie **ContosoVMVault**.
3. Selecteer bij **Resourcegroep** de bestaande resourcegroep met de naam **contosoRG**.
4. Geef bij **Locatie** de Azure-regio **West-Europa** op, die we in deze reeks zelfstudies gebruiken.
5. Voor snelle toegang tot de kluis vanuit het dashboard, selecteert u **Aan dashboard vastmaken** > **Maken**.

   ![Een nieuwe kluis maken](./media/tutorial-prepare-azure/new-vault-settings.png)

   De nieuwe kluis wordt weergegeven in **Dashboard** > **Alle resources** en op de hoofdpagina **Recovery Services-kluizen**.

## <a name="set-up-an-azure-network"></a>Een Azure-netwerk instellen

Wanneer de Azure VM's zijn gemaakt vanuit de opslag na de failover, worden ze gekoppeld aan dit netwerk.

1. Selecteer in [Azure Portal](https://portal.azure.com) **Een resource maken** > **Netwerken** > **Virtueel netwerk**.
2. Laat **Resource Manager** geselecteerd als het implementatiemodel. Resource Manager is het voorkeursimplementatiemodel. Voer vervolgens deze stappen uit:

   a. Voer bij **Naam** een netwerknaam in. De naam moet uniek zijn binnen de Azure-resourcegroep. Gebruik de naam **ContosoASRnet**.

   b. Gebruik bij **Resourcegroep** de bestaande resourcegroep **contosoRG**.

   c. Voer bij **Adresbereik** het adresbereik in van het netwerk: **10.0.0.0/24**.

   d. Voor deze zelfstudie hebt u geen subnet nodig.

   e. Selecteer bij **Abonnement** het abonnement waarin u het netwerk wilt maken.

   f. Selecteer bij **Locatie** de optie **West-Europa**. Het netwerk moet zich in dezelfde regio bevinden als de Recovery Services-kluis.

3. Selecteer **Maken**.

   ![Een virtueel netwerk maken](media/tutorial-prepare-azure/create-network.png)

   Het duurt een paar seconden voordat het virtuele netwerk is gemaakt. Nadat dit is gemaakt, wordt het in het Azure Portal-dashboard weergegeven.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [De on-premises VMware-infrastructuur voorbereiden op herstel naar Azure na een noodgeval](tutorial-prepare-on-premises-vmware.md)
