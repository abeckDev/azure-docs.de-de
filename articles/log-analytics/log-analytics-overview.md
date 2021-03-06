---
title: Was ist Log Analytics in Azure? | Microsoft-Dokumentation
description: "Log Analytics ist ein Dienst in Azure, mit dem Sie betriebsbezogene Daten sammeln und analysieren können, die von Ressourcen in Ihren Cloud- und lokalen Umgebungen generiert werden.  Dieser Artikel bietet eine kurze Übersicht über die verschiedenen Komponenten von Log Analytics sowie Links zu weiteren Informationen."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: bd90b460-bacf-4345-ae31-26e155beac0e
ms.service: log-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2018
ms.author: bwren
ms.openlocfilehash: a95528f5bd259a36ea96c7bc0660ca082c09d6e6
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2018
---
# <a name="what-is-log-analytics"></a>Was ist Log Analytics?
Log Analytics ist ein Dienst in Azure, der Ihre cloudbasierten und lokalen Umgebungen überwacht, um die Verfügbarkeit und Leistung sicherzustellen.  Er sammelt Daten, die von Ressourcen in Ihren cloudbasierten und lokalen Umgebungen sowie von anderen Überwachungstools generiert werden, um Analysen für mehrere Quellen zu ermöglichen.  Dieser Artikel enthält eine kurze Erläuterung des Nutzens von Log Analytics, eine Übersicht über die Funktionsweise sowie Links zu ausführlicheren Inhalten mit weiteren Informationen.

## <a name="is-log-analytics-for-you"></a>Ist Log Analytics für Sie interessant?
Falls Sie derzeit über keine Überwachungslösung für Ihre Azure-Umgebung verfügen, sollten Sie mit [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md) beginnen. Dieser Dienst sammelt und analysiert Überwachungsdaten für Ihre Azure-Ressourcen.  Log Analytics kann [Daten von Azure Monitor erfassen](log-analytics-azure-storage.md), mit anderen Daten abgleichen und zusätzliche Analysen bereitstellen.

Wenn Sie Ihre lokale Umgebung überwachen möchten oder bereits über eine Überwachungslösung mit Diensten wie Azure Monitor oder System Center Operations Manager verfügen, kann Log Analytics sehr hilfreich sein.  Der Dienst kann Daten direkt von Ihren Agents sowie von diesen anderen Tools sammeln und in einem einzelnen Repository zusammenfassen.  Analysetools in Log Analytics (wie Protokollsuche, Ansichten und Lösungen) berücksichtigen alle gesammelten Daten und bieten somit eine zentrale Analyse Ihrer gesamten Umgebung.


## <a name="using-log-analytics"></a>Verwenden von Log Analytics
Auf die Log Analytics-Funktionalität können Sie über das Azure-Portal zugreifen. Diese kann in einem beliebigen Browser ausgeführt werden und bietet Zugriff auf Konfigurationseinstellungen und verschiedene Tools, mit denen Sie die gesammelten Daten analysieren und darauf reagieren können.  Im Portal können Sie mithilfe von [Protokollsuchvorgängen](log-analytics-log-searches.md) Abfragen erstellen, um die gesammelten Daten zu analysieren. Sie können [Dashboards](log-analytics-dashboards.md) nutzen und Ihre wichtigsten Suchvorgänge mit grafischen Ansichten anpassen. Darüber hinaus bieten [Lösungen](log-analytics-add-solutions.md) zusätzliche Funktionalität und Analysemöglichkeiten.

In der folgenden Abbildung ist der Übersichtsbildschirm dargestellt, auf dem die Informationen zu den im Arbeitsbereich installierten [Lösungen](#add-functionality-with-management-solutions) zusammengefasst sind.  Sie können auf eine beliebige Kachel klicken, um einen Drilldown in die Daten für die betreffende Lösung durchzuführen.

![OMS-Portal](media/log-analytics-overview/portal.png)

Log Analytics bietet eine Abfragesprache, mit der Sie Daten schnell abrufen und im Repository konsolidieren können.  Sie können [Protokollsuchen](log-analytics-log-searches.md) erstellen und speichern, um Daten direkt im Portal zu analysieren, oder automatisch ausgeführte Protokollsuchen verwenden, um Warnungen zu generieren, wenn die Ergebnisse einer Abfrage auf eine wichtige Bedingung hindeuten.

![Protokollsuche](media/log-analytics-overview/log-search.png)

Um Daten außerhalb von Log Analytics zu analysieren, können Sie die Daten in Tools wie [Power BI](log-analytics-powerbi.md) oder Excel exportieren.  Sie können auch die [Protokollsuch-API](log-analytics-log-search-api.md) verwenden, um benutzerdefinierte Lösungen zu erstellen, die Log Analytics-Daten nutzen, oder um sie in andere Systeme zu integrieren.

## <a name="add-functionality-with-management-solutions"></a>Hinzufügen von Funktionen mit Verwaltungslösungen
[Verwaltungslösungen](log-analytics-add-solutions.md) erweitern den Funktionsumfang von Log Analytics und stellen zusätzliche Daten und Analysetools für Log Analytics bereit.  Sie können auch neue Datensatztypen definieren, die gesammelt und mit Protokollsuchen oder über eine zusätzliche Benutzeroberfläche analysiert werden können, die von der Lösung im Dashboard bereitgestellt wird.  Die folgende Beispielabbildung zeigt die [Lösung für die Änderungsnachverfolgung](log-analytics-change-tracking.md).

![Lösung für die Änderungsnachverfolgung](media/log-analytics-overview/change-tracking.png)

Lösungen stehen für verschiedenste Funktionen zur Verfügung, und es kommen kontinuierlich weitere Lösungen hinzu.  Im Azure Marketplace können Sie komfortabel nach verfügbaren Lösungen suchen und sie [Ihrem Arbeitsbereich hinzufügen](log-analytics-add-solutions.md).  Viele werden automatisch bereitgestellt und können sofort verwendet werden, bei anderen sind unter Umständen noch Konfigurationsschritte erforderlich.

![Lösungskatalog](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-components"></a>Log Analytics-Komponenten
Im Mittelpunkt von Log Analytics steht ein Repository mit gesammelten Daten, das in der Azure-Cloud gehostet wird.  Konfigurieren Sie Datenquellen, und fügen Sie Ihrem Abonnement Lösungen hinzu, um Daten aus verbundenen Quellen zu sammeln.  Datenquellen und Lösungen erzeugen verschiedene Arten von Datensätzen, die jeweils über einen eigenen Eigenschaftensatz verfügen, aber dennoch in Abfragen im Repository gemeinsam analysiert werden können.  So können Sie die gleichen Tools und Methoden für verschiedene Arten von Daten verwenden, die aus verschiedenen Quellen gesammelt werden.

![Log Analytics-Komponenten](media/log-analytics-overview/overview.png)

Bei den verbundenen Quellen handelt es sich um die Computer und anderen Ressourcen, die die Daten generieren, die von Log Analytics gesammelt werden.  Hierzu können Agents gehören, die auf direkt verbundenen [Windows](log-analytics-windows-agent.md)- oder [Linux](log-analytics-linux-agents.md)-Computern installiert sind, oder Agents in einer [verbundenen System Center Operations Manager-Verwaltungsgruppe](log-analytics-om-agents.md).  Bei Azure-Ressourcen erfasst Log Analytics Daten aus [Azure Monitor und Azure Diagnostics](log-analytics-azure-storage.md).

[Datenquellen](log-analytics-data-sources.md) sind die verschiedenen Arten von Daten, die aus jeder verbundenen Quelle gesammelt werden.  Hierzu zählen [Ereignisse](log-analytics-data-sources-windows-events.md) und [Leistungsdaten](log-analytics-data-sources-performance-counters.md) aus [Windows](log-analytics-data-sources-windows-events.md)- und Linux-Agents sowie Quellen wie [IIS-Protokolle](log-analytics-data-sources-iis-logs.md) und [benutzerdefinierte Textprotokolle](log-analytics-data-sources-custom-logs.md).  Sie konfigurieren jede Datenquelle, aus der Sie Daten sammeln möchten, und die Konfiguration wird automatisch an jede verbundene Quelle übermittelt.

Wenn Sie besondere Anforderungen haben, können Sie die [HTTP-Datensammler-API](log-analytics-data-collector-api.md) verwenden, um Daten eines REST-API-Clients in das Repository zu schreiben.

## <a name="log-analytics-architecture"></a>Log Analytics-Architektur
Die Bereitstellungsanforderungen von Log Analytics sind minimal, da die zentralen Komponenten in der Azure-Cloud gehostet werden.  Dies umfasst auch das Repository sowie die Dienste, mit deren Hilfe Sie gesammelte Daten korrelieren und analysieren können.  Sie können in jedem Browser auf das Portal zugreifen, Clientsoftware wird nicht benötigt.

Sie müssen Agents auf [Windows](log-analytics-windows-agent.md)- und [Linux](log-analytics-linux-agents.md)-Computern installieren. Für Computer, die bereits zu einer [verbundenen System Center Operations Manager-Verwaltungsgruppe](log-analytics-om-agents.md) gehören, ist kein zusätzlicher Agent erforderlich.  Operations Manager-Agents kommunizieren weiterhin mit Verwaltungsservern, die ihre Daten an Log Analytics weiterleiten.  Bei einigen Lösungen ist es jedoch erforderlich, dass Agents direkt mit Log Analytics kommunizieren.  Informationen zu den Kommunikationsanforderungen finden Sie in der Dokumentation zu den jeweiligen Lösungen.

Wenn Sie sich [bei Log Analytics registrieren](log-analytics-get-started.md), erstellen Sie einen Arbeitsbereich.  Sie können sich diesen Arbeitsbereich als einzigartige Log Analytics-Umgebung mit eigenem Datenrepository, eigenen Datenquellen und eigenen Lösungen vorstellen. Sie können mehrere Arbeitsbereiche in Ihrem Abonnement erstellen, um mehrere Umgebungen z.B. zu Produktions- und Testzwecken zu unterstützen.

![Log Analytics-Architektur](media/log-analytics-overview/architecture.png)

## <a name="next-steps"></a>Nächste Schritte
* [Registrieren Sie sich für ein kostenloses Log Analytics-Konto](log-analytics-get-started.md) , um es in Ihrer eigenen Umgebung auszuprobieren.
* Sehen Sie sich die verschiedenen verfügbaren [Datenquellen](log-analytics-data-sources.md) an, aus denen Daten für Log Analytics gesammelt werden können.
* [Durchsuchen Sie die verfügbaren Lösungen im Lösungskatalog](log-analytics-add-solutions.md) , um Funktionen zu Log Analytics hinzuzufügen.

