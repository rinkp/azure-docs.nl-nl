---
title: Een Site Recovery-kluis verwijderen
description: Informatie over het verwijderen van een Azure Site Recovery-kluis op basis van de Site Recovery-scenario.
service: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: rajani-janaki-ram
ms.openlocfilehash: 1c069e8b09f71ac46017974dfd94945c404b16ba
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="delete-a-site-recovery-vault"></a>Een Site Recovery-kluis verwijderen
Afhankelijkheden kunnen voorkomen dat u een Azure Site Recovery-kluis verwijderen. De acties die u moet nemen afhankelijk van het scenario met Site Recovery: VMware naar Azure, Hyper-V (met en zonder System Center Virtual Machine Manager) voor Azure en Azure Backup. Zie het verwijderen van een gebruikt in Azure Backup-kluis [verwijderen van een back-upkluis in Azure](../backup/backup-azure-delete-vault.md).



## <a name="delete-a-site-recovery-vault"></a>Een Site Recovery-kluis verwijderen 
Volg de aanbevolen stappen voor uw scenario voor het verwijderen van de kluis.

### <a name="vmware-vms-to-azure"></a>VMware-VM’s naar Azure

1. Verwijder alle virtuele machines beveiligd door de stappen in [Schakel de beveiliging voor een VMware](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure).

2. Beleidsregels voor alle replicatie verwijderen door de stappen in [verwijderen van een beleid voor wachtwoordreplicatie](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy).

3. Verwijzingen naar vCenter verwijderen door de stappen in [een vCenter-server verwijderen](vmware-azure-manage-vcenter.md#delete-a-vcenter-server).

4. De configuratieserver verwijderen door de stappen in [een configuratieserver buiten gebruik stellen](vmware-azure-manage-configuration-server.md#delete-or-unregister-a-configuration-server).

5. Verwijder de kluis.


### <a name="hyper-v-vms-with-virtual-machine-manager-to-azure"></a>Hyper-V-machines (met Virtual Machine Manager) naar Azure
1. Verwijder alle virtuele machines beveiligd door de stappen in[Schakel de beveiliging voor een Hyper-V virtuele machines repliceren naar Azure met behulp van de System Center VMM voor Azure scenario](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario).

2. Koppeling & verwijderen van alle beleidsregels voor replicatie door naar uw kluis te bladeren -> **Site Recovery-infrastructuur** -> **voor System Center VMM** -> **replicatie Beleid**

3.  Verwijzingen naar Virtual Machine Manager-servers verwijderen door de stappen in [Hef de registratie van een VMM-server verbonden](site-recovery-manage-registration-and-protection.md##unregister-a-vmm-server).

4.  Verwijder de kluis.

### <a name="hyper-v-vms-without-virtual-machine-manager-to-azure"></a>Hyper-V-machines (zonder Virtual Machine Manager) naar Azure
1. Verwijder alle virtuele machines beveiligd door de stappen in [Schakel de beveiliging voor een Hyper-V virtuele machine (Hyper-V naar Azure)](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-hyper-v-to-azure).

2. Koppeling & verwijderen van alle beleidsregels voor replicatie door naar uw kluis te bladeren -> **Site Recovery-infrastructuur** -> **voor Hyper-V-Sites** -> **beleidsregels voor replicatie**

3. Verwijzingen naar Hyper-V-servers verwijderen door de stappen in [Hef de registratie van een Hyper-V-host](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).

4. De Hyper-V-site verwijderen.

5. Verwijder de kluis.


## <a name="use-powershell-to-force-delete-the-vault"></a>Gebruik PowerShell om af te dwingen de kluis verwijderen 

> [!Important]
> Als u het product test en niet betrokken zijn bij verlies van gegevens, gebruikt u de delete-methode force snel de kluis en de afhankelijkheden ervan verwijderen.
> De PowerShell-opdracht wordt de inhoud van de kluis verwijderd en is **niet omkeerbaar**.

Voor het verwijderen van de Site Recovery-kluis zelfs als er beveiligde items, moet u deze opdrachten gebruiken:

    Connect-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzureRmRecoveryServicesVault -Name "vaultname"

    Remove-AzureRmRecoveryServicesVault -Vault $vault

Meer informatie over [Get-AzureRMRecoveryServicesVault](https://docs.microsoft.com/en-us/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault?view=azurermps-6.0.0), en [verwijderen AzureRMRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/remove-azurermrecoveryservicesvault?view=azurermps-6.0.0).
