---
title: "Überwachen von XTP-In-Memory-Speicher | Microsoft Docs"
description: "Einschätzen und Überwachen der XTP-In-Memory-Speicherverwendung und -kapazität; Beheben des Kapazitätsfehlers 41823"
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: b617308e-692c-4938-8fa2-070034a3ecef
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jodebrui
ms.openlocfilehash: 1e7088e80cc86e3c7cf8ae8ea180d797de613e71
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="monitor-in-memory-oltp-storage"></a>Überwachen des In-Memory-OLTP-Speichers
Bei der Verwendung von [In-Memory-OLTP](sql-database-in-memory.md)befinden sich die Daten der speicheroptimierten Tabellen und Tabellenvariablen im In-Memory-OLTP-Speicher. Jeder Premium-Tarif weist eine maximale In-Memory-OLTP-Speichergröße auf, die in „Ressourceneinschränkungen für Azure SQL-Datenbank“ in den Abschnitten zu den [Ressourceneinschränkungen für einzelne Datenbanken](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels) und [Ressourceneinschränkungen für Pools für elastische Datenbanken](sql-database-resource-limits.md#elastic-pool-change-storage-size) dokumentiert ist. Wenn dieses Limit überschritten wird, tritt bei Einfüge- und Aktualisierungsvorgängen möglicherweise der Fehler 41823 (eigenständige Datenbanken) bzw. der Fehler 41840 (Pools für elastische Datenbanken) auf. An diesem Punkt müssen Sie entweder Daten löschen, um Speicherplatz freizugeben, oder die Leistungsstufe Ihrer Datenbank upgraden.

## <a name="determine-whether-data-fits-within-the-in-memory-oltp-storage-cap"></a>Bestimmen, ob genügend In-Memory-OLTP-Speicherplatz für die Daten zur Verfügung steht
Bestimmen Sie die Speicherkapazitäten der verschiedenen Premium-Dienstebenen. Siehe Abschnitte zu den [Ressourceneinschränkungen für einzelne Datenbanken](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels) und [Ressourceneinschränkungen für Pools für elastische Datenbanken](sql-database-resource-limits.md#elastic-pool-change-storage-size) in „Ressourceneinschränkungen für Azure SQL-Datenbank“.

Das Einschätzen des Speicherbedarfs für eine speicheroptimierte Tabelle funktioniert für SQL Server genauso wie in Azure SQL-Datenbank. Lesen Sie sich dazu kurz den Artikel auf [MSDN](https://msdn.microsoft.com/library/dn282389.aspx) durch.

Sowohl die Tabellen- und Tabellenvariablenzeilen als auch die Indizes werden in die maximale Größe für Benutzerdaten eingerechnet. Darüber hinaus benötigt ALTER TABLE genügend Platz, um eine neue Version der gesamten Tabelle und ihrer Indizes zu erstellen.

## <a name="monitoring-and-alerting"></a>Überwachung und Warnung
Sie können die Nutzung von In-Memory-Speicher als Prozentsatz der Speicherkapazität für Ihre Leistungsstufe im [Azure-Portal](https://portal.azure.com/) überwachen: 

1. Suchen Sie auf dem Blatt „Datenbank“ das Feld „Ressourcenverwendung“, und klicken Sie auf „Bearbeiten“.
2. Wählen Sie die Metrik `In-Memory OLTP Storage percentage` aus.
3. Um eine Warnung hinzuzufügen, klicken Sie auf das Feld „Ressourcenverwendung“, um das Blatt „Metrik“ zu öffnen, und klicken Sie dann auf „Warnung hinzufügen“.

Oder verwenden Sie folgende Abfrage, um die In-Memory-Speicherverwendung anzuzeigen:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-in-memory-oltp-storage-situations---errors-41823-and-41840"></a>Behandeln von Situationen mit zu wenig In-Memory-OLTP-Speicher: Fehler 41823 und 41840
Bei Erreichen der Obergrenze des In-Memory-OLTP-Speichers in Ihrer Datenbank tritt bei INSERT-, UPDATE-, ALTER- und CREATE-Vorgängen der Fehler 41823 (eigenständige Datenbanken) bzw. der Fehler 41840 (Pools für elastische Datenbanken) auf. Beide Fehler führen zum Abbruch der aktiven Transaktion.

Die Fehlermeldungen 41823 und 41840 geben an, dass die speicheroptimierten Tabellen und Tabellenvariablen in der Datenbank oder im Pool die maximale In-Memory-OLTP-Speichergröße erreicht haben.

Beheben Sie den Fehler mit einer der folgenden Methoden:

* Löschen Sie Daten aus den speicheroptimierten Tabellen – Sie können die Daten beispielsweise in herkömmliche datenträgerbasierte Tabellen auslagern.
* Führen Sie ein Upgrade auf eine Dienstebene durch, die genügend In-Memory-Speicher für die Daten bietet, die Sie in In-Memory-Tabellen speichern müssen.

> [!NOTE] 
> In seltenen Fällen treten die Fehler 41823 und 41840 nur vorübergehend auf. In diesem Fällen steht genügend In-Memory-OLTP-Speicher zur Verfügung, und der Vorgang ist erfolgreich, wenn er erneut ausgeführt wird. Es empfiehlt sich daher, den insgesamt verfügbaren In-Memory OLTP-Speicher zu überwachen und den Vorgang beim erstmaligen Auftreten des Fehlers 41823 oder 41840 zu wiederholen. Weitere Informationen zur Wiederholungslogik finden Sie unter [Konflikterkennung und Wiederholungslogik](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables#conflict-detection-and-retry-logic).

## <a name="next-steps"></a>Nächste Schritte
Eine Anleitung zur Überwachung finden Sie unter [Überwachen von Azure SQL-Datenbank mit dynamischen Verwaltungssichten](sql-database-monitoring-with-dmvs.md).
