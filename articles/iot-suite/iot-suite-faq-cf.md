---
title: "Lösung für verbundene Factorys – Häufig gestellte Fragen – Azure | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen zur IoT Suite Connected Factory"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: ab72152fc937e3c4552147fce29c95ea0efcadf4
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="frequently-asked-questions-for-iot-suite-connected-factory-preconfigured-solution"></a>Häufig gestellte Fragen zur vorkonfigurierten IoT Suite Connected Factory-Lösung

Informationen finden Sie auch in den allgemeinen [häufig gestellten Fragen](iot-suite-faq.md) zur IoT Suite.

### <a name="where-can-i-find-the-source-code-for-the-preconfigured-solution"></a>Wo finde ich den Quellcode für die vorkonfigurierte Lösung?

Der Quellcode ist im folgenden GitHub-Repository gespeichert:

* [Connected factory preconfigured solution](https://github.com/Azure/azure-iot-connected-factory) (Vorkonfigurierte Lösung für eine verbundene Factory)

### <a name="what-is-opc-ua"></a>Was ist OPC UA?

Die OPC Unified Architecture (UA), die 2008 veröffentlicht wurde, ist ein plattformunabhängiger, dienstorientierter Interoperabilitätsstandard. Die OPC UA wird von verschiedenen Industriesystemen und -geräten wie z.B. Industrie-PCs, -PLCs und -sensoren verwendet. Die OPC UA integriert alle Funktionen der klassischen OPC-Spezifikationen in ein erweiterbares Framework mit integrierter Sicherheit. Der Standard wird von der OPC Foundation verwaltet. Die [OPC Foundation](http://opcfoundation.org/) ist eine gemeinnützige Organisation mit über 440 Mitgliedern. Das Ziel der Organisation ist die Verwendung der OPC-Spezifikationen, um eine sichere und zuverlässige Interoperabilität über mehrere Anbieter und Plattformen hinweg zu ermöglichen. Folgende Elemente werden dabei berücksichtigt:

* Infrastruktur
* Spezifikationen
* Technologie
* Prozesse

### <a name="why-did-microsoft-choose-opc-ua-for-the-connected-factory-preconfigured-solution"></a>Warum hat sich Microsoft für OPC UA als vorkonfigurierte Lösung für eine verbundene Factory entschieden?

Microsoft hat OPC UA ausgewählt, da es sich um einen offenen, nicht proprietären, plattformunabhängigen, von der Industrie anerkannten und bewährten Standard handelt. Er stellt eine Voraussetzung für Referenzarchitekturlösungen für die Industrie 4.0 (RAMI4.0) dar und gewährleistet die Interoperabilität zwischen einer Vielzahl von Herstellungsverfahren und -geräten. Microsoft sieht bei seinen Kunden einen Bedarf für die Erstellung von Industrie 4.0-Lösungen. Die Unterstützung für OPC UA verringert die Barrieren für Kunden, ihre Ziele zu erreichen, und bietet sofort Geschäftsvorteile.

### <a name="how-do-i-add-a-public-ip-address-to-the-simulation-vm"></a>Wie füge ich der Simulations-VM eine öffentliche IP-Adresse hinzu?

Zum Hinzufügen der IP-Adresse stehen Ihnen zwei Optionen zur Verfügung:

* Verwenden Sie das PowerShell-Skript `Simulation/Factory/Add-SimulationPublicIp.ps1` im [Repository](https://github.com/Azure/azure-iot-connected-factory). Übergeben Sie den Namen Ihrer Bereitstellung als Parameter. Für eine lokale Bereitstellung verwenden Sie `<your username>ConnFactoryLocal`. Das Skript gibt die IP-Adresse des virtuellen Computers aus.

* Suchen Sie im Azure-Portal die Ressourcengruppe Ihrer Bereitstellung. Außer bei einer lokalen Bereitstellung weist die Ressourcengruppe den Namen auf, den Sie als Lösungs- oder Bereitstellungsnamen angegeben haben. Für eine lokale Bereitstellung mithilfe des Buildskripts lautet der Name der Ressourcengruppe `<your username>ConnFactoryLocal`. Fügen Sie jetzt der Ressourcengruppe eine neue Ressource für die **öffentliche IP-Adresse** hinzu.

> [!NOTE]
> Stellen Sie in allen Fällen sicher, dass Sie die neuesten Patches installieren, indem Sie die Anweisungen auf der [Ubuntu-Website](https://wiki.ubuntu.com/Security/Upgrades) befolgen. Sorgen Sie dafür, dass die Installation aktuell ist, solange Ihr virtueller Computer über eine öffentliche IP-Adresse erreichbar ist.

### <a name="how-do-i-remove-the-public-ip-address-to-the-simulation-vm"></a>Wie entferne ich eine öffentliche IP-Adresse aus der Simulations-VM?

Zum Entfernen der IP-Adresse stehen Ihnen zwei Optionen zur Verfügung:

* Verwenden Sie das PowerShell-Skript Simulation/Factory/Remove-SimulationPublicIp.ps1 im [Repository](https://github.com/Azure/azure-iot-connected-factory). Übergeben Sie den Namen Ihrer Bereitstellung als Parameter. Für eine lokale Bereitstellung verwenden Sie `<your username>ConnFactoryLocal`. Das Skript gibt die IP-Adresse des virtuellen Computers aus.

* Suchen Sie im Azure-Portal die Ressourcengruppe Ihrer Bereitstellung. Außer bei einer lokalen Bereitstellung weist die Ressourcengruppe den Namen auf, den Sie als Lösungs- oder Bereitstellungsnamen angegeben haben. Für eine lokale Bereitstellung mithilfe des Buildskripts lautet der Name der Ressourcengruppe `<your username>ConnFactoryLocal`. Entfernen Sie jetzt die Ressource für die **öffentliche IP-Adresse** aus der Ressourcengruppe.

### <a name="how-do-i-sign-in-to-the-simulation-vm"></a>Wie melde ich mich bei der Simulations-VM an?

Die Anmeldung bei der Simulations-VM wird nur unterstützt, wenn Sie Ihre Lösung mithilfe des PowerShell-Skripts `build.ps1` im [Repository](https://github.com/Azure/azure-iot-connected-factory) bereitgestellt haben.

Wenn Sie die Lösung über www.azureiotsuite.com bereitgestellt haben, können Sie sich nicht bei der VM anmelden. Dies liegt daran, dass das Kennwort zufällig generiert wird und Sie es nicht zurücksetzen können.

1. Fügen Sie der VM eine öffentliche IP-Adresse hinzu. Informationen dazu finden Sie unter [Wie füge ich der Simulations-VM eine öffentliche IP-Adresse hinzu?](#how-do-i-remove-the-public-ip-address-to-the-simulation-vm)
1. Erstellen Sie unter Angabe der IP-Adresse der VM eine SSH-Sitzung mit Ihrer VM.
1. Der zu verwendende Benutzername lautet `docker`.
1. Das Kennwort richtet sich nach der Version, die Sie für die Bereitstellung verwendet haben:
    * Bei Lösungen, die vor dem 1. Juni 2017 mithilfe des Skripts „build.ps1“ erstellt wurden, lautet das Kennwort `Passw0rd`.
    * Bei Lösungen, die nach dem 1. Juni 2017 mithilfe des Skripts „build.ps1“ erstellt wurden, finden Sie das Kennwort in der Datei `<name of your deployment>.config.user`. Das Kennwort ist in der Einstellung **VmAdminPassword** gespeichert. Das Kennwort wird zur Bereitstellungszeit per Zufallsprinzip generiert, sofern Sie es nicht im `build.ps1`-Skriptparameter `-VmAdminPassword` angegeben haben.

### <a name="how-do-i-stop-and-start-all-docker-processes-in-the-simulation-vm"></a>Wie stoppe und starte ich alle Dockerprozesse in der Simulations-VM?

1. Melden Sie sich bei der Simulations-VM an. Informationen dazu finden Sie unter [Wie melde ich mich bei der Simulations-VM an?](#how-do-i-sign-in-to-the-simulation-vm)
1. Um zu prüfen, welche Container aktiv sind, führen Sie `docker ps` aus.
1. Um alle Simulationscontainer anzuhalten, führen Sie `./stopsimulation` aus.
1. Um alle Simulationscontainer zu starten, gehen Sie folgendermaßen vor:
    * Exporten Sie eine Shellvariable mit dem Namen **IOTHUB_CONNECTIONSTRING**. Verwenden Sie den Wert der **IotHubOwnerConnectionString**-Einstellung in der Datei `<name of your deployment>.config.user`. Beispiel: 

        ```
        export IOTHUB_CONNECTIONSTRING="HostName={yourdeployment}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={your key}"
        ```

    * Führen Sie `./startsimulation`aus.

### <a name="how-do-i-update-the-simulation-in-the-vm"></a>Wie aktualisiere ich die Simulation in der VM?

Wenn Sie Änderungen an der Simulation vorgenommen haben, können Sie das PowerShell-Skript `build.ps1` im [Repository](https://github.com/Azure/azure-iot-connected-factory) mit dem Befehl `updatedimulation` verwenden. Dieses Skript erstellt alle Simulationskomponenten, hält die Simulation in der VM an, lädt die Komponenten hoch, installiert sie und startet sie.

### <a name="how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution"></a>Wie finde ich die Verbindungszeichenfolge des IoT-Hubs, den meine Lösung verwendet?

Wenn Sie Ihre Lösung mit dem `build.ps1`-Skript im [Repository](https://github.com/Azure/azure-iot-connected-factory) bereitgestellt haben, ist die Verbindungszeichenfolge der Wert von **IotHubOwnerConnectionString** in der Datei `<name of your deployment>.config.user`.

Sie können die Verbindungszeichenfolge auch im Azure-Portal finden. Ermitteln Sie in der IoT Hub-Ressource in der Ressourcengruppe Ihrer Bereitstellung die Einstellung für die Verbindungszeichenfolge.

### <a name="which-iot-hub-devices-does-the-connected-factory-simulation-use"></a>Welche IoT Hub-Geräte werden von der Simulation der verbundenen Factory verwendet?

Die Simulation registriert die folgenden Geräte selbst:

* proxy.beijing.corp.contoso
* proxy.capetown.corp.contoso
* proxy.mumbai.corp.contoso
* proxy.munich0.corp.contoso
* proxy.rio.corp.contoso
* proxy.seattle.corp.contoso
* publisher.beijing.corp.contoso
* publisher.capetown.corp.contoso
* publisher.mumbai.corp.contoso
* publisher.munich0.corp.contoso
* publisher.rio.corp.contoso
* publisher.seattle.corp.contoso

Mithilfe von [Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) und der [IoT-Erweiterung für Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension) können Sie überprüfen, welche Geräte bei dem von Ihrer Lösung verwendeten IoT Hub registriert sind. Um Device Explorer zu verwenden, benötigen Sie die Verbindungszeichenfolge für den IoT Hub in Ihrer Bereitstellung. Um die IoT-Erweiterung für Azure CLI 2.0 zu verwenden, benötigen Sie Ihren IoT Hub-Namen.

### <a name="how-can-i-get-log-data-from-the-simulation-components"></a>Wie kann ich Protokolldaten aus dem Simulationskomponenten abrufen?

Alle Komponenten in der Simulation schreiben Daten in Protokolldateien. Diese Dateien befinden sich auf dem virtuellen Computer im Ordner `home/docker/Logs`. Um die Protokolle abzurufen, können Sie das PowerShell-Skript `Simulation/Factory/Get-SimulationLogs.ps1` im [Repository](https://github.com/Azure/azure-iot-connected-factory) verwenden.

Dieses Skript muss sich bei der VM anmelden. Möglicherweise müssen Sie Anmeldeinformationen für die Anmeldung angeben. Informationen zum Ermitteln der Anmeldeinformationen finden Sie unter [Wie melde ich mich bei der Simulations-VM an?](#how-do-i-sign-in-to-the-simulation-vm)

Das Skript fügt dem virtuellen Computer eine öffentliche IP-Adresse hinzu, wenn dieser noch keine besitzt, und entfernt diese wieder. Das Skript speichert alle Protokolldateien in ein Archiv und lädt das Archiv auf Ihre Entwicklungsarbeitsstation herunter.

Alternativ dazu können Sie sich über SSH beim virtuellen Computer anmelden und die Protokolldateien zur Laufzeit untersuchen.

### <a name="how-can-i-check-if-the-simulation-is-sending-data-to-the-cloud"></a>Wie kann ich überprüfen, ob die Simulation Daten an die Cloud sendet?

Mit dem Tools [Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) oder [IoT Hub Explorer](https://github.com/azure/iothub-explorer) können Sie die Daten überprüfen, die von bestimmten Geräten an IoT Hub gesendet werden. Um diese Tools zu verwenden, müssen Sie die Verbindungszeichenfolge für den IoT Hub in Ihrer Bereitstellung kennen. Informationen finden Sie unter [Wie finde ich die Verbindungszeichenfolge des IoT-Hubs, den meine Lösung verwendet?](#how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution)

Untersuchen Sie die Daten, die von einem der Publisher-Geräte gesendet werden:

* publisher.beijing.corp.contoso
* publisher.capetown.corp.contoso
* publisher.mumbai.corp.contoso
* publisher.munich0.corp.contoso
* publisher.rio.corp.contoso
* publisher.seattle.corp.contoso

Wenn Sie feststellen, dass keine Daten an IoT Hub gesendet werden, liegt ein Problem mit der Simulation vor. Als erstes sollten Sie die Protokolldateien der Simulationskomponenten analysieren. Informationen dazu finden Sie unter [Wie kann ich Protokolldaten aus dem Simulationskomponenten abrufen?](#how-can-i-get-log-data-from-the-simulation-components) Versuchen Sie danach, die Simulation anzuhalten und neu zu starten. Wenn weiterhin keine Daten gesendet werden, aktualisieren Sie die Simulation insgesamt. Informationen dazu finden Sie unter [Wie aktualisiere ich die Simulation in der VM?](#how-do-i-update-the-simulation-in-the-vm)

### <a name="how-do-i-enable-an-interactive-map-in-my-connected-factory-solution"></a>Wie aktiviere ich in meiner Connected Factory-Lösung eine interaktive Karte?

Um in Ihrer Connected Factory-Lösung eine interaktive Karte aktivieren zu können, müssen Sie über einen Tarif vom Typ „Bing Karten-API für Unternehmen“ verfügen.

Wenn Sie von [www.azureiotsuite.com](http://www.azureiotsuite.com) bereitstellen, wird während der Bereitstellung überprüft, ob Ihr Abonnement über einen aktivierten Plan für die Bing Karten-API für Unternehmen verfügt. Eine interaktive Karte zur Connected Factory wird automatisch bereitgestellt. Ist dies nicht der Fall, können Sie folgendermaßen trotzdem eine interaktive Karte in Ihrer Bereitstellung aktivieren:

Wenn Sie die Bereitstellung mit dem Skript `build.ps1` im GitHub-Repository für die Connected Factory durchführen und über einen Plan für die Bing Karten-API für Unternehmen verfügen, legen Sie die Umgebungsvariable `$env:MapApiQueryKey` im Buildfenster auf den Abfrageschlüssel Ihres Plans fest. Die interaktive Karte wird dann automatisch aktiviert.

Wenn Sie nicht über einen Plan für die Bing Karten-API für Unternehmen verfügen, stellen Sie die Connected Factory-Lösung über [www.azureiotsuite.com](http://www.azureiotsuite.com) oder mit dem Skript `build.ps1` bereit. Fügen Sie dann den Plan für die Bing Karten-API für Unternehmen Ihrem Abonnement wie unter [Wie erstelle ich ein Konto für die Bing Karten-API für Unternehmen?](#how-do-i-create-a-bing-maps-api-for-enterprise-account) beschrieben hinzu. Suchen Sie den Abfrageschlüssel für dieses Konto wie unter [Wie erhalte ich meinen Abfrageschlüssel für die Bing Karten-API für Unternehmen?](#how-to-obtain-your-bing-maps-api-for-enterprise-querykey) beschrieben hinzu, und speichern Sie diesen Schlüssel. Browsen Sie zum Azure-Portal, und greifen Sie auf die App Service-Ressource in Ihrer Connected Factory-Bereitstellung zu. Navigieren Sie zu **Anwendungseinstellungen**. Hier finden Sie den Abschnitt **App-Einstellungen**. Legen Sie **MapApiQueryKey** auf den Abfrageschlüssel fest, den Sie abgerufen haben. Speichern Sie die Einstellungen, navigieren Sie zu **Übersicht**, und starten Sie App Service neu.

### <a name="how-do-i-create-a-bing-maps-api-for-enterprise-account"></a>Wie erstelle ich ein Konto für die Bing Karten-API für Unternehmen?

Sie können kostenlos einen *Tarif vom Typ „Bing Karten für Unternehmen“ für die erste interne Transaktionsebene* erhalten. Einem Azure-Abonnement können allerdings maximal zwei dieser Tarife hinzugefügt werden. Falls Sie über kein Konto vom Typ „Bing Karten-API für Unternehmen“ verfügen, erstellen Sie eines, indem Sie im Azure-Portal auf **+ Ressource erstellen** klicken. Suchen Sie dann nach **Bing Karten-API für Unternehmen**, und befolgen Sie die Erstellungsanweisungen.

![Bing-Schlüssel](media/iot-suite-faq-cf/bing.png)

### <a name="how-to-obtain-your-bing-maps-api-for-enterprise-querykey"></a>Wie erhalte ich meinen Abfrageschlüssel für die Bing Karten-API für Unternehmen?

Fügen Sie im Azure-Portal nach dem Erstellen Ihres Tarifs vom Typ „Bing Karten-API für Unternehmen“ der Ressourcengruppe Ihrer Connected Factory-Lösung eine Ressource vom Typ „Bing Karten für Unternehmen“ hinzu.

1. Navigieren Sie im Azure-Portal zu der Ressourcengruppe mit dem Tarif vom Typ „Bing Karten-API für Unternehmen“.

1. Klicken Sie auf **Alle Einstellungen** und dann auf **Schlüsselverwaltung**.

1. Dort befinden sich zwei Schlüssel: **MasterKey** und **QueryKey**. Kopieren Sie den Wert von **QueryKey**.

1. Legen Sie die Umgebungsvariable `$env:MapApiQueryKey` in Ihrer PowerShell-Umgebung auf den Abfrageschlüssel (**QueryKey**) Ihres Tarifs fest, um den Schlüssel für das Skript `build.ps1` verfügbar zu machen. Das Buildskript fügt den Wert dann automatisch den App Service-Einstellungen hinzu.

1. Führen Sie unter Verwendung des Skripts `build.ps1` eine lokale oder cloudbasierte Bereitstellung aus.

### <a name="how-do-enable-the-interactive-map-while-debugging-locally"></a>Wie aktiviere ich die interaktive Karte während des lokalen Debuggens?

Legen Sie den Wert der Einstellung `MapApiQueryKey` in den Dateien `local.user.config` und `<yourdeploymentname>.user.config` im Stammverzeichnis Ihrer Bereitstellung auf den Wert des zuvor kopierten Abfrageschlüssels (**QueryKey**) fest, um die interaktive Karte während des lokalen Debuggens zu aktivieren.

### <a name="how-do-i-use-a-different-image-at-the-home-page-of-my-dashboard"></a>Wie kann ich das Bild auf der Startseite meines Dashboards ändern?

Ersetzen Sie das Bild `WebApp\Content\img\world.jpg`, um das statische Bild auf der Startseite des Dashboards zu ändern. Erstellen Sie die Web-App anschließend neu, und stellen Sie sie erneut bereit.

### <a name="how-do-i-use-non-opc-ua-devices-with-connected-factory"></a>Wie kann ich OPC UA-fremde Geräte mit der Connected Factory-Lösung verwenden?

Gehen Sie wie folgt vor, um Telemetriedaten von OPC UA-fremden Geräten an eine Connected Factory-Lösung zu senden:

1. [Konfigurieren Sie eine neue Station in der Connected Factory-Topologie](iot-suite-connected-factory-configure.md) (in der Datei `ContosoTopologyDescription.json`).

1. Erfassen Sie die Telemetriedaten in einem für Connected Factory geeigneten JSON-Format:

    ```json
    [
      {
        "ApplicationUri": "<the_value_of_OpcUri_of_your_station",
        "DisplayName": "<name_of_the_datapoint>",
        "NodeId": "value_of_NodeId_of_your_datapoint_in_the_station",
        "Value": {
          "Value": <datapoint_value>,
          "SourceTimestamp": "<timestamp>"
        }
      }
    ]
    ```

1. Das Format von `<timestamp>` lautet: `2017-12-08T19:24:51.886753Z`

1. Starten Sie den Connected Factory-App Service neu.

### <a name="next-steps"></a>Nächste Schritte

Sie können auch einige andere Features und Funktionen der vorkonfigurierten IoT Suite-Lösungen ausprobieren:

* [Übersicht über die vorkonfigurierte Predictive Maintenance-Lösung](iot-suite-predictive-overview.md)
* [Übersicht über die vorkonfigurierte Lösung für eine verbundene Factory](iot-suite-connected-factory-overview.md)
* [Sicherheit im Internet der Dinge von Anfang an](securing-iot-ground-up.md)