---
title: 'Snelstartgids: Telemetrie verzenden naar Azure IoT Hub (Node.js) | Microsoft Docs'
description: In deze snelstartgids voert u twee Node.js-voorbeeldtoepassingen uit om gesimuleerde telemetrie te verzenden naar een IoT-hub en telemetrie van de IoT-hub te lezen voor verwerking in de cloud.
services: iot-hub
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-hub
ms.devlang: node
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: ns
ms.date: 04/30/2018
ms.author: dobett
ms.openlocfilehash: 0be67b936780d68198a00cb99529643d453621e3
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-the-telemetry-from-the-hub-with-a-back-end-application-nodejs"></a>Snelstartgids: Telemetrie vanaf een apparaat verzenden naar een IoT-hub en de telemetrie lezen uit de hub met een back-endtoepassing (Node.js)

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub is een Azure-service waarmee grote hoeveelheden telemetrie van uw IoT-apparaten naar de cloud kunt opnemen voor opslag of verwerking. In deze snelstartgids verzendt u telemetrie vanuit een toepassing voor een gesimuleerd apparaat via IoT Hub naar een back-endtoepassing voor verwerking.

In de snelstartgids worden twee vooraf geschreven Node.js-toepassingen gebruikt, één voor het verzenden van de telemetrie en één voor het lezen van de telemetrie van de hub. Voordat u deze twee toepassingen kunt uitvoeren, moet u een IoT-hub maken en een apparaat registreren bij de hub.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

De twee voorbeeldtoepassingen die u uitvoert in deze snelstartgids zijn geschreven in Node.js. Node.js v4.x.x of hoger moet zijn geïnstalleerd op uw ontwikkelcomputer.

U kunt Node.js voor meerdere platforms downloaden van [nodejs.org](https://nodejs.org).

Gebruik de volgende opdracht om de huidige versie van Node.js op uw ontwikkelcomputer te controleren:

```cmd/sh
node --version
```

Download het Node.js-voorbeeldproject van https://github.com/Azure-Samples/azure-iot-samples-node/archive/master.zip en pak het ZIP-archief uit.

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-quickstarts-create-hub](../../includes/iot-hub-quickstarts-create-hub.md)]

## <a name="register-a-device"></a>Een apparaat registreren

Een apparaat moet zijn geregistreerd bij uw IoT-hub voordat het verbinding kan maken. In deze snelstart gebruikt u de Azure-CLI om een gesimuleerd apparaat te registreren.

1. Voeg de IoT Hub CLI-extensie toe en maak de apparaatidentiteit. Vervang `{YourIoTHubName}` door de naam die u hebt gekozen voor uw IoT-hub:

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyNodeDevice
    ```

    Als u een andere naam voor het apparaat kiest, werkt u de apparaatnaam bij in de voorbeeldtoepassingen voordat u ze uitvoert.

1. Voer de volgende opdracht uit om de _apparaatverbindingsreeks_ op te halen voor het apparaat dat u zojuist hebt geregistreerd:

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyNodeDevice --output table
    ```

    Noteer de apparaatverbindingsreeks, die er ongeveer zo uitziet: `Hostname=...=`. U gebruikt deze waarde verderop in de snelstartgids.

1. U hebt ook een _service-verbindingsreeks_ nodig, zodat de back-end-toepassing verbinding kan maken met de IoT-hub en de berichten kan ophalen. Met de volgende opdracht haalt u de serviceverbindingsreeks voor uw IoT-hub op:

    ```azurecli-interactive
    az iot hub show-connection-string --hub-name {YourIoTHubName} --output table
    ```

    Noteer de serviceverbindingsreeks, die er ongeveer zo uitziet: `Hostname=...=`. U gebruikt deze waarde verderop in de snelstartgids. De verbindingsreeks voor de service is iets anders dan de verbindingsreeks voor het apparaat.

## <a name="send-simulated-telemetry"></a>Gesimuleerde telemetrie verzenden

De toepassing voor het gesimuleerde apparaat maakt verbinding met een apparaatspecifiek eindpunt op uw IoT-hub en verstuurt gesimuleerde telemetrie over temperatuur en luchtvochtigheid.

1. Navigeer in een terminalvenster naar de hoofdmap van het voorbeeldproject in Node.js. Navigeer vervolgens naar de map **Quickstarts\simulated-device**.

1. Open het bestand **SimulatedDevice.js** in een teksteditor van uw keuze.

    Vervang de waarde van de variabele `connectionString` door de apparaatverbindingsreeks die u eerder hebt genoteerd. Sla ten slotte de wijzigingen in **SimulatedDevice.js** op.

1. Voer in het terminalvenster de volgende opdrachten uit om de vereiste bibliotheken te installeren en de toepassing voor het gesimuleerde apparaat uit te voeren:

    ```cmd/sh
    npm install
    node SimulatedDevice.js
    ```

    In de volgende schermafbeelding ziet u de uitvoer op het moment dat de toepassing voor het gesimuleerde apparaat telemetriegegevens naar uw IoT-hub verzendt:

    ![Het gesimuleerde apparaat uitvoeren](media/quickstart-send-telemetry-node/SimulatedDevice.png)

## <a name="read-the-telemetry-from-your-hub"></a>De telemetrie van uw hub lezen

De back-endtoepassing maakt verbinding met het eindpunt **Events** aan de servicezijde van uw IoT-hub. De toepassing ontvangt de berichten die van het gesimuleerde apparaat naar de cloud worden verzonden. Een back-endtoepassing van IoT Hub wordt meestal uitgevoerd in de cloud om berichten van apparaat naar cloud te ontvangen en verwerken.

1. Navigeer in een ander terminalvenster naar de hoofdmap van het voorbeeldproject in Node.js. Navigeer vervolgens naar de map **read-d2c-messages**.

1. Open het bestand **Quickstarts\ReadDeviceToCloudMessages.js** in een teksteditor van uw keuze.

    Vervang de waarde van de variabele `connectionString` door de service-verbindingsreeks die u eerder hebt genoteerd. Sla de wijzigingen in het bestand **ReadDeviceToCloudMessages.js** op.

1. Voer in het terminalvenster de volgende opdrachten uit om de vereiste bibliotheken te installeren en de back-endtoepassing uit te voeren:

    ```cmd/sh
    npm install
    node ReadDeviceToCloudMessages.js
    ```

    In de volgende schermafbeelding ziet u de uitvoer op het moment dat de back-endtoepassing telemetriegegevens ontvangt die door het gesimuleerde apparaat naar de hub zijn verzonden:

    ![De back-endtoepassing uitvoeren](media/quickstart-send-telemetry-node/ReadDeviceToCloud.png)

## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent om de volgende snelstartgids uit te voeren, hebt u de resourcegroep en IoT-hub nog nodig.

Als u de IoT-hub niet langer nodig hebt, verwijdert u deze en de resourcegroep in de portal. Dit doet u door de resourcegroep **qs-iot-hub-rg** met de IoT-hub te selecteren en op **Verwijderen** te klikken.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een IoT-hub geconfigureerd, een apparaat geregistreerd, gesimuleerde telemetrie verzonden naar de hub met behulp van een Node.js-toepassing en de telemetrie weer gelezen van de hub met behulp van een eenvoudig hulpprogramma.

Ga verder met de volgende snelstartgids als u wilt weten hoe u een gesimuleerd apparaat beheert vanuit een back-endtoepassing.

> [!div class="nextstepaction"]
> [Snelstartgids: Een apparaat beheren dat is verbonden met een IoT-hub](quickstart-control-device-node.md)