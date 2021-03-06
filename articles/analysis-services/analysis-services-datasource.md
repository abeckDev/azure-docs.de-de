---
title: "In Azure Analysis Services unterstützte Datenquellen | Microsoft-Dokumentation"
description: "Beschreibt Datenquellen, die für Datenmodelle in Azure Analysis Services unterstützt werden."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 6ec63319-ff9b-4b01-a1cd-274481dc8995
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/27/2018
ms.author: owend
ms.openlocfilehash: e2f7e356b260c0e5af67d28811bd88a63a601312
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>In Azure Analysis Services unterstützte Datenquellen

Im Assistenten zum Abrufen oder Importieren von Daten in Visual Studio werden Datenquellen und Connectors für Azure Analysis Services und SQL Server Analysis Services angezeigt. Es werden jedoch nicht alle angezeigten Datenquellen und Connectors in Azure Analysis Services unterstützt. Mit welchen Typen von Datenquellen Sie eine Verbindung herstellen können, ist von vielen Faktoren abhängig, z.B. Modellkompatibilitätsgrad, verfügbare Datenconnectors, Authentifizierungstyp, Anbieter und Unterstützung lokaler Datengateways. 

## <a name="azure-data-sources"></a>Azure-Datenquellen

|Datenquelle  |In-Memory  |DirectQuery  |
|---------|---------|---------|
|Azure SQL-Datenbank     |   Ja      |    Ja      |
|Azure SQL Data Warehouse     |   Ja      |   Ja       |
|Azure Blob Storage*     |   Ja       |    Nein       |
|Azure Table Storage*    |   Ja       |    Nein       |
|Azure Cosmos DB (Beta)*     |  Ja        |  Nein         |
|Azure Data Lake Store*     |   Ja       |    Nein       |
|Azure HDInsight HDFS*     |     Ja     |   Nein        |
|Azure HDInsight Spark (Beta)*     |   Ja       |   Nein        |
|Azure Database for MySQL (Vorschau)*     |   Ja       |   Nein       |
|Azure Database for PostgreSQL (Vorschau)*     | Ja         |  Nein        |
||||

\* nur tabellarische Modelle mit Kompatibilitätsgrad 1400.

**Anbieter**   
In-Memory- und DirectQuery-Modelle, die eine Verbindung mit Azure-Datenquellen herstellen, verwenden den .NET Framework-Datenanbieter für SQL Server.

## <a name="on-premises-data-sources"></a>Lokale Datenquellen

Zum Herstellen einer Verbindung mit lokalen Datenquellen ist ein lokales Gateway erforderlich. Bei Verwendung eines Gateways müssen 64-Bit-Anbieter installiert sein.

### <a name="in-memory-and-directquery"></a>In-Memory und DirectQuery

|Datenquelle | In-Memory-Anbieter | DirectQuery-Anbieter |
|  --- | --- | --- |
| SQL Server |SQL Server Native Client 11.0, Microsoft OLE DB-Anbieter für SQL Server, .NET Framework-Datenanbieter für SQL Server | .NET Framework-Datenanbieter für SQL Server |
| SQL Server Data Warehouse |SQL Server Native Client 11.0, Microsoft OLE DB-Anbieter für SQL Server, .NET Framework-Datenanbieter für SQL Server | .NET Framework-Datenanbieter für SQL Server |
| Oracle |Microsoft OLE DB-Anbieter für Oracle, Oracle-Datenanbieter für .NET |Oracle-Datenanbieter für .NET | |
| Teradata |OLE DB-Anbieter für Teradata, Teradata-Datenanbieter für .NET |Teradata-Datenanbieter für .NET | |
| | | |

\* nur tabellarische Modelle mit Kompatibilitätsgrad 1400.

### <a name="in-memory-only"></a>Nur In-Memory

> [!IMPORTANT]
> Die Anbietertests für die folgenden Datenquellen laufen noch. 

|Datenquelle  |  
|---------|---------|
|Access-Datenbank     |  
|Active Directory*     |  
|Analysis Services     | 
|Analytics Platform System     |  
|Dynamics CRM*     |  
|Excel-Arbeitsmappe     | 
|Exchange*     |  
|Ordner*     | 
|JSON-Dokument*     |  
|Zeilen aus Binärdatei*     | 
|MySQL Database     | 
|OData-Feed*     | 
|ODBC-Abfrage     | 
|OLE DB     |  
|Postgre SQL-Datenbank*    | 
|SAP HANA*    |   
|SAP Business Warehouse*    |  
|SharePoint*     |   
|Sybase-Datenbank     |  
|XML-Tabelle*    |  
|||
 
\* nur tabellarische Modelle mit Kompatibilitätsgrad 1400.

## <a name="specifying-a-different-provider"></a>Angeben eines anderen Herstellers

Datenmodelle in Azure Analysis Services erfordern möglicherweise verschiedene Datenanbieter beim Verbinden mit bestimmten Datenquellen. Gelegentlich kann es vorkommen, dass tabellarische Modelle beim Herstellen einer Verbindung mit Datenquellen mithilfe von nativen Anbietern wie SQL Server Native Client (SQLNCLI11) einen Fehler zurückgeben. Wenn Sie andere native Anbieter als SQLOLEDB verwenden, wird möglicherweise folgende Fehlermeldung angezeigt: **Der Anbieter „SQLNCLI11.1“ ist nicht registriert**. Wenn ein DirectQuery-Modell eine Verbindung mit lokalen Datenquellen herstellt und Sie native Anbieter verwenden, wird möglicherweise folgende Fehlermeldung angezeigt: **Error creating OLE DB row set. Incorrect syntax near „LIMIT“. (Fehler beim Erstellen eines OLE DB-Rowsets. Falsche Syntax bei „LIMIT“)**.

Bei der Migration eines lokalen SQL Server Analysis Services-Tabellenmodells zu Azure Analysis Services muss möglicherweise der Anbieter gewechselt werden.

**So geben Sie einen Anbieter an**

1. Klicken Sie unter SSDT > **Tabular Model Explorer** (Explorer für tabellarisches Modell)  > **Datenquellen** mit der rechten Maustaste auf eine Datenquellenverbindung, und klicken Sie anschließend auf **Datenquelle bearbeiten**.
2. Klicken Sie unter **Verbindung bearbeiten** auf **Erweitert**, um das Fenster „Erweiterte Eigenschaften“ zu öffnen.
3. Wählen Sie anschließend unter **Erweiterte Eigenschaften festlegen** > **Anbieter** den gewünschten Anbieter aus.

## <a name="impersonation"></a>Identitätswechsel
In einigen Fällen muss möglicherweise ein anderes Identitätswechselkonto angegeben werden. Das Identitätswechselkonto kann in Visual Studio (SSDT) oder SSMS angegeben werden.

Für lokale Datenquellen:

* Wenn Sie die SQL-Authentifizierung verwenden, sollte das Identitätswechselkonto ein Dienstkonto sein.
* Wenn Sie die Windows-Authentifizierung verwenden, legen Sie Windows-Benutzername/-Kennwort fest. Bei SQL Server wird die Windows-Authentifizierung mit einem bestimmten Identitätswechselkonto nur für In-Memory-Datenmodelle unterstützt.

Für Clouddatenquellen:

* Wenn Sie die SQL-Authentifizierung verwenden, sollte das Identitätswechselkonto ein Dienstkonto sein.

## <a name="next-steps"></a>Nächste Schritte
[Lokales Gateway](analysis-services-gateway.md)   
[Manage your server (Verwalten des Servers)](analysis-services-manage.md)   

