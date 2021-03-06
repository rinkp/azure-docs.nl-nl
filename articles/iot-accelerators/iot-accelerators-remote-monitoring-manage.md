---
title: Beheer van apparaten in de oplossing voor externe controle - Azure | Microsoft Docs
description: Deze zelfstudie laat zien hoe u voor het beheren van apparaten die zijn verbonden met de oplossing voor externe controle.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 05/01/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 3ad6de2a0ebcd257ca90ea3c5c69988d4c1afd7a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="manage-and-configure-your-devices"></a>Uw apparaten beheren en configureren

Deze zelfstudie ziet het apparaat management mogelijkheden van de oplossing voor externe controle. De zelfstudie maakt ter introductie van het volgen van deze mogelijkheden gebruik van een scenario in de Contoso IoT-toepassing.

Contoso heeft besteld nieuwe machines uit te breiden een van de mogelijkheden om te verhogen van de uitvoer. Terwijl u op de nieuwe machine wacht moet worden geleverd, die u wilt uitvoeren van een simulatie om te controleren of het gedrag van uw oplossing. Als operator die u wilt beheren en configureren van de apparaten in de oplossing voor externe controle.

Bieden een uitbreidbare manier om te beheren en configureren van apparaten, gebruikt de oplossing voor externe controle IoT Hub functies zoals [taken](../iot-hub/iot-hub-devguide-jobs.md) en [methoden directe](../iot-hub/iot-hub-devguide-direct-methods.md). Zie voor meer informatie over hoe een apparaat-ontwikkelaar de methoden op een fysiek apparaat implementeert, [aanpassen van de externe controle oplossingsverbetering](iot-accelerators-remote-monitoring-customize.md).

In deze zelfstudie leert u het volgende:

>[!div class="checklist"]
> * Een gesimuleerd apparaat inrichten.
> * Test het gesimuleerde apparaat.
> * Apparaat methoden aanroepen vanuit de oplossing.
> * Een apparaat opnieuw te configureren.

## <a name="prerequisites"></a>Vereisten

Volg deze zelfstudie, moet u een geïmplementeerd exemplaar van de oplossing voor externe controle in uw Azure-abonnement.

Als u de oplossing voor externe controle nog niet hebt geïmplementeerd, maar u moet voltooien de [implementeren van de externe controle oplossingsverbetering](iot-accelerators-remote-monitoring-deploy.md) zelfstudie.

## <a name="add-a-simulated-device"></a>Een gesimuleerd apparaat toevoegen

Navigeer naar de **apparaten** pagina in de oplossing en kies vervolgens **+ nieuw apparaat**. In de **nieuw apparaat** deelvenster, kiest u **gesimuleerde**:

![Een gesimuleerd apparaat inrichten](./media/iot-accelerators-remote-monitoring-manage/devicesprovision.png)

Laat het aantal apparaten inrichten ingesteld op **1**. Kies de **defecte Engine** apparaat model en kies vervolgens **toepassen** voor het maken van het gesimuleerde apparaat:

![Een engine voor het gesimuleerde apparaat inrichten](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine.png)

Voor meer informatie over het inrichten van een *fysieke* apparaat, Zie [uw apparaat aansluit op de externe controle oplossingsverbetering](iot-accelerators-connecting-devices-node.md).

## <a name="test-the-simulated-device"></a>Het gesimuleerde apparaat testen

Om de details van uw nieuw gesimuleerd apparaat weergeven, selecteert u deze in de lijst met apparaten op de **apparaten** pagina. Informatie over uw apparaat wordt weergegeven in de **apparaat detail** Configuratiescherm:

![Het nieuwe engine voor het gesimuleerde apparaat weergeven](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew.png)

In **apparaat detail**, Controleer of het nieuwe apparaat verzenden van telemetrie. Als u wilt weergeven van de stromen verschillende telemetrie van uw apparaat, kies een naam telemetrie zoals **trillingen**:

![Selecteer een telemetriestroom om weer te geven](./media/iot-accelerators-remote-monitoring-manage/devicesvibration.png)

De **apparaat detail** deelvenster andere informatie over het apparaat zoals labelwaarden en de methoden die deze ondersteuning biedt voor de eigenschappen die zijn gerapporteerd door het apparaat weergegeven.

Als u wilt weergeven van gedetailleerde diagnostische gegevens, bladert u omlaag naar de weergave **Diagnostics**.

## <a name="act-on-a-device"></a>Reageren op een apparaat

Als u wilt uitvoeren op een of meer apparaten, selecteert u deze in de lijst met apparaten en kies vervolgens **taken**. De **Engine** Apparaatmodel bevat drie methoden die een apparaat moet ondersteunen:

![Engine-methoden](./media/iot-accelerators-remote-monitoring-manage/devicesmethods.png)

Kies **FillTank**, de taaknaam van de ingesteld op **FillEngineTank**, en kies vervolgens **toepassen**:

![Plannen van de methode opnieuw starten](./media/iot-accelerators-remote-monitoring-manage/devicesrestartengine.png)

De status van de taak volgen op de **onderhoud** pagina **taken**:

![De taak schema's bewaken](./media/iot-accelerators-remote-monitoring-manage/maintenancerestart.png)

### <a name="methods-in-other-devices"></a>Methoden in andere apparaten

Als u de verschillende gesimuleerde apparaattypen verkennen, ziet u dat andere apparaattypen verschillende methoden ondersteunen. In een implementatie met fysieke apparaten geeft het model met de methoden die moet worden ondersteund door het apparaat. De ontwikkelaar van het apparaat is meestal verantwoordelijk voor het ontwikkelen van de code waarmee het apparaat die fungeren als reactie op een methodeaanroep.

Als u een methode uit te voeren op meerdere apparaten plannen, kunt u meerdere apparaten in de lijst selecteert op de **apparaten** pagina. De **taken** deelvenster toont de typen van de methode voor de geselecteerde apparaten.

## <a name="reconfigure-a-device"></a>Een apparaat configureren

Als u de configuratie van een apparaat, selecteert u deze in de lijst met apparaten op de **apparaten** pagina en kies vervolgens **taken**, en kies vervolgens **configureren**. Het deelvenster Taken ziet u de waarden van de eigenschappen voor het geselecteerde apparaat die u kunt wijzigen:

![Een apparaat configureren](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure.png)

Als u wilt een wijziging aanbrengt, Voeg een naam voor de taak, werk de eigenschapswaarden en kiest u **toepassen**:

![De waarde van de eigenschap van een apparaat bijwerken](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical.png)

De status van de taak volgen op de **onderhoud** pagina **taken**.

## <a name="next-steps"></a>Volgende stappen

Deze zelfstudie hebt u geleerd hoe naar:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Een gesimuleerd apparaat inrichten.
> * Test het gesimuleerde apparaat.
> * Apparaat methoden aanroepen vanuit de oplossing.
> * Een apparaat opnieuw te configureren.

U hebt geleerd hoe u uw apparaten beheert, de voorgestelde volgende stappen zijn voor meer informatie over hoe:

* [Problemen oplossen en het oplossen van problemen met apparaat](iot-accelerators-remote-monitoring-maintain.md).
* [Testen van uw oplossing met gesimuleerde apparaten](iot-accelerators-remote-monitoring-test.md).
* [Uw apparaat aansluit op de externe controle oplossingsverbetering](iot-accelerators-connecting-devices-node.md).

<!-- Next tutorials in the sequence -->