---
title: 'Zelfstudie: Virtuele Azure-machines beheren met Azure CLI 2.0 | Microsoft Docs'
description: In deze zelfstudie leert u hoe u de Azure CLI 2.0 gebruikt voor het beheren van virtuele Azure-machines door RBAC, beleid, vergrendelingen en tags toe te passen
services: virtual-machines-linux
documentationcenter: virtual-machines
author: tfitzmac
manager: jeconnoc
editor: tysonn
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: tutorial
ms.date: 02/21/2018
ms.author: tomfitz
ms.custom: mvc
ms.openlocfilehash: 4ce2b133ed4266028f1d99151939538fb8ce60f5
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-learn-about-linux-virtual-machine-governance-with-azure-cli-20"></a>Zelfstudie: Meer informatie over het beheren van virtuele Linux-machines met Azure CLI 2.0

[!INCLUDE [Resource Manager governance introduction](../../../includes/resource-manager-governance-intro.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u Azure CLI 2.0.30 of hoger gebruiken voor deze zelfstudie. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="understand-scope"></a>Reikwijdte

[!INCLUDE [Resource Manager governance scope](../../../includes/resource-manager-governance-scope.md)]

In deze zelfstudie past u alle beheerinstellingen toe op een resourcegroep zodat u deze instellingen eenvoudig kunt verwijderen wanneer u klaar bent.

We gaan nu de resourcegroep maken.

```azurecli-interactive
az group create --name myResourceGroup --location "East US"
```

De resourcegroep is momenteel leeg.

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

U wilt er zeker van zijn dat gebruikers in uw organisatie het juiste toegangsniveau tot deze resources hebben. U wilt gebruikers geen onbeperkte toegang verlenen, maar u moet er ook voor zorgen dat ze hun werk kunnen doen. Met [toegangsbeheer op basis van rollen](../../role-based-access-control/overview.md) kunt u beheren welke gebruikers gemachtigd zijn specifieke acties binnen een bepaald bereik uit te voeren.

Om roltoewijzingen te maken en te verwijderen, moeten gebruikers `Microsoft.Authorization/roleAssignments/*`-toegang hebben. Deze toegang wordt verleend via de rol Eigenaar of Administrator voor gebruikerstoegang.

Voor het beheren van virtuele machine-oplossingen zijn er drie resourcespecifieke rollen die toegang bieden die gewoonlijk nodig is:

* [Inzender voor virtuele machines](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor)
* [Inzender voor netwerken](../../role-based-access-control/built-in-roles.md#network-contributor)
* [Inzender voor opslagaccounts](../../role-based-access-control/built-in-roles.md#storage-account-contributor)

In plaats van de rollen toe te wijzen aan individuele gebruikers, is het vaak eenvoudiger [een Azure Active Directory-groep te maken](../../active-directory/active-directory-groups-create-azure-portal.md) voor gebruikers die vergelijkbare acties moeten ondernemen. U wijst dan de juiste rol aan die groep toe. Ter vereenvoudiging van dit artikel maakt u een Azure Active Directory-groep zonder leden. U kunt aan deze groep wel een rol voor een bereik toewijzen. 

In het volgende voorbeeld wordt een Azure Active Directory-groep met de naam *VMDemoContributors* gemaakt met de e-mailbijnaam *vmDemoGroup*. De e-mailbijnaam fungeert als een alias voor de groep.

```azurecli-interactive
adgroupId=$(az ad group create --display-name VMDemoContributors --mail-nickname vmDemoGroup --query objectId --output tsv)
```

Nadat de opdrachtprompt is teruggekeerd, duurt het even voordat de groep is doorgegeven in Azure Active Directory. Gebruik nadat u 20 of 30 seconden hebt gewacht de opdracht [az role assignment create](/cli/azure/role/assignment#az_role_assignment_create) om de nieuwe Azure Active Directory-groep toe te wijzen aan de rol Inzender voor virtuele machines voor de resourcegroep.  Als u de volgende opdracht uitvoert voordat deze is doorgegeven, ontvangt u een foutbericht met de melding dat de **principal <guid> niet in de map bestaat**. Probeer de opdracht opnieuw uit te voeren.

```azurecli-interactive
az role assignment create --assignee-object-id $adgroupId --role "Virtual Machine Contributor" --resource-group myResourceGroup
```

Normaal gesproken herhaalt u het proces voor *Inzender voor netwerken* en *Inzender voor opslagaccounts* om ervoor te zorgen dat gebruikers worden toegewezen om de geïmplementeerde resources te beheren. In dit artikel kunt u deze stappen overslaan.

## <a name="azure-policies"></a>Azure-beleid

[!INCLUDE [Resource Manager governance policy](../../../includes/resource-manager-governance-policy.md)]

### <a name="apply-policies"></a>Beleid toepassen

Uw abonnement heeft al meerdere beleidsdefinities. Als u de beschikbare beleidsdefinities wilt bekijken, gebruikt u de opdracht [az policy definition list](/cli/azure/policy/definition#az_policy_definition_list):

```azurecli-interactive
az policy definition list --query "[].[displayName, policyType, name]" --output table
```

U ziet de bestaande beleidsdefinities. Het type beleid is **Ingebouwd** of **Aangepast**. Bekijk de definities voor beleid waarin een voorwaarde wordt beschreven die u wilt toewijzen. In dit artikel wijst u beleid toe waarmee:

* De locaties voor alle resources worden beperkt.
* De SKU's voor virtuele machines worden beperkt.
* Controleer virtuele machines die niet gebruikmaken van beheerde schijven.

In het volgende voorbeeld haalt u drie beleidsdefinities op basis van de weergavenaam op. U gebruikt de opdracht [az policy assignment create](/cli/azure/policy/assignment#az_policy_assignment_create) om deze definities toe te wijzen aan de resourcegroep. Voor sommige beleidsregels kunt u parameterwaarden opgeven om de toegestane waarden te specificeren.

```azurecli-interactive
# Get policy definitions for allowed locations, allowed SKUs, and auditing VMs that don't use managed disks
locationDefinition=$(az policy definition list --query "[?displayName=='Allowed locations'].name | [0]" --output tsv)
skuDefinition=$(az policy definition list --query "[?displayName=='Allowed virtual machine SKUs'].name | [0]" --output tsv)
auditDefinition=$(az policy definition list --query "[?displayName=='Audit VMs that do not use managed disks'].name | [0]" --output tsv)

# Assign policy for allowed locations
az policy assignment create --name "Set permitted locations" \
  --resource-group myResourceGroup \
  --policy $locationDefinition \
  --params '{ 
      "listOfAllowedLocations": {
        "value": [
          "eastus", 
          "eastus2"
        ]
      }
    }'

# Assign policy for allowed SKUs
az policy assignment create --name "Set permitted VM SKUs" \
  --resource-group myResourceGroup \
  --policy $skuDefinition \
  --params '{ 
      "listOfAllowedSKUs": {
        "value": [
          "Standard_DS1_v2", 
          "Standard_E2s_v2"
        ]
      }
    }'

# Assign policy for auditing unmanaged disks
az policy assignment create --name "Audit unmanaged disks" \
  --resource-group myResourceGroup \
  --policy $auditDefinition
```

In het voorgaande voorbeeld wordt ervan uitgegaan dat u al weet wat de parameters voor een beleid zijn. Als u de parameters moet weergeven, gebruikt u:

```azurecli-interactive
az policy definition show --name $locationDefinition --query parameters
```

## <a name="deploy-the-virtual-machine"></a>De virtuele machine implementeren

U hebt rollen en beleid hebt toegewezen. U bent nu dus klaar om uw oplossing te implementeren. De standaardgrootte is Standard_DS1_v2. Dit is een van de toegestane SKU's. Met de opdracht maakt u SSH-sleutels als deze niet bestaan op een standaardlocatie.

```azurecli-interactive
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys
```

Nadat de implementatie is voltooid, kunt u meer beheerinstellingen toepassen op de oplossing.

## <a name="lock-resources"></a>Resources vergrendelen

Met [resourcevergrendelingen](../../azure-resource-manager/resource-group-lock-resources.md) voorkomt u dat gebruikers in uw organisatie per ongeluk kritieke bronnen wijzigen of verwijderen. In tegenstelling tot toegangsbeheer op basis van rollen wordt met resourcevergrendelingen een beperking toegepast op alle gebruikers en rollen. U kunt de vergrendeling instellen op *CanNotDelete* of *ReadOnly*.

Als u beheervergrendelingen wilt maken of verwijderen, moet u toegang hebben tot `Microsoft.Authorization/locks/*`-acties. Van de ingebouwde rollen worden deze acties alleen toegekend aan **Eigenaar** en **Administrator voor gebruikerstoegang**.

Als u de virtuele machine en de netwerkbeveiligingsgroep wilt vergrendelen, gebruikt u de opdracht [az lock create](/cli/azure/lock#az_lock_create):

```azurecli-interactive
# Add CanNotDelete lock to the VM
az lock create --name LockVM \
  --lock-type CanNotDelete \
  --resource-group myResourceGroup \
  --resource-name myVM \
  --resource-type Microsoft.Compute/virtualMachines

# Add CanNotDelete lock to the network security group
az lock create --name LockNSG \
  --lock-type CanNotDelete \
  --resource-group myResourceGroup \
  --resource-name myVMNSG \
  --resource-type Microsoft.Network/networkSecurityGroups
```

Voer de volgende opdracht uit om de vergrendelingen te testen:

```azurecli-interactive 
az group delete --name myResourceGroup
```

Er wordt een fout weergegeven met de melding dat de verwijderbewerking niet kan worden uitgevoerd vanwege een vergrendeling. De resourcegroep kan alleen worden verwijderd als u de vergrendelingen specifiek verwijdert. Deze stap wordt weergegeven in [Resources opschonen](#clean-up-resources).

## <a name="tag-resources"></a>Resources taggen

U past [tags](../../azure-resource-manager/resource-group-using-tags.md) toe op uw Azure-resources om deze logisch te ordenen in categorieën. Elke tag bestaat uit een naam en een waarde. U kunt de naam Omgeving en de waarde Productie bijvoorbeeld toepassen op alle resources in de productie.

[!INCLUDE [Resource Manager governance tags CLI](../../../includes/resource-manager-governance-tags-cli.md)]

Voor het toepassen van tags op een virtuele machine gebruikt u de opdracht [az resource tag](/cli/azure/resource#az_resource_tag). Eventuele bestaande tags in de resource blijven niet behouden.

```azurecli-interactive
az resource tag -n myVM \
  -g myResourceGroup \
  --tags Dept=IT Environment=Test Project=Documentation \
  --resource-type "Microsoft.Compute/virtualMachines"
```

### <a name="find-resources-by-tag"></a>Resources zoeken op tag

Als u naar resources met een tagnaam en -waarde wilt zoeken, gebruikt u de opdracht [az resource list](/cli/azure/resource#az_resource_list):

```azurecli-interactive
az resource list --tag Environment=Test --query [].name
```

U kunt de geretourneerde waarden gebruiken voor beheertaken zoals het stoppen van alle virtuele machines met een tagwaarde.

```azurecli-interactive
az vm stop --ids $(az resource list --tag Environment=Test --query "[?type=='Microsoft.Compute/virtualMachines'].id" --output tsv)
```

### <a name="view-costs-by-tag-values"></a>Kosten weergeven op tagwaarden

[!INCLUDE [Resource Manager governance tags billing](../../../includes/resource-manager-governance-tags-billing.md)]

## <a name="clean-up-resources"></a>Resources opschonen

De vergrendelde netwerkbeveiligingsgroep kan pas worden verwijderd nadat de vergrendeling is verwijderd. Als u de vergrendelingen wilt verwijderen, haalt u de id van de vergrendelingen op en voert u er de opdracht [az lock delete](/cli/azure/lock#az_lock_delete) voor uit:

```azurecli-interactive
vmlock=$(az lock show --name LockVM \
  --resource-group myResourceGroup \
  --resource-type Microsoft.Compute/virtualMachines \
  --resource-name myVM --output tsv --query id)
nsglock=$(az lock show --name LockNSG \
  --resource-group myResourceGroup \
  --resource-type Microsoft.Network/networkSecurityGroups \
  --resource-name myVMNSG --output tsv --query id)
az lock delete --ids $vmlock $nsglock
```

U kunt de opdracht [az group delete](/cli/azure/group#az_group_delete) gebruiken om de resourcegroep, de VM en alle gerelateerde resources te verwijderen wanneer u ze niet meer nodig hebt. Sluit SSH-sessie met uw virtuele machine af en verwijder vervolgens de resources als volgt:

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een aangepaste installatiekopie voor een virtuele machine gemaakt. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Gebruikers toewijst aan een rol
> * Beleidsregels toepast die standaarden afdwingen
> * Kritieke resources beveiligt met vergrendelingen
> * Resources tagt voor facturering en beheer

Ga door met de volgende zelfstudie voor meer informatie over virtuele machines met hoge beschikbaarheid.

> [!div class="nextstepaction"]
> [Virtuele machines bewaken](tutorial-monitoring.md)

