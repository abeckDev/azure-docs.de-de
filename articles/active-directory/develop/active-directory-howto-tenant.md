---
title: Einrichten eines Azure AD-Mandanten | Microsoft Docs
description: "Informationen zum Einrichten einen Azure Active Directory-Mandanten für das Registrieren und Erstellen von Anwendungen."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.assetid: 1f4b24eb-ab4d-4baa-a717-2a0e5b8d27cd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/19/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 85783d58b2b02a9d0c6230429bebf2806514dee5
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-get-an-azure-active-directory-tenant"></a>Einrichten eines Azure Active Directory-Mandanten
In Azure Active Directory (Azure AD) ist ein [Mandant](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) ein Stellvertreter einer Organisation.  Es handelt sich um eine dedizierte Instanz des Azure AD-Diensts, den eine Organisation erhält und besitzt, nachdem sie sich für einen Microsoft-Clouddienst wie Azure, Microsoft Intune oder Office 365 registriert hat.  Jeder Azure AD-Mandant ist eindeutig und von anderen Azure AD-Mandanten getrennt.  

Ein Mandant enthält die Benutzer in einem Unternehmen und die dazugehörigen Informationen, wie z. B. Kennwörter, Benutzerprofildaten, Berechtigungen usw.  Er enthält außerdem Gruppen, Anwendungen und andere Informationen, die eine Organisation und ihre Sicherheit betreffen.

Damit sich Azure AD-Benutzer bei Ihrer Anwendung anmelden können, müssen Sie Ihre Anwendung in einem eigenen Mandanten registrieren.  Das Veröffentlichen einer Anwendung in einem Azure AD-Mandanten ist **völlig kostenlos**.  Die meisten Entwickler erstellen für Experimentier-, Entwicklungs-, Staging- und Testzwecke meist mehrere Mandanten und Anwendungen.  Organisationen, die sich für die Nutzung Ihrer Anwendung registrieren, können optional auch Lizenzen erwerben, wenn sie erweiterte Verzeichnisfeatures nutzen möchten.

Wie also wird ein Azure AD-Mandanten eingerichtet?  Der Prozess ist möglicherweise ein wenig unterschiedlich, wenn Sie:

* [bereits für ein Office 365-Abonnement verfügen](#use-an-existing-office-365-subscription)
* [bereits über ein Azure-Abonnement verfügen, das einem Microsoft-Konto zugeordnet ist](#use-an-msa-azure-subscription)
* [bereits über ein Azure-Abonnement verfügen, das einem Organisationskonto zugeordnet ist](#use-an-organizational-azure-subscription)
* [über keines der genannten verfügen und ganz von vorn beginnen möchten](#start-from-scratch)

## <a name="use-an-existing-office-365-subscription"></a>Verwenden eines vorhandenes Office 365-Abonnements
Wenn Sie im Besitz eines Office 365-Abonnements sind, verfügen Sie bereits über einen Azure AD-Mandanten. Sie können sich am [Azure-Portal](https://portal.azure.com) mit Ihrem O365-Konto anmelden und mit der Nutzung von Azure AD beginnen.

## <a name="use-an-msa-azure-subscription"></a>Verwenden eines Azure-MSA-Abonnements
Wenn Sie sich zuvor mit Ihrem persönlichen Microsoft-Konto für ein Azure-Abonnement registriert haben, verfügen Sie bereits über einen Mandanten!  Wenn Sie sich beim [Azure-Portal](https://portal.azure.com) anmelden, werden Sie automatisch bei Ihrem Standardmandanten angemeldet. Sie können diesen Mandanten wie gewünscht nutzen, sollten aber das Erstellen eines Organisationsadministratorkontos erwägen.

Gehen Sie dazu folgendermaßen vor.  Alternativ können Sie einen neuen Mandanten und in diesem Mandanten einen Administrator erstellen, indem Sie einen ähnlichen Prozess befolgen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit Ihrem persönlichen Konto an.
2. Navigieren Sie auf der linken Navigationsleiste unter **Alle Dienste** zum Abschnitt „Azure Active Directory“ des Portals.
3. Sie werden automatisch am „Standardverzeichnis“ angemeldet. Falls nicht, können Sie das Verzeichnis wechseln, indem Sie oben rechts auf Ihren Kontonamen klicken.
4. Wählen Sie im Abschnitt **Schnelle Aufgaben** die Option **Benutzer hinzufügen**.
5. Geben Sie zum Hinzufügen eines Benutzers die folgenden Details in das Formular ein:

   * Name: (Wählen Sie einen passenden Wert.)
   * Benutzername: (Wählen Sie einen Benutzernamen für diesen Administrator.)
   * Profil: (Geben Sie die entsprechenden Werte für Vorname, Nachname, Position und Abteilung ein.)
   * Rolle: Globaler Administrator
6. Wenn Sie das Formular „Benutzer hinzufügen“ ausgefüllt und das temporäre Kennwort für den neuen Administrator erhalten haben, müssen Sie dieses Kennwort aufzeichnen, da Sie sich als dieser neue Benutzer anmelden müssen, um das Kennwort zu ändern. Sie können das Kennwort auch mithilfe einer alternativen E-Mail-Adresse direkt an den Benutzer senden.
7. Klicken Sie auf **Erstellen**, um den neuen Benutzer zu erstellen.
8. Um das temporäre Kennwort zu ändern, melden Sie sich bei [https://login.microsoftonline.com](https://login.microsoftonline.com) mit diesem neuen Benutzerkonto an, und ändern Sie das Kennwort nach der entsprechenden Aufforderung.

## <a name="use-an-organizational-azure-subscription"></a>Verwenden eines Azure-Organisationsabonnements
Wenn Sie sich zuvor mit Ihrem Organisationskonto für ein Azure-Abonnement registriert haben, verfügen Sie bereits über einen Mandanten!  Im [Azure-Portal](https://portal.azure.com) sollte ein Mandant angezeigt werden, wenn Sie zu „Alle Dienste“ und „Azure Active Directory“ navigieren.  Sie können diesen Mandanten gemäß Ihren Anforderungen nutzen.

## <a name="start-from-scratch"></a>Ganz von vorn beginnen
Wenn Ihnen diese genannten Schritte unverständlich sind, machen Sie sich keine Sorgen. Erstellen Sie einfach im [Azure-Portal](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory) ein neues Azure AD-Verzeichnis. Nachdem Sie den Vorgang abgeschlossen haben, erhalten Sie einen eigenen Azure AD-Mandanten mit dem Domänennamen, den Sie bei der Registrierung gewählt haben.  Sie finden Ihren Mandanten im [Azure-Portal](https://portal.azure.com), indem Sie im linken Navigationsbereich zu **Azure Active Directory** navigieren.
