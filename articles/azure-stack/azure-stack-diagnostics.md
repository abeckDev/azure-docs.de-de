---
title: Diagnose in Azure Stack
description: "Es wird beschrieben, wie Sie Protokolldateien für die Diagnose in Azure Stack sammeln."
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 12/15/2017
ms.author: jeffgilb
ms.reviewer: adshar
ms.openlocfilehash: e823aeb4291b3e765b35181c24b41fa58c170cca
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2018
---
# <a name="azure-stack-diagnostics-tools"></a>Azure Stack-Diagnosetools

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*
 
Bei Azure Stack handelt es sich um eine große Sammlung von Komponenten, die zusammenarbeiten und miteinander interagieren. Alle diese Komponenten generieren eigene eindeutige Protokolle. Dies kann die Diagnose von Problemen zu einer Herausforderung machen – insbesondere bei Fehlern, die von verschiedenen interagierenden Azure Stack-Komponenten stammen. 

Mit unseren Diagnosetools wird sichergestellt, dass der Mechanismus für die Protokollsammlung einfach und effizient ist. Im folgenden Diagramm ist dargestellt, wie Tools zum Sammeln von Protokollen in Azure Stack funktionieren:

![Azure Stack-Diagnosetools](media/azure-stack-diagnostics/get-azslogs.png)
 
 
## <a name="trace-collector"></a>Collector der Ablaufverfolgung
 
Der Collector der Ablaufverfolgung ist standardmäßig aktiviert und wird dauerhaft im Hintergrund ausgeführt, um alle ETW-Protokolle (Ereignisablaufverfolgung für Windows) von Azure Stack-Komponentendiensten zu sammeln. ETW-Protokolle werden für fünf Tage in einer gemeinsamen lokalen Freigabe gespeichert. Nachdem dieser Grenzwert erreicht wurde, werden die ältesten Dateien gelöscht, wenn neue Dateien erstellt werden. Die maximal zulässige Standardgröße pro Datei beträgt 200 MB. Die Größe wird alle zwei Minuten überprüft. Ab einer Größe von 200 MB wird die aktuelle Datei gespeichert und eine neue Datei generiert. Für die Gesamtdateigröße, die pro Ereignissitzung generiert werden kann, gilt außerdem eine Obergrenze von 8 GB. 

## <a name="log-collection-tool"></a>Tool für die Protokollsammlung
 
Das PowerShell-Cmdlet **Get-AzureStackLog** kann verwendet werden, um Protokolle für alle Komponenten einer Azure Stack-Umgebung zu sammeln. Sie werden an einem vom Benutzer angegebenen Ort in ZIP-Dateien gespeichert. Wenn das Team für den technischen Support für Azure Stack Ihre Protokolle für die Behandlung eines Problems benötigt, werden Sie unter Umständen gebeten, dieses Tool auszuführen.

> [!CAUTION]
> Es kann sein, dass diese Protokolldateien personenbezogene Informationen enthalten. Berücksichtigen Sie dies, bevor Sie Protokolldateien öffentlich bereitstellen.
 
Es folgen einige Beispiele für die gesammelten Protokolltypen:
*   **Azure Stack-Bereitstellungsprotokolle**
*   **Windows-Ereignisprotokolle**
*   **Panther-Protokolle**
*   **Clusterprotokolle**
*   **Speicherdiagnoseprotokolle**
*   **ETW-Protokolle**

Diese Dateien werden vom Collector der Ablaufverfolgung gesammelt und in einer Freigabe gespeichert. Das PowerShell-Cmdlet **Get-AzureStackLog** kann dann verwendet werden, um sie bei Bedarf zu sammeln.
 
### <a name="to-run-get-azurestacklog-on-an-azure-stack-development-kit-asdk-system"></a>So führen Sie Get-AzureStackLog in einem System mit dem Azure Stack Development Kit (ASDK) aus
1. Melden Sie sich auf dem Host als **AzureStack\CloudAdmin** an.
2. Öffnen Sie ein PowerShell-Fenster als Administrator.
3. Führen Sie PowerShell-Cmdlet **Get-AzureStackLog**.

**Beispiele:**

  Sammeln Sie alle Protokolle für alle Rollen:

  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs
  ```

  Sammeln Sie Protokolle für die Rollen VirtualMachines und BareMetal:

  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal
  ```

  Sammeln Sie Protokolle für die Rollen VirtualMachines und BareMetal, und verwenden Sie die Datumsfilterung für Protokolldateien für die letzten acht Stunden:
    
  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8)
  ```

  Sammeln Sie Protokolle für die Rollen VirtualMachines und BareMetal, und verwenden Sie für die Datumsfilterung der Protokolldateien den Zeitraum zwischen acht und zwei Stunden vor dem aktuellen Zeitpunkt:

  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8) -ToDate (Get-Date).AddHours(-2)
  ```

### <a name="to-run-get-azurestacklog-on-an-azure-stack-integrated-system"></a>So führen Sie Get-AzureStackLog in einem integrierten Azure Stack-System aus

Um das Protokollerfassungstool in einem integrierten System auszuführen, benötigen Sie Zugriff auf den privilegierten Endpunkt (PEP). Das folgende Beispielskript können Sie mit dem PEP ausführen, um Protokolle in einem integrierten System zu sammeln:

```powershell
$ip = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.
 
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
 
$shareCred = Get-Credential
 
$s = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred

$fromDate = (Get-Date).AddHours(-8)
$toDate = (Get-Date).AddHours(-2)  #provide the time that includes the period for your issue
 
Invoke-Command -Session $s {    Get-AzureStackLog -OutputPath "\\<HLH MACHINE ADDRESS>\c$\logs" -OutputSharePath "<EXTERNAL SHARE ADDRESS>" -OutputShareCredential $using:shareCred  -FilterByRole Storage -FromDate $using:fromDate -ToDate $using:toDate}

if($s)
{
    Remove-PSSession $s
}
```

- Geben Sie beim Sammeln von Protokollen vom PEP für den **OutputPath**-Parameter einen Speicherort auf dem HLH-Computer (Hardware Lifecycle Host) an. Stellen Sie außerdem sicher, dass der Speicherort verschlüsselt ist.
- Die Parameter **OutputSharePath** und **OutputShareCredential** sind optional und werden verwendet, wenn Sie Protokolle in einen externen freigegebenen Ordner hochladen. Verwenden Sie diese Parameter *zusätzlich* zu **OutputPath**. Wenn **OutputPath** nicht angegeben ist, verwendet das Protokollsammlungstool das Systemlaufwerk der PEP-VM für die Speicherung. Dies kann zu einem Fehler beim Skript führen, da der Speicherplatz auf dem Laufwerk begrenzt ist.
- Wie im vorherigen Beispiel gezeigt, können die Parameter **FromDate** und **ToDate** zum Sammeln von Protokollen für einen bestimmten Zeitraum verwendet werden. Dies kann für Szenarien wie das Sammeln von Protokollen nach dem Anwenden von Updatepaketen in einem integrierten System hilfreich sein.

### <a name="parameter-considerations-for-both-asdk-and-integrated-systems"></a>Überlegungen zu den Parametern für das ASDK und integrierte Systeme

- Wenn die Parameter **FromDate** und **ToDate** nicht angegeben sind, werden standardmäßig Protokolle für die letzten vier Stunden gesammelt.
- Sie können den **TimeOutInMinutes**-Parameter verwenden, um das Timeout für die Sammlung von Protokollen festzulegen. Es ist standardmäßig auf 150 (2,5 Stunden) festgelegt.

- Derzeit können Sie den **FilterByRole**-Parameter verwenden, um die Protokollsammlung nach den folgenden Rollen zu filtern:

   |   |   |   |
   | - | - | - |
   | ACSMigrationService     | ACSMonitoringService   | ACSSettingsService |
   | ACS                     | ACSFabric              | ACSFrontEnd        |
   | ACSTableMaster          | ACSTableServer         | ACSWac             |
   | ADFS                    | ASAppGateway           | BareMetal          |
   | BRP                     | CA                     | CPI                |
   | CRP                     | DeploymentMachine      | DHCP               |
   | Domäne                  | ECE                    | ECESeedRing        | 
   | FabricRing              | FabricRingServices     | FRP                |
   | Gateway                 | HealthMonitoring       | HRP                |   
   | IBC                     | InfraServiceController | KeyVaultAdminResourceProvider|
   | KeyVaultControlPlane    | KeyVaultDataPlane      | NC                 |   
   | NonPrivilegedAppGateway | NRP                    | SeedRing           |
   | SeedRingServices        | SLB                    | SQL                |   
   | SRP                     | Speicher                | StorageController  |
   | URP                     | UsageBridge            | VirtualMachines    |  
   | WAS                     | WASPUBLIC              | WDS                |


### <a name="bkmk_gui"></a>Erfassen von Protokollen mithilfe einer grafischen Benutzeroberfläche
Anstatt die erforderlichen Parameter für das Cmdlet Get-AzureStackLog zum Abrufen von Azure Stack-Protokollen bereitzustellen, können Sie auch die verfügbaren Open-Source-Tools für Azure Stack verwenden, die Sie im GitHub-Toolrepository unter http://aka.ms/AzureStackTools finden.

Das PowerShell-Skript **ERCS_AzureStackLogs.ps1** ist im GitHub-Toolrepository enthalten und wird regelmäßig aktualisiert. Laden Sie es am besten direkt von http://aka.ms/ERCS herunter, um sicherzugehen, dass Sie über die neueste Version verfügen. Beim Start aus einer PowerShell-Administratorsitzung stellt das Skript eine Verbindung mit dem privilegierten Endpunkt her und führt Get-AzureStackLog mit den angegebenen Parametern aus. Wenn keine Parameter angegeben werden, fordert das Skript standardmäßig über eine grafische Benutzeroberfläche zur Eingabe von Parametern auf.

Weitere Informationen zum PowerShell-Skript „ERCS_AzureStackLogs.ps1“ erhalten Sie in [einem kurzen Video](https://www.youtube.com/watch?v=Utt7pLsXEBc) und in der [Infodatei](https://github.com/Azure/AzureStack-Tools/blob/master/Support/ERCS_Logs/ReadMe.md) des Skripts, das sich im GitHub-Toolrepository für Azure Stack befindet. 

### <a name="additional-considerations"></a>Zusätzliche Überlegungen

* Je nachdem, welche Rollen in den Protokollen gesammelt werden, kann dieser Befehl mehr Zeit beanspruchen. Zu den entscheidenden Faktoren gehören auch der Zeitraum, der für die Protokollsammlung angegeben wurde, und die Anzahl von Knoten in der Azure Stack-Umgebung.
* Überprüfen Sie nach Abschluss der Protokollsammlung den neuen Ordner, der mit dem **OutputPath**-Parameter des Befehls erstellt wird.
* Für jede Rolle sind die Protokolle in separaten ZIP-Dateien enthalten. Abhängig von der Größe der gesammelten Protokolle kann das Protokoll einer Rolle möglicherweise in mehrere ZIP-Dateien aufgeteilt werden. Wenn Sie alle Protokolldateien in denselben Ordner entzippen möchten, verwenden Sie für eine solche Rolle ein Tool, das den Entzipvorgang in einer Sammeloperation durchführen kann (z.B. 7-Zip). Wählen Sie alle ZIP-Dateien für die Rolle und dann **Hier entpacken** aus. Damit werden die Protokolldateien für diese Rolle in einem zusammengeführten Ordner entzippt.
* Eine Datei namens **Get-AzureStackLog_Output.log** wird ebenfalls in dem Ordner mit den gezippten Protokolldateien erstellt. Diese Datei ist ein Protokoll der Befehlsausgabe, die Sie zum Beheben von Problemen während der Protokollsammlung verwenden können.
* Zum Untersuchen eines bestimmten Fehlers werden unter Umständen die Protokolle von mehr als einer Komponente benötigt.
    -   System- und Ereignisprotokolle für alle Infrastruktur-VMs werden unter der Rolle *VirtualMachines* gesammelt.
    -   System- und Ereignisprotokolle für alle Hosts werden unter der Rolle *BareMetal* gesammelt.
    -   Failovercluster- und Hyper-V-Ereignisprotokolle werden unter der Rolle *Storage* gesammelt.
    -   ACS-Protokolle werden unter den Rollen *Storage* und *ACS* gesammelt.

> [!NOTE]
> Größen- und Altersgrenzwerte für die gesammelten Protokolle werden erzwungen, da es sehr wichtig ist, für eine effiziente Nutzung Ihres Speicherplatzes zu sorgen. So wird sichergestellt, dass keine übermäßig hohe Anzahl von Protokollen gespeichert wird. Allerdings werden beim Diagnostizieren von Problemen manchmal Protokolle benötigt, die aufgrund dieser Grenzwerte möglicherweise nicht mehr vorhanden sind. Daher wird **dringend empfohlen**, Ihre Protokolle alle acht bis zwölf Stunden in einen externen Speicher zu verlagern (Speicherkonto in Azure, zusätzliches lokales Speichergerät usw.) und je nach Ihren Anforderungen dort ein bis drei Monate lang aufzubewahren. Stellen Sie außerdem sicher, dass der Speicherort verschlüsselt ist.

## <a name="next-steps"></a>Nächste Schritte
[Microsoft Azure Stack troubleshooting (Problembehandlung für Microsoft Azure Stack)](azure-stack-troubleshooting.md)

