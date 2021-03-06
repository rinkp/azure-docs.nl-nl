---
title: Problemen met System Center Data Protection Manager met Azure Backup | Microsoft Docs
description: Oplossen van problemen in System Center Data Protection Manager.
services: backup
documentationcenter: ''
author: adigan
manager: shreeshd
editor: ''
ms.assetid: 2d73c349-0fc8-4ca8-afd8-8c9029cb8524
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: pullabhk;markgal;adigan
ms.openlocfilehash: 8f8117b216dcbda217bc433e643090e8eb47cf57
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="troubleshoot-system-center-data-protection-manager"></a>Problemen oplossen met System Center Data Protection Manager

In dit artikel beschrijft oplossingen voor problemen die u tegenkomen kunt tijdens het gebruik van Data Protection Manager.

Zie voor de meest recente release-opmerkingen voor System Center Data Protection Manager, de [documentatie voor System Center](https://docs.microsoft.com/system-center/dpm/dpm-release-notes?view=sc-dpm-2016). U kunt meer informatie over ondersteuning voor Data Protection Manager in [deze matrix](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-2016).


## <a name="error-replica-is-inconsistent"></a>Fout: De Replica is inconsistent

Een replica is inconsistent voor de volgende redenen:
- De taak voor het maken van replica is mislukt.
- Er zijn problemen met het wijzigingslogboek.
- Volume-filter op niveau bitmap bevat fouten.
- De bronmachine is onverwacht afgesloten.
- Het synchronisatielogboek overloop.
- De replica is echt inconsistent.

U lost dit probleem, moet u de volgende acties uitvoeren:
- Verwijder de inconsistente status, de consistentiecontrole handmatig uitvoeren of een dagelijkse consistentiecontrole plannen.
- Zorg ervoor dat u de meest recente versie van Microsoft Azure Backup-Server en Data Protection Manager.
- Zorg ervoor dat de **automatische consistentiecontroles** instelling is ingeschakeld.
- Probeer het opnieuw opstarten van de services vanaf de opdrachtprompt. Gebruik de `net stop dpmra` opdracht, gevolgd door `net start dpmra`.
- Zorg ervoor dat u aan de netwerkvereisten voor verbindingen en bandbreedte voldoet.
- Controleer als de bronmachine onverwacht werd afgesloten.
- Zorg ervoor dat de schijf in orde is en of er voldoende ruimte voor de replica.
- Zorg ervoor dat er geen dubbele back-uptaken die gelijktijdig worden uitgevoerd.

## <a name="error-online-recovery-point-creation-failed"></a>Fout: Online herstelpunt maken mislukt vanwege

U lost dit probleem, moet u de volgende acties uitvoeren:
- Zorg ervoor dat u de nieuwste versie van de Azure Backup agent.
- Wilt u handmatig een herstelpunt maken in het taakgebied beveiliging.
- Zorg ervoor dat u een consistentiecontrole op gegevensbron uitvoeren.
- Zorg ervoor dat u aan de netwerkvereisten voor verbindingen en bandbreedte voldoet.
- Als de replicagegevens een inconsistente status heeft, maak een schijfherstelpunt van deze gegevensbron.
- Zorg ervoor dat de replica aanwezig zijn en niet ontbreekt.
- Zorg ervoor dat de replica voldoende ruimte voor het maken van het journaal update sequence USN (number).

## <a name="error-unable-to-configure-protection"></a>Fout: Kan geen beveiliging configureren

Deze fout treedt op wanneer de Data Protection Manager-server kan geen verbinding maken met de beveiligde server. 

U lost dit probleem, moet u de volgende acties uitvoeren:
- Zorg ervoor dat u de nieuwste versie van de Azure Backup agent.
- Zorg ervoor dat er verbinding (firewall-netwerk/proxy) tussen de Data Protection Manager-server en de beveiligde server is.
- Als u een SQL-server beveiligt, zorg ervoor dat de **Aanmeldingseigenschappen** > **NT AUTHORITY\SYSTEM** eigenschap bevat de **sysadmin** instelling is ingeschakeld.

## <a name="error-server-not-registered-as-specified-in-vault-credential-file"></a>Fout: Server niet geregistreerd zoals opgegeven in het kluisreferentiebestand

Deze fout treedt op tijdens het herstelproces voor Data Protection Manager/Azure Backup-server-gegevens. Het kluisreferentiebestand die wordt gebruikt in het herstelproces behoort niet tot de Recovery Services-kluis voor de Data Protection Manager/Azure Backup-server.

U lost dit probleem, moet u deze stappen uitvoeren:
1. Download het kluisreferentiebestand uit de Recovery Services-kluis waarnaar de Data Protection Manager/Azure Backup-server is geregistreerd.
2. Probeer de server met de kluis te registreren met behulp van het meest recent gedownloade kluisreferentiebestand.

## <a name="error-no-recoverable-data-or-selected-server-not-a-data-protection-manager-server"></a>Fout: Er is geen herstelbare gegevens of de geselecteerde server niet in een Data Protection Manager-server

Deze fout treedt op om de volgende redenen:
- Er zijn geen andere back-up van Data Protection Manager/Azure-servers geregistreerd bij de Recovery Services-kluis.
- De servers nog niet nog de metagegevens geüpload.
- De geselecteerde server is niet een back-up van Data Protection Manager/Azure-server.

Wanneer andere back-up van Data Protection Manager/Azure-servers zijn geregistreerd bij de Recovery Services-kluis, voer deze stappen om het probleem te verhelpen:
1. Zorg ervoor dat de meest recente Azure Backup-agent is geïnstalleerd.
2. Nadat u ervoor te zorgen dat de nieuwste agent is geïnstalleerd, wacht u één dag voordat u begint met het herstelproces. De metagegevens voor alle beveiligde back-ups uploadt de nachtelijke back-uptaak naar de cloud. De back-upgegevens is vervolgens beschikbaar voor herstel.

## <a name="error-provided-encryption-passphrase-doesnt-match-passphrase-for-server"></a>Fout: De wachtwoordzin voor versleuteling opgegeven wachtwoordzin voor de server komt niet overeen

Deze fout treedt op tijdens het versleutelingsproces tijdens het herstellen van gegevens van de Data Protection Manager/Azure Backup-server. De wachtwoordzin voor versleuteling die wordt gebruikt in het herstelproces komt niet overeen met de wachtwoordzin voor versleuteling van de server. Als gevolg hiervan de agent de gegevens niet decoderen en het herstel is mislukt.

> [!IMPORTANT]
> Als u vergeet of de wachtwoordzin voor versleuteling kwijtraakt, zijn er geen andere methoden voor het herstellen van de gegevens. De enige optie is de wachtwoordzin opnieuw genereren. Gebruik de nieuwe wachtwoordzin voor het versleutelen van toekomstige back-upgegevens.
>
> Wanneer u gegevens herstelt, moet u altijd de dezelfde wachtwoordzin voor versleuteling die is gekoppeld aan de Data Protection Manager/Azure Backup-server opgeven. 
>
