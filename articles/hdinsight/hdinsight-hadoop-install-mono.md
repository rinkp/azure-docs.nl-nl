---
title: Installeren of bijwerken van Mono op HDInsight - Azure | Microsoft Docs
description: Informatie over het gebruik van een specifieke versie van Mono met HDInsight-cluster. Mono wordt gebruikt voor het uitvoeren van .NET-toepassingen op Linux gebaseerde HDInsight-clusters.
services: hdinsight
documentationCenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/10/2018
ms.author: larryfr
ms.custom: hdinsightactive
ms.openlocfilehash: 165f1d8175c7c7b58a5eec02a208b81fe73cb5f9
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="install-or-update-mono-on-hdinsight"></a>Installeren of bijwerken van Mono in HDInsight

Informatie over het installeren van een specifieke versie van [Mono](https://www.mono-project.com) op HDInsight 3.4 of hoger.

Mono op HDInsight 3.4 en hoger is geïnstalleerd en wordt gebruikt voor het uitvoeren van .NET-toepassingen. Zie voor informatie over de versie van Mono opgenomen in elke versie van HDInsight, [versiebeheer van HDInsight-onderdeel](hdinsight-component-versioning.md). Gebruik voor het installeren van een andere versie op het cluster, de scriptactie in dit document. 

## <a name="how-it-works"></a>Hoe werkt het?

Dit script accepteert de volgende parameter:

* __Mono versienummer__: de versie van Mono te installeren. De versie moet beschikbaar zijn vanuit [ https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/ ](https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/).

Het script wordt geïnstalleerd voor de volgende Mono-pakketten:

* __Mono-voltooid__

* __ca-certificates-mono__

## <a name="the-script"></a>Het script

__Locatie script__: [https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash](https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash)

__Vereisten__:

* Het script moet worden toegepast op de __hoofdknooppunten__ en __worker-knooppunten__.

## <a name="to-use-the-script"></a>Het script gebruiken

Zie voor meer informatie over het gebruik van dit script met HDInsight de [aanpassen Linux gebaseerde HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) document. U kunt het script via de Azure-portal, Azure PowerShell of Azure CLI gebruiken.

Tijdens het script actie document te volgen, gebruikt u de volgende URI:

    https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash

Om op te geven de Mono-versie die is geïnstalleerd, gebruikt u het versienummer in de __Parameters__ veld. Voer bijvoorbeeld `5.4` Mono 5.4 installeren.

> [!NOTE]
> Bij het configureren van HDInsight met dit script, markeert u het script als __persistente__. Deze instelling kunt HDInsight het script op de worker-knooppunten die zijn toegevoegd door te schalen bewerkingen toepassen.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u wilt upgraden of installeren van een specifieke versie van Mono op HDInsight. Zie de volgende documenten voor meer informatie over het gebruik van .NET-toepassingen met Mono op HDInsight:

* [.NET gebruiken voor het streamen van MapReduce in HDInsight](hadoop/apache-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [.NET met Hive en Pig in HDInsight gebruiken](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)
* [C#-oplossingen met Storm op HDInsight ontwikkelen](storm/apache-storm-develop-csharp-visual-studio-topology.md)
* [.NET-oplossingen migreren naar HDInsight op basis van Linux](hdinsight-hadoop-migrate-dotnet-to-linux.md)

Zie voor meer informatie over het gebruik van scriptacties [aanpassen Linux gebaseerde HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster-linux.md)