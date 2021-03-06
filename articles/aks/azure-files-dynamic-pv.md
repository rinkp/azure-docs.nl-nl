---
title: Azure-bestand met AKS gebruiken
description: Azure-schijven met AKS gebruiken
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/17/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 991db1fc32ae89ab04ca040cfb6e8d59ffe5262f
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="persistent-volumes-with-azure-files"></a>Permanente volumes met Azure-bestanden

Een permanente volume is een onderdeel van de opslag die is gemaakt voor gebruik in een cluster Kubernetes. Een permanente volume kan worden gebruikt door een of meer gehele product en kan worden dynamisch of statisch gemaakt. De Documentdetails van dit **dynamisch maken** van een Azure-bestandsshare als een permanente volume.

Zie voor meer informatie over Kubernetes permanente volumes, waaronder het maken van de statische, [Kubernetes permanente volumes][kubernetes-volumes].

## <a name="create-storage-account"></a>Een opslagaccount maken

Wanneer u een Azure-bestandsshare als een volume Kubernetes dynamisch maakt, kan een opslagaccount worden gebruikt als deze zich in dezelfde resourcegroep bevinden als het cluster AKS. Maak een opslagaccount in dezelfde resourcegroep bevinden als het cluster AKS indien nodig.

Om de juiste resourcegroep hebt geïdentificeerd, gebruiken de [az groepslijst] [ az-group-list] opdracht.

```azurecli-interactive
az group list --output table
```

Zoek naar een resourcegroep met een naam die lijkt op `MC_clustername_clustername_locaton`.

```
Name                                 Location    Status
-----------------------------------  ----------  ---------
MC_myAKSCluster_myAKSCluster_eastus  eastus      Succeeded
myAKSCluster                         eastus      Succeeded
```

Gebruik de [az storage-account maken] [ az-storage-account-create] opdracht voor het maken van het opslagaccount.

Het volgende voorbeeld bijwerken `--resource-group` met de naam van de resourcegroep en `--name` in een naam van uw keuze.

```azurecli-interactive
az storage account create --resource-group MC_myAKSCluster_myAKSCluster_eastus --name mystorageaccount --location eastus --sku Standard_LRS
```

## <a name="create-storage-class"></a>Opslagklasse maken

Een opslagklasse wordt gebruikt om te definiëren hoe een Azure-bestandsshare wordt gemaakt. Een specifieke storage-account kan worden opgegeven in de klasse. Als u een opslagaccount niet is opgegeven, een `skuName` en `location` moet worden opgegeven, en alle opslagaccounts in de gekoppelde resourcegroep worden geëvalueerd voor een overeenkomst.

Zie voor meer informatie over Kubernetes Opslagklassen voor Azure files [Kubernetes Opslagklassen][kubernetes-storage-classes].

Maak een bestand met de naam `azure-file-sc.yaml` en kopieer het volgende manifest. Update de `storageAccount` met de naam van uw doelopslagaccount.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
parameters:
  storageAccount: mystorageaccount
```

Maken van de opslagklasse met de [kubectl toepassen] [ kubectl-apply] opdracht.

```azurecli-interactive
kubectl apply -f azure-file-sc.yaml
```

## <a name="create-persistent-volume-claim"></a>Permanente volume claim maken

Een claim permanente volume (PVC) maakt gebruik van het opslagobject klasse te richten op dynamische wijze een Azure-bestandsshare.

De volgende YAML kan worden gebruikt voor het maken van een claim permanente volume `5GB` aan de grootte van `ReadWriteOnce` toegang. Zie voor meer informatie over toegangsmodi in de [Kubernetes permanente volume] [ access-modes] documentatie.

Maak een bestand met de naam `azure-file-pvc.yaml` en kopieer de volgende YAML. Zorg ervoor dat de `storageClassName` overeenkomt met de opslagklasse in de vorige stap hebt gemaakt.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azurefile
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: azurefile
  resources:
    requests:
      storage: 5Gi
```

Maken van de claim permanente volume met de [kubectl toepassen] [ kubectl-apply] opdracht.

```azurecli-interactive
kubectl apply -f azure-file-pvc.yaml
```

Zodra de voltooid, kunt u de bestandsshare wordt gemaakt. Een geheim Kubernetes wordt ook gemaakt waarin de verbindingsgegevens en referenties.

## <a name="using-the-persistent-volume"></a>Met behulp van de permanente volume

De volgende YAML maakt een schil die gebruikmaakt van de claim permanente volume `azurefile` koppelen van de Azure-bestandsshare op de `/mnt/azure` pad.

Maak een bestand met de naam `azure-pvc-files.yaml`, en kopieer de volgende YAML. Zorg ervoor dat de `claimName` overeenkomt met het PVC in de vorige stap hebt gemaakt.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/mnt/azure"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azurefile
```

Maken van de schil met de [kubectl toepassen] [ kubectl-apply] opdracht.

```azurecli-interactive
kubectl apply -f azure-pvc-files.yaml
```

U hebt nu een actieve schil met uw Azure-schijf gekoppeld in de `/mnt/azure` directory. Deze configuratie kan worden weergegeven bij de inspectie van uw schil via `kubectl describe pod mypod`.

## <a name="mount-options"></a>Koppelingsopties

Standaardwaarden fileMode en dirMode verschillen tussen versies Kubernetes zoals beschreven in de volgende tabel.

| versie | waarde |
| ---- | ---- |
| V1.6.x, v1.7.x | 0777 |
| v1.8.0-v1.8.5 | 0700 |
| V1.8.6 of hoger | 0755 |
| v1.9.0 | 0700 |
| V1.9.1 of hoger | 0755 |

Als u een cluster van versie 1.8.5 of hoger en dynamisch maken van het volume persistant met een opslagklasse, koppelpunt opties kunnen worden opgegeven op het opslagobject klasse. Het volgende voorbeeld wordt `0777`.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
parameters:
  skuName: Standard_LRS
```

Als u een cluster van versie 1.8.5 of hoger en statisch maken van het object van de volume persistant, koppelpunt opties moeten worden opgegeven voor de `PersistentVolume` object. Zie voor meer informatie over het maken van een volume persistant statisch [statische permanente Volumes][pv-static].

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: azurefile
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteMany
  azureFile:
    secretName: azure-secret
    shareName: azurefile
    readOnly: false
  mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
  ```

Als u een cluster van versie 1.8.0 - 1.8.4, een beveiligingscontext kan worden opgegeven met de `runAsUser` waarde ingesteld op `0`. Zie voor meer informatie over schil beveiligingscontext [configureren van een beveiligingscontext][kubernetes-security-context].

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Kubernetes permanente volumes met behulp van Azure-bestanden.

> [!div class="nextstepaction"]
> [Kubernetes-invoegtoepassing voor Azure-bestanden][kubernetes-files]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-describe]: https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_describe/
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/#azure-file
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[pv-static]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#static

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-group-list]: /cli/azure/group#az_group_list
[az-storage-account-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create
