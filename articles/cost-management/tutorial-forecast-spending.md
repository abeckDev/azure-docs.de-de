---
title: 'Tutorial: Prognostizieren der Ausgaben mit Azure Cost Management | Microsoft-Dokumentation'
description: In diesem Tutorial wird beschrieben, wie Sie die Ausgaben prognostizieren, indem Sie die Daten zum Nutzungsverlauf und zu den Ausgaben verwenden.
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 02/27/2018
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 35142cb40560db848c71da266bbaa1881f12e15d
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="tutorial-forecast-future-spending"></a>Tutorial: Prognostizieren zukünftiger Ausgaben

Mit der Azure-Kostenverwaltung von Cloudyn können Sie zukünftige Ausgaben mithilfe von historischer Nutzung und Ausgabendaten prognostizieren. Sie verwenden Cloudyn-Berichte, um alle Kostenprognosedaten anzuzeigen. Die Beispiele dieses Tutorials führen Sie in einer exemplarischen Vorgehensweise durch das Überprüfen der Kostenprognosen mithilfe der Berichte. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Vorhersage zukünftiger Ausgaben

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen ein Azure-Abonnement.
- Sie müssen entweder über eine Registrierung für die Testversion oder ein kostenpflichtiges Abonnement für Azure Cost Management verfügen.

## <a name="forecast-future-spending"></a>Vorhersage zukünftiger Ausgaben

Cloudyn enthält Kostenprognoseberichte, anhand derer Sie Ausgaben basierend auf Ihrer Nutzung im Laufe der Zeit vorhersagen können. Der Hauptzweck besteht darin, Sie dabei zu unterstützen, sicherzustellen, dass Ihre Kostentrends die Erwartungen Ihrer Organisation nicht überschreiten. Die Berichte, die Sie verwenden, beziehen sich auf die aktuellen monatlichen vorhergesagten und jährlichen vorhergesagten Kosten. Beide zeigen, welche Ausgaben zu erwarten sind, wenn Ihre Nutzung mit der Nutzung innerhalb der letzten 30 Tage relativ konsistent bleibt.

Der Bericht über die aktuellen monatlichen vorhergesagten Kosten zeigt die Kosten für Ihre Dienste. Er bezieht sich auf Kosten ab Anfang des Monats und des vorhergehenden Monats, um die voraussichtlichen Kosten anzuzeigen. Klicken Sie im Berichtsmenü am oberen Rand des Portals auf **Cost** > **Projection and Budget** > **Current Month Projected Cost**. Die folgende Abbildung zeigt ein Beispiel.

![Vorhergesagte Kosten im aktuellen Monat](./media/tutorial-forecast-spending/project-month01.png)

Im Beispiel können Sie sehen, für welche Dienste die höchsten Kosten entstanden sind. Azure-Kosten waren niedriger als AWS Kosten. Wenn Sie die Kostenprognose für Azure-VMs im Detail sehen möchten, wählen Sie in der Liste **Filter** **Azure/VM**.

![Vorhergesagte Kosten für Azure-VM im aktuellen Monat](./media/tutorial-forecast-spending/project-month02.png)

Führen Sie die gleichen grundlegenden Schritte aus, um die Prognose der monatlichen Kosten für andere Dienste anzuzeigen, die Sie interessieren.

Der Bericht über die vorhergesagten jährlichen Kosten zeigt die extrapolierten Kosten für Ihre Dienste für die nächsten 12 Monate an.

Klicken Sie im Berichtsmenü am oberen Rand des Portals auf **Cost** > **Projection and Budget** > **Annual Projected Cost**. Die folgende Abbildung zeigt ein Beispiel.

![Bericht über die vorhergesagten jährlichen Kosten](./media/tutorial-forecast-spending/project-annual01.png)

Im Beispiel können Sie sehen, für welche Dienste die höchsten Kosten entstanden sind. Wie im monatlichen Beispiel waren die Azure-Kosten niedriger als die AWS Kosten. Wenn Sie die Kostenprognose für Azure-VMs im Detail sehen möchten, wählen Sie in der Liste **Filter** **Azure/VM**.

![Vorhergesagte jährliche VM-Kosten](./media/tutorial-forecast-spending/project-annual02.png)

In der Abbildung oben belaufen sich die vorhergesagten jährlichen Kosten der Azure-VMs auf 28.374 US-Dollar.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Vorhersage zukünftiger Ausgaben


Wechseln Sie zum nächsten Tutorial, um zu erfahren, wie Sie Kosten mit Kostenzuteilung und Showbackberichten verwalten.

> [!div class="nextstepaction"]
> [Manage costs by using Azure Cost Management](tutorial-manage-costs.md) (Verwalten von Kosten mit Azure Cost Management)
