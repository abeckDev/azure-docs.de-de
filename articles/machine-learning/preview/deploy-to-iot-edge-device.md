---
title: "Bereitstellen eines Azure Machine Learning-Modells auf einem Azure IoT Edge-Gerät | Microsoft-Dokumentation"
description: "Dieses Dokument beschreibt, wie Azure Machine Learning-Modelle für Azure IoT Edge-Geräte bereitgestellt werden können."
services: machine-learning
author: tedway
ms.author: tedway
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 2/1/2018
ms.openlocfilehash: ceab96b1ef28527c8aa2692b83d3609f133f339c
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2018
---
# <a name="deploy-an-azure-machine-learning-model-to-an-azure-iot-edge-device"></a>Bereitstellen eines Azure Machine Learning-Modells auf einem Azure IoT Edge-Gerät

Alle als Docker-basierte Webdienste containerisierten Azure Machine Learning-Modelle können auch auf Azure IoT Edge-Geräten ausgeführt werden. Zusätzliche Skripts und Anweisungen finden Sie im [AI-Toolkit für Azure IoT Edge](http://aka.ms/AI-toolkit).

## <a name="operationalize-the-model"></a>Operationalisieren des Modells
Operationalisieren Sie Ihr Modell entsprechend der Anleitung in [Bereitstellen eines Machine Learning-Modells als Webdienst](model-management-service-deploy.md), um ein Docker-Image mit Ihrem Modell zu erstellen.

## <a name="deploy-to-azure-iot-edge"></a>Bereitstellen für Azure IoT Edge
Azure IoT Edge verschiebt Cloudanalysen und benutzerdefinierte Geschäftslogik auf Geräte. Alle Machine Learning-Modelle können auf IoT Edge-Geräten ausgeführt werden. Die Dokumentation zum Einrichten eines IoT Edge-Geräts und zum Erstellen einer Bereitstellung finden Sie unter [aka.ms/Azure-Iot-Edge-Doc](https://aka.ms/azure-iot-edge-doc).

Die folgenden Abschnitte enthalten weitere zu beachtende Aspekte.

### <a name="add-registry-credentials-to-the-edge-runtime-on-your-edge-device"></a>Hinzufügen von Registrierungsanmeldeinformationen zur Edge-Runtime auf Ihrem Edge-Gerät
Fügen Sie auf dem Computer, auf dem Sie IoT Edge ausführen, die Anmeldeinformationen Ihrer Registrierung hinzu, damit die Runtime Zugriff erhält, um den Container zu pullen.

Unter Windows führen Sie den folgenden Befehl aus:
```cmd/sh
iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password>
```
Unter Linux führen Sie den folgenden Befehl aus:
```cmd/sh
sudo iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password>
```

### <a name="find-the-machine-learning-container-image-location"></a>Navigieren zum Speicherort des Machine Learning-Containerimage
Sie benötigen den Speicherort Ihres Machine Learning-Containerimage. So finden Sie den Speicherort des Containerimage:

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com/) an.
2. Wählen Sie in **Azure Container Registry** die Registrierung aus, die Sie untersuchen möchten.
3. Klicken Sie unter „Registrierung“ auf **Repositorys**, um eine Liste aller Repositorys und Images anzuzeigen.













