---
title: Wiederherstellen eines Servers in Azure Database for MySQL
description: "In diesem Artikel wird beschrieben, wie Sie mit dem Azure-Portal einen Server in Azure-Datenbank für MySQL wiederherstellen."
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 5bef3f11d0b546fbd6b1161b20d7dfb81e975f99
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-mysql-using-the-azure-portal"></a>Sichern und Wiederherstellen eines Servers in Azure Database for MySQL mit dem Azure-Portal

## <a name="backup-happens-automatically"></a>Automatische Sicherung
Azure Database for MySQL-Server werden regelmäßig gesichert, um Wiederherstellungsfunktionen zu ermöglichen. Mithilfe dieses Features können Sie für den Server und alle dazugehörigen Datenbanken einen Zustand zu einem früheren Zeitpunkt auf einem neuen Server wiederherstellen.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:
- Einen [Azure Database for MySQL-Server und eine Datenbank](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="set-backup-configuration"></a>Festlegen der Sicherungskonfiguration

Bei der Erstellung des Servers treffen Sie im Fenster **Tarif** die Entscheidung, ob dafür lokal redundante oder georedundante Sicherungen erstellt werden sollen.

> [!NOTE]
> Nachdem ein Server erstellt wurde, kann die Redundanzart (georedundant oder lokal redundant) nicht mehr gewechselt werden.
>

Bei der Erstellung eines Servers über das Azure-Portal wählen Sie im Fenster **Tarif** entweder **Lokal redundant** oder **Georedundant** für die Sicherungen Ihres Servers aus. Außerdem wählen Sie in diesem Fenster die **Aufbewahrungszeit für Sicherung** aus. Hier wird angegeben, wie lange die Sicherungen gespeichert werden sollen (in Tagen).

   ![Tarif – Auswählen der Sicherungsredundanz](./media/howto-restore-server-portal/pricing-tier.png)

Weitere Informationen zum Festlegen dieser Werte während der Erstellung finden Sie unter [Erstellen eines Servers für Azure Database for MySQL über das Azure-Portal](quickstart-create-mysql-server-database-using-azure-portal.md).

Die Option „Aufbewahrungszeit für Sicherung“ kann auf einem Server mit den folgenden Schritten geändert werden:
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie Ihren Server für Azure Database for MySQL aus. Mit dieser Aktion wird die Seite **Übersicht** geöffnet.
3. Wählen Sie im Menü unter **EINSTELLUNGEN** die Option **Tarif**. Mit dem Schieberegler können Sie die **Aufbewahrungszeit für Sicherung** auf einen Wert zwischen 7 und 35 Tagen festlegen.
Im folgenden Screenshot wurde der Wert auf 34 Tage erhöht.
![Erhöhung der Aufbewahrungszeit für Sicherung](./media/howto-restore-server-portal/3-increase-backup-days.png)

4. Klicken Sie auf **OK**, um die Änderung zu bestätigen.

Mit „Aufbewahrungszeit für Sicherung“ wird auch gesteuert, für welchen zurückliegenden Zeitraum eine Point-in-Time-Wiederherstellung durchgeführt werden kann, da dies auf den verfügbaren Sicherungen basiert. Die Point-in-Time-Wiederherstellung wird im folgenden Abschnitt näher beschrieben. 

## <a name="point-in-time-restore-in-the-azure-portal"></a>Point-in-Time-Wiederherstellung im Azure-Portal
Mit Azure Database for MySQL können Sie den Zustand des Servers zu einem bestimmten Zeitpunkt und auf einer neuen Kopie des Servers wiederherstellen. Sie können diesen neuen Server verwenden, um Ihre Daten wiederherzustellen, oder Ihre Clientanwendungen so einrichten, dass sie auf diesen neuen Server verweisen.

Beispiel: Wenn eine Tabelle heute um 12 Uhr versehentlich gelöscht wurde, können Sie den Zustand von kurz vor 12 Uhr wiederherstellen und die fehlende Tabelle und die Daten von dieser neuen Kopie des Servers abrufen. Die Point-in-Time-Wiederherstellung wird nicht auf Datenbankebene, sondern auf der Serverebene durchgeführt.

Mithilfe der folgenden Schritte wird der Status des Beispielservers zu einem bestimmten Zeitpunkt wiederhergestellt:
1. Wählen Sie im Azure-Portal Ihren Azure Database for MySQL-Server aus. 

2. Klicken Sie in der Symbolleiste auf der Seite **Übersicht** des Servers auf **Wiederherstellen**.

   ![Azure Database for MySQL – Übersicht – Schaltfläche „Wiederherstellen“](./media/howto-restore-server-portal/2-server.png)

3. Geben Sie im Formular „Wiederherstellen“ die erforderlichen Informationen ein:

   ![Azure Database for MySQL – Wiederherstellungsinformationen ](./media/howto-restore-server-portal/3-restore.png)
  - **Wiederherstellungspunkt**: Wählen Sie den Zeitpunkt aus, für den der Zustand wiederhergestellt werden soll.
  - **Zielserver**: Geben Sie einen Namen für den neuen Server an.
  - **Standort**: Sie können die Region nicht auswählen. Standardmäßig ist dieser Wert mit dem Wert für den Quellserver identisch.
  - **Tarif**: Sie können diese Parameter nicht ändern, wenn Sie eine Point-in-Time-Wiederherstellung durchführen. Er ist mit dem Wert für den Quellserver identisch. 

4. Klicken Sie auf **OK**, um den Zustand eines Servers zu einem bestimmten Zeitpunkt wiederherzustellen. 

5. Suchen Sie nach Abschluss der Wiederherstellung den neuen erstellten Server, um zu überprüfen, ob die Daten wie erwartet wiederhergestellt wurden.

>[!Note]
>Beachten Sie Folgendes: Der neue Server, der durch die Point-in-Time-Wiederherstellung erstellt wurde, verfügt über den gleichen Serveradministrator-Anmeldenamen (und das dazugehörige Kennwort), der für den vorhandenen Server zum gewählten Zeitpunkt gültig war. Sie können das Kennwort auf der Seite **Übersicht** des neuen Servers ändern.

## <a name="next-steps"></a>Nächste Schritte
- Informieren Sie sich über die [Sicherungen](concepts-backup.md) des Diensts.
- Informieren Sie sich über die Optionen in Bezug auf die [Geschäftskontinuität](concepts-business-continuity.md).
