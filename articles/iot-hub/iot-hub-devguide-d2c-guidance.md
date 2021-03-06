---
title: "Azure IoT Hub – D2C-Optionen (Gerät-zu-Cloud) | Microsoft Docs"
description: "Entwicklerhandbuch – Leitfaden, der angibt, wann D2C-Nachrichten, gemeldete Eigenschaften oder Dateiupload für die C2D-Kommunikation verwendet werden sollen."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 979136db-c92d-4288-870c-f305e8777bdd
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: elioda
ms.openlocfilehash: a9a062ebb8d6e3b37d917064209eda618d0dd308
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="device-to-cloud-communications-guidance"></a>Leitfaden zur D2C-Kommunikation
Beim Senden von Informationen von der Geräte-App an das Lösungs-Back-End stehen in IoT Hub drei Optionen zur Verfügung:

* [Gerät-zu-Cloud-Nachrichten][lnk-d2c] für Time Series-Telemetrie und Warnungen
* [Gemeldete Eigenschaften des Gerätezwillings][lnk-twins] für die Meldung von Gerätestatusinformationen, z.B. verfügbare Funktionen, Bedingungen und Status von Workflows mit langer Laufzeit. z.B. Konfigurations- und Softwareupdates.
* [Dateiuploads][lnk-fileupload] für Mediendateien und große Telemetriebatches, die von zeitweise verbundenen Geräten hochgeladen oder komprimiert werden, um Bandbreite zu sparen.

Hier finden Sie einen detaillierten Vergleich verschiedener Optionen für die D2C-Kommunikation.

|  | D2C-Nachrichten | Gemeldete Eigenschaften des Gerätezwillings | Dateiuploads |
| ---- | ------- | ---------- | ---- |
| Szenario | Telemetrie-Zeitreihen und -Warnungen, Beispiel: Sendung von 256-KB-Sensordatenbatches alle 5 Minuten | Verfügbare Funktionen und Bedingungen, z.B. der aktuelle Gerätekonnektivitätsmodus wie Mobilfunk oder WLAN. Synchronisierung von Workflows mit langer Laufzeit, z.B. Konfiguration und Softwareupdates. | Mediendateien. Große (normalerweise komprimierte) Telemetriebatches. |
| Speichern und Abrufen | Temporäre Speicherung durch IoT Hub, bis zu 7 Tage. Nur sequenzielles Lesen. | Von IoT Hub im Gerätezwilling gespeichert. Abrufbar mithilfe der [IoT Hub-Abfragesprache][lnk-query]. | Speicherung im vom Benutzer bereitgestellten Azure Storage-Konto. |
| Größe | Nachrichten bis zu 256 KB | Die Maximalgröße gemeldeter Eigenschaften beträgt 8 KB. | Maximale von Azure Blob Storage unterstützte Dateigröße. |
| Frequency | Hoch. Weitere Informationen finden Sie unter [Referenz: IoT Hub-Kontingente und -Drosselung][lnk-quotas]. | Mittel. Weitere Informationen finden Sie unter [Referenz: IoT Hub-Kontingente und -Drosselung][lnk-quotas]. | Niedrig. Weitere Informationen finden Sie unter [Referenz: IoT Hub-Kontingente und -Drosselung][lnk-quotas]. |
| Protokoll | Mit allen Protokollen verfügbar. | Mit MQTT oder AMQP verfügbar. | Mit jedem Protokoll verfügbar, auf dem Gerät ist jedoch HTTPS erforderlich. |

Eine Anwendung muss möglicherweise Informationen sowohl als Telemetriezeitreihen als auch als Warnung senden und diese außerdem im Gerätezwilling zur Verfügung stellen. In diesem Szenario können Sie eine der folgenden Optionen auswählen:

* Die Geräte-App sendet eine D2C-Nachricht und meldet eine Eigenschaftsänderung.
* Das Lösungs-Back-End speichert die Informationen beim Empfang der Nachricht in den Tags des Gerätezwillings.

Da D2C-Nachrichten einen viel höheren Durchsatz zulassen als Gerätezwillingsupdates, ist es in einigen Fällen ratsam, das Gerätezwillingsupdate nicht für jede D2C-Nachricht durchzuführen.


[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-fileupload]: iot-hub-devguide-file-upload.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
