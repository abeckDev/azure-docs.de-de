---
title: "Weiterleiten von Azure Blob Storage-Ereignissen an einen benutzerdefinierten Web-Endpunkt – PowerShell | Microsoft-Dokumentation"
description: Abonnieren Sie Blob Storage-Ereignisse mit Azure Event Grid.
services: storage,event-grid
keywords: 
author: david-stanford
ms.author: dastanfo
ms.date: 01/30/2018
ms.topic: article
ms.service: storage
ms.openlocfilehash: 329a79511b810159244b5530a49a5916440d2046
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="route-blob-storage-events-to-a-custom-web-endpoint-with-powershell"></a>Weiterleiten von Blob Storage-Ereignissen an einen benutzerdefinierten Web-Endpunkt mit PowerShell

Azure Event Grid ist ein Ereignisdienst für die Cloud. In diesem Artikel abonnieren Sie mithilfe von Azure PowerShell Blob Storage-Ereignisse, lösen ein Ereignis aus und zeigen das Ergebnis an. 

Ereignisse werden üblicherweise an einen Endpunkt gesendet, der auf das Ereignis reagiert (beispielsweise ein Webhook oder eine Azure-Funktion). Zur Vereinfachung des Beispiels in diesem Artikel werden Ereignisse an eine URL gesendet, die die Nachrichten lediglich sammelt. Sie erstellen diese URL mithilfe von Drittanbietertools entweder über [RequestBin](https://requestb.in/) oder [Hookbin](https://hookbin.com/).

> [!NOTE]
> **RequestBin** und **Hookbin** sind nicht für hohen Durchsatz konzipiert. Die Verwendung dieser Tools dient lediglich zur Veranschaulichung. Wenn Sie gleichzeitig mehrere Ereignisse pushen, werden möglicherweise nicht alle Ereignisse im Tool angezeigt.

Nach Abschluss der Schritte in diesem Artikel sehen Sie, dass die Ereignisdaten an einen Endpunkt gesendet wurden.

![Ereignisdaten](./media/storage-blob-event-quickstart/request-result.png)

## <a name="setup"></a>Einrichtung

Dieser Artikel setzt voraus, dass Sie die aktuelle Version von Azure PowerShell verwenden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-azurerm-ps) Informationen dazu.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl `Login-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Authentifizierungsanweisungen auf dem Bildschirm.

```powershell
Login-AzureRmAccount
```

> [!NOTE]
> Die Verfügbarkeit von Storage-Ereignissen ist an die [Verfügbarkeit](../../event-grid/overview.md) von Event Grid gebunden und wird in anderen Regionen verfügbar, wenn dort Event Grid verfügbar ist.

In diesem Beispiel wird **westus2** verwendet und die Auswahl zur Verwendung im gesamten Artikel in einer Variable gespeichert.

```powershell
$location = "westus2"
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Event Grid-Themen sind Azure-Ressourcen und müssen in einer Azure-Ressourcengruppe platziert werden. Die Azure-Ressourcengruppe ist eine logische Sammlung, in der Azure-Ressourcen bereitgestellt und verwaltet werden.

Erstellen Sie mit dem Befehl [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Ressourcengruppe.

Das folgende Beispiel erstellt eine Ressourcengruppe namens **gridResourceGroup** am Standort **westus2**.  

```powershell
$resourceGroup = "gridResourceGroup"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location
```

## <a name="create-a-storage-account"></a>Speicherkonto erstellen

Zur Verwendung von Blob Storage-Ereignissen benötigen Sie ein [Blob Storage-Konto](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-storage-accounts) oder ein [Speicherkonto des Typs „General Purpose v2“](../common/storage-account-options.md#general-purpose-v2). Bei **General Purpose v2 (GPv2)** handelt es sich um Speicherkonten, die alle Features für alle Speicherdienste unterstützen, z.B. Blobs, Files, Queues und Tables. **Blob Storage-Konten** sind spezielle Speicherkonten und dienen dazu, unstrukturierte Daten als Blobs (Objekte) in Azure Storage zu speichern. BLOB-Speicherkonten sind wie allgemeine Speicherkonten und besitzen die gleichen Haltbarkeits-, Verfügbarkeits-, Skalierbarkeits- und Leistungseigenschaften, die Sie schon heute verwenden – einschließlich vollständiger API-Konsistenz für Blockblobs und Anfügeblobs. Bei Anwendungen, die nur Block- oder Anfügeblobspeicher benötigen, empfiehlt sich die Verwendung von BLOB-Speicherkonten.  

Erstellen Sie mithilfe von [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount) ein Blob Storage-Konto mit LRS-Replikation, und rufen Sie anschließend den Kontext des Speicherkontos ab, der das zu verwendende Speicherkonto definiert. Wenn Sie Aktionen für ein Speicherkonto ausführen, verweisen Sie auf den Kontext, anstatt wiederholt die Anmeldeinformationen anzugeben. In diesem Beispiel wird ein Speicherkonto namens **gridstorage** mit lokal redundantem Speicher (LRS) und Blobverschlüsselung (standardmäßig aktiviert) erstellt. 

> [!NOTE]
> Speicherkontonamen sind in einem globalen Namespace angegeben, sodass Sie dem in diesem Skript angegebenen Namen zufällig gewählte Zeichen anfügen müssen.

```powershell
$storageName = "gridstorage"
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind BlobStorage `
  -AccessTier Hot

$ctx = $storageAccount.Context
```

## <a name="create-a-message-endpoint"></a>Erstellen eines Nachrichtenendpunkts

Vor dem Abonnieren des Themas erstellen wir zunächst den Endpunkt für die Ereignisnachricht. Wir schreiben allerdings keinen Code, um eine Reaktion auf das Ereignis auszulösen, sondern erstellen einen Endpunkt, der die Nachrichten sammelt, damit Sie sie anzeigen können. RequestBin und Hookbin sind Open-Source-Drittanbietertools, mit denen Sie einen Endpunkt erstellen und Anforderungen anzeigen können, die an ihn gesendet werden. Klicken Sie in [RequestBin](https://requestb.in/) auf **Create a RequestBin** (RequestBin erstellen), oder klicken Sie in [Hookbin](https://hookbin.com/) auf **Create New Endpoint** (Neuen Endpunkt erstellen). Kopieren Sie die Bin-URL, und ersetzen Sie `<bin URL>` im folgenden Skript.

```powershell
$binEndPoint = "<bin URL>"
```

## <a name="subscribe-to-your-storage-account"></a>Abonnieren Ihres Speicherkontos

Sie abonnieren ein Thema, um Event Grid mitzuteilen, welche Ereignisse Sie nachverfolgen möchten. Im folgenden Beispiel wird das von Ihnen erstellte Speicherkonto abonniert. Außerdem wird die RequestBin- oder Hookbin-URL als Endpunkt für Ereignisbenachrichtigungen übergeben. 

```powershell
$storageId = (Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup -AccountName $storageName).Id
New-AzureRmEventGridSubscription `
  -EventSubscriptionName gridBlobQuickStart `
  -Endpoint $binEndPoint `
  -ResourceId $storageId
```

## <a name="trigger-an-event-from-blob-storage"></a>Auslösen eines Ereignisses aus Blob Storage

Als Nächstes lösen wir ein Ereignis aus, um zu sehen, wie Event Grid die Nachricht an Ihren Endpunkt weiterleitet. Zunächst erstellen wir einen Container und ein Objekt. Anschließend wird das Objekt in den Container hochgeladen.

```powershell
$containerName = "gridcontainer"
New-AzureStorageContainer -Name $containerName -Context $ctx

echo $null >> gridTestFile.txt

Set-AzureStorageBlobContent -File gridTestFile.txt -Container $containerName -Context $ctx -Blob gridTestFile.txt
```

Sie haben das Ereignis ausgelöst, und Event Grid hat die Nachricht an den Endpunkt gesendet, den Sie beim Abonnieren konfiguriert haben. Browsen Sie zur zuvor erstellten Endpunkt-URL. Oder klicken Sie im geöffneten Browser auf die Option zum Aktualisieren. Das soeben gesendete Ereignis wird angezeigt. 

```json
[{
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myrg/providers/Microsoft.Storage/storageAccounts/myblobstorageaccount",
  "subject": "/blobServices/default/containers/gridcontainer/blobs/gridTestFile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-08-16T20:33:51.0595757Z",
  "id": "4d96b1d4-0001-00b3-58ce-16568c064fab",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "Azure-Storage-PowerShell-d65ca2e2-a168-4155-b7a4-2c925c18902f",
    "requestId": "4d96b1d4-0001-00b3-58ce-16568c000000",
    "eTag": "0x8D4E4E61AE038AD",
    "contentType": "application/octet-stream",
    "contentLength": 0,
    "blobType": "BlockBlob",
    "url": "https://myblobstorageaccount.blob.core.windows.net/gridcontainer/gridTestFile.txt",
    "sequencer": "00000000000000EB0000000000046199",
    "storageDiagnostics": {
      "batchId": "dffea416-b46e-4613-ac19-0371c0c5e352"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]

```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie das Speicherkonto und das Ereignisabonnement weiterverwenden möchten, überspringen Sie die Bereinigung der in diesem Artikel erstellten Ressourcen. Führen Sie andernfalls den folgenden Befehl aus, um die in diesem Artikel erstellten Ressourcen zu löschen.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben gelernt, wie Sie Themen und Ereignisabonnements erstellen. Nun können Sie sich ausführlicher über Blob Storage-Ereignisse und die Möglichkeiten informieren, die Event Grid Ihnen bietet:

- [Reacting to Blob storage events (preview)](storage-blob-event-overview.md) (Reagieren auf Blob Storage-Ereignisse (Vorschauversion))
- [An introduction to Azure Event Grid](../../event-grid/overview.md) (Einführung in Azure Event Grid)
