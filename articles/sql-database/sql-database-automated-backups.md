---
title: Automatische, georedundante Azure SQL-Datenbank-Sicherungen | Microsoft-Dokumentation
description: "SQL-Datenbank erstellt alle paar Minuten automatisch eine lokale Datenbanksicherung und verwendet georedundanten Azure-Speicher mit Lesezugriff (Read-Access Geographically Redundant Storage, RA-GRS), um für Georedundanz zu sorgen."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 3ee3d49d-16fa-47cf-a3ab-7b22aa491a8d
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: Active
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: 57132367faaa4c4869f2415f32897562c8056fb1
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="learn-about-automatic-sql-database-backups"></a>Informationen zu automatischen Sicherungen von SQL-Datenbank

SQL-Datenbank erstellt automatisch Datenbanksicherungen und verwendet georedundanten Azure-Speicher mit Lesezugriff (Read-Access Geographically Redundant Storage, RA-GRS), um für Georedundanz zu sorgen. Diese Sicherungen werden automatisch und ohne zusätzliche Kosten erstellt. Sie müssen keinerlei Aktionen durchführen. Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Wenn Sie Sicherungen in Ihrem eigenen Speichercontainer beibehalten möchten, können Sie eine Richtlinie für die langfristige Aufbewahrung der Sicherung konfigurieren. Weitere Informationen finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).

## <a name="what-is-a-sql-database-backup"></a>Was ist die Sicherung einer SQL-Datenbank-Instanz?

SQL-Datenbank verwendet die SQL Server-Technologie, um [vollständige Sicherungen](https://msdn.microsoft.com/library/ms186289.aspx), [differenzielle Sicherungen](https://msdn.microsoft.com/library/ms175526.aspx) und [Transaktionsprotokollsicherungen](https://msdn.microsoft.com/library/ms191429.aspx) zu erstellen. Die Transaktionsprotokollsicherungen erfolgen im Allgemeinen alle 5-10 Minuten, wobei die Häufigkeit auf der Leistungsebene und der Menge an Datenbankaktivität basiert. Transaktionsprotokollsicherungen mit vollständigen und differenziellen Sicherungen ermöglichen die Wiederherstellung einer Datenbank zu einem bestimmten Zeitpunkt auf dem gleichen Server, der die Datenbank hostet. Wenn Sie eine Datenbank wiederherstellen, ermittelt der Dienst, welche vollständigen und differenziellen Sicherungen bzw. Transaktionsprotokollsicherungen wiederhergestellt werden müssen.


Sie können diese Sicherungen für Folgendes verwenden:

* Stellen Sie eine Datenbank innerhalb der Aufbewahrungsdauer auf einen Zeitpunkt wieder her. Dieser Vorgang wird eine neue Datenbank auf dem gleichen Server wie die ursprüngliche Datenbank erstellen.
* Wiederherstellen einer gelöschten Datenbank auf den Zeitpunkt, zu dem sie gelöscht wurde, oder auf einen beliebigen Zeitpunkt innerhalb des Aufbewahrungszeitraums. Die gelöschte Datenbank kann nur auf dem Server wiederhergestellt werden, auf dem die ursprüngliche Datenbank erstellt wurde.
* Wiederherstellen einer Datenbank in einer anderen geografischen Region. Dies ermöglicht die Wiederherstellung nach dem Ausfall einer geografischen Region, wenn Sie keinen Zugriff auf Ihren Server und Ihre Datenbank haben. Dabei wird eine neue Datenbank auf einem beliebigen Server an einem beliebigen Ort der Welt erstellt. 
* Wiederherstellen einer Datenbank aus einer bestimmten Sicherung, die in Ihrem Azure Recovery Services-Tresor gespeichert ist. Dadurch können Sie eine alte Version der Datenbank wiederherstellen, um eine Konformitätsanforderung zu erfüllen oder eine alte Version der Anwendung auszuführen. Weitere Informationen hierzu finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).
* Informationen zum Durchführen einer Wiederherstellung finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen](sql-database-recovery-using-backups.md).

> [!NOTE]
> In Azure Storage bezieht sich der Begriff *Replikation* auf das Kopieren von Dateien von einem Speicherort zum anderen. Die *Datenbankreplikation* der SQL bezieht sich auf das Beibehalten von mehreren sekundären Datenbanken, die mit einer primären Datenbank synchronisiert werden. 
> 

## <a name="how-often-do-backups-happen"></a>Wie oft erfolgen Sicherungen?
Vollständige Datenbanksicherungen werden wöchentlich, differenzielle Datenbanksicherungen im Allgemeinen alle paar Stunden und Transaktionsprotokollsicherungen alle 5-10 Minuten ausgeführt. Die erste vollständige Sicherung wird unmittelbar nach der Datenbankerstellung geplant. Sie wird normalerweise innerhalb von 30 Minuten abgeschlossen, aber der Vorgang kann auch länger dauern, wenn es sich um eine sehr große Datenbank handelt. Die erste Sicherung kann bei einer wiederhergestellten Datenbank oder einer Datenbankkopie beispielsweise länger dauern. Nach der ersten vollständigen Sicherung werden alle weiteren Sicherungen automatisch geplant und im Hintergrund verwaltet. Die genaue Zeitplanung für alle Datenbanksicherungen wird vom SQL-Datenbank-Dienst je nach der gesamten Systemworkload bestimmt. 

Die Georeplikation des Sicherungsspeichers erfolgt basierend auf dem Azure Storage-Replikationszeitplan.

## <a name="how-long-do-you-keep-my-backups"></a>Wie lang werden meine Sicherungen aufbewahrt?
Jede SQL-Datenbank-Sicherung verfügt über einen Aufbewahrungszeitraum, der auf der [Dienstebene](sql-database-service-tiers.md) der Datenbank basiert. Folgende Aufbewahrungszeiträume gelten:


* Tarif „Basic“: 7 Tage
* Tarif „Standard“: 35 Tage
* Tarif „Premium“: 35 Tage

Wenn Sie Ihre Datenbank von den Tarifen „Standard“ oder „Premium“ auf „Basic“ herabstufen, werden die Sicherungen sieben Tage lang gespeichert. Alle vorhandenen Sicherungen, die älter sind als sieben Tage, sind nicht mehr verfügbar. 

Wenn Sie Ihre Datenbank vom Tarif „Basic“ auf „Standard“ oder „Premium“ heraufstufen, bewahrt SQL-Datenbank vorhandene Sicherungen so lange auf, bis sie 35 Tage alt sind. Neue Sicherungen werden 35 Tage lang aufbewahrt.

Wenn Sie eine Datenbank löschen, bewahrt SQL-Datenbank die Sicherungen auf die gleiche Weise auf wie für eine Onlinedatenbank. Ein Beispiel: Sie löschen eine Basic-Datenbank, die einen Aufbewahrungszeitraum von sieben Tagen aufweist. Eine vier Tage alte Sicherung wird drei weitere Tage gespeichert.

> [!IMPORTANT]
> Wenn Sie die Azure SQL Server-Instanz löschen, auf der die SQL-Datenbanken gehostet werden, werden alle Datenbanken, die zum Server gehören, gelöscht und können nicht wiederhergestellt werden. Es ist nicht möglich, einen gelöschten Server wiederherzustellen.
> 

## <a name="how-to-extend-the-backup-retention-period"></a>Wie kann der Aufbewahrungszeitraum für Sicherungen erweitert werden?
Wenn Ihre Anwendung erfordert, dass die Sicherungen für einen längeren Zeitraum verfügbar sind, können Sie den integrierten Aufbewahrungszeitraum erweitern, indem Sie die Richtlinie für die langfristige Aufbewahrung von Sicherungen (LTR-Richtlinie, Long-term backup retention) konfigurieren. Dadurch können Sie den integrierten Aufbewahrungszeitraum von 35 Tagen auf bis zu 10 Jahre erweitern. Weitere Informationen finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).

Sobald Sie die LTR-Richtlinie einer Datenbank mithilfe des Azure-Portals oder der API hinzufügen, werden die wöchentlichen vollständigen Datenbanksicherungen automatisch in Ihren Azure Backup-Diensttresor kopiert. Wenn Ihre Datenbank mit TDE verschlüsselt ist, werden die Sicherungen automatisch im Ruhezustand verschlüsselt.  Der Diensttresor löscht Ihre abgelaufenen Sicherungen basierend auf ihrem Zeitstempel und der LTR-Richtlinie automatisch.  Sie müssen also weder den Sicherungszeitplan verwalten noch sich Gedanken über die Bereinigung der alten Dateien machen. Die Wiederherstellungs-API unterstützt im Tresor gespeicherte Sicherungen, solange sich der Tresor im gleichen Abonnement wie Ihre SQL-Datenbank befindet. Sie können das Azure-Portal oder PowerShell verwenden, um auf diese Sicherungen zuzugreifen.

> [!TIP]
> Einen Leitfaden finden Sie unter [Konfiguration einer langfristig aufbewahrten Sicherung einer Azure SQL-Datenbank und die Wiederherstellung daraus](sql-database-long-term-backup-retention-configure.md).
>

## <a name="are-backups-encrypted"></a>Werden Sicherungen verschlüsselt?

Wenn Transparent Data Encryption (TDE) für eine Azure SQL-Datenbank aktiviert ist, werden die Sicherungen ebenfalls verschlüsselt. In allen neuen Azure SQL-Datenbanken ist TDE standardmäßig aktiviert. Weitere Informationen finden Sie unter [Transparente Datenverschlüsselung in Azure SQL-Datenbank](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="next-steps"></a>Nächste Schritte

- Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Weitere Informationen zu den anderen Geschäftskontinuitätslösungen von Azure SQL-Datenbank finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md).
- Informationen zur Wiederherstellung des Zustands zu einem bestimmten Zeitpunkt über das Azure-Portal finden Sie unter [Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt über das Azure-Portal](sql-database-recovery-using-backups.md).
- Informationen zur Wiederherstellung des Zustands zu einem bestimmten Zeitpunkt mithilfe von PowerShell finden Sie unter [Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt mit PowerShell](scripts/sql-database-restore-database-powershell.md).
- Informationen zum Konfigurieren, Verwalten und Wiederherstellen aus der langfristigen Aufbewahrung automatisierter Sicherungen in einem Azure Recovery Services-Tresor mit dem Azure-Portal finden Sie im Artikel über das [Verwalten der langfristigen Sicherungsaufbewahrung mit dem Azure-Portal](sql-database-long-term-backup-retention-configure.md).
- Informationen zum Konfigurieren, Verwalten und Wiederherstellen aus der langfristigen Aufbewahrung automatisierter Sicherungen in einem Azure Recovery Services-Tresor mit PowerShell finden Sie im Artikel über das [Verwalten der langfristigen Sicherungsaufbewahrung mit PowerShell](sql-database-long-term-backup-retention-configure.md).
