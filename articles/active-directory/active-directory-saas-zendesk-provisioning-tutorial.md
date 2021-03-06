---
title: "Tutorial: Konfigurieren von ZenDesk für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in ZenDesk konfigurieren."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: 1a1414eefd20e6d7c025da08cfd5ae7c45daad33
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---

# <a name="tutorial-configuring-zendesk-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von ZenDesk für die automatische Benutzerbereitstellung


Dieses Tutorial zeigt Ihnen die Schritte, die Sie in ZenDesk und Azure AD ausführen müssen, um Benutzerkonten von Azure AD in ZenDesk automatisch bereitzustellen bzw. deren Bereitstellung automatisch aufzuheben. 

## <a name="prerequisites"></a>Voraussetzungen

Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

*   Einen Azure Active Directory-Mandanten
*   Einen ZenDesk-Mandanten, für den mindestens der [Enterprise-Tarif](https://www.zendesk.com/product/pricing/) aktiviert ist 
*   Ein Benutzerkonto in ZenDesk mit Teamadministratorberechtigungen 

> [!NOTE]
> Die Integration der Azure AD-Bereitstellung basiert auf der [ZenDesk-REST-API](https://developer.zendesk.com/rest_api/docs/core/introduction#the-api), die für ZenDesk-Teams mit dem Essential-Tarif oder einem höheren Tarif zur Verfügung steht.

## <a name="assigning-users-to-zendesk"></a>Zuweisen von Benutzern zu ZenDesk

Azure Active Directory ermittelt anhand von Zuweisungen, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Bereitstellung von Benutzerkonten werden nur die Benutzer und Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden. 

Vor dem Konfigurieren und Aktivieren des Bereitstellungsdiensts müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD die Benutzer darstellen, die Zugriff auf Ihre ZenDesk-App benötigen. Anschließend können Sie diese Benutzer Ihrer ZenDesk-App zuweisen, indem Sie diese Anweisungen befolgen:

[Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-zendesk"></a>Wichtige Tipps zum Zuweisen von Benutzern zu ZenDesk

*   Es wird empfohlen, ZenDesk einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der Bereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

*   Beim Zuweisen eines Benutzers zu ZenDesk müssen Sie entweder die Rolle **Benutzer** oder eine andere gültige anwendungsspezifische Rolle (sofern verfügbar) im Dialogfeld für die Zuweisung auswählen. Die Rolle **Standardzugriff** funktioniert nicht für die Bereitstellung. Diese Benutzer werden übersprungen.

> [!NOTE]
> Als zusätzliches Feature liest der Bereitstellungsdienst alle in Zendesk festgelegten benutzerdefinierten Rollen und importiert sie in Azure AD. Dort können sie im Dialogfeld „Rolle auswählen“ ausgewählt werden. Diese Rollen werden im Azure-Portal angezeigt, nachdem der Bereitstellungsdienst aktiviert und ein Synchronisierungszyklus abgeschlossen wurde.

## <a name="configuring-user-provisioning-to-zendesk"></a>Konfigurieren der Benutzerbereitstellung in ZenDesk 

Dieser Abschnitt führt Sie durch das Herstellen einer Verbindung von Azure AD mit der ZenDesk-API zur Bereitstellung von Benutzerkonten sowie durch das Konfigurieren des Bereitstellungsdiensts für das Erstellen, Aktualisieren und Deaktivieren zugewiesener Benutzerkonten in ZenDesk basierend auf der Benutzer- und Gruppenzuweisung in Azure AD.

> [!TIP] 
> Sie können auch das SAML-basierte einmalige Anmelden für ZenDesk aktivieren. Befolgen Sie hierzu die Anweisungen im [Azure-Portal](https://portal.azure.com). Einmaliges Anmelden kann unabhängig von der automatischen Bereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.


### <a name="configure-automatic-user-account-provisioning-to-zendesk-in-azure-ad"></a>Konfigurieren der automatischen Bereitstellung von Benutzerkonten für ZenDesk in Azure AD


1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zum Abschnitt **Azure Active Directory > Unternehmens-Apps > Alle Anwendungen**.

2. Wenn Sie ZenDesk bereits für einmaliges Anmelden konfiguriert haben, suchen Sie über das Suchfeld nach Ihrer ZenDesk-Instanz. Wählen Sie andernfalls **Hinzufügen**, und suchen Sie im Anwendungskatalog nach **ZenDesk**. Wählen Sie ZenDesk in den Suchergebnissen aus, und fügen Sie es Ihrer Anwendungsliste hinzu.

3. Wählen Sie Ihre ZenDesk-Instanz aus, und wählen Sie dann die Registerkarte **Bereitstellung**.

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![ZenDesk-Bereitstellung](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk1.png)

5. Geben Sie im Abschnitt **Admin Credentials** (Administratoranmeldeinformationen) den von Ihrem ZenDesk-Konto generierten **Administratorbenutzernamen, den generierten Tokenschlüssel und die erstellte Domäne** ein. (Sie finden das Token in Ihrem Konto: **Admin** > **API** > **Einstellungen**). 

    ![ZenDesk-Bereitstellung](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk2.png)

6. Klicken Sie im Azure-Portal auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Ihrer ZenDesk-App herstellen kann. Wenn die Verbindung nicht möglich ist, stellen Sie sicher, dass Ihr ZenDesk-Konto über Teamadministratorberechtigungen verfügt, und wiederholen Sie Schritt 5.

7. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen „Bei Fehler E-Mail-Benachrichtigung senden“.

8. Klicken Sie auf **Speichern**. 

9. Wählen Sie im Abschnitt „Zuordnungen“ die Option **Azure Active Directory-Benutzer mit ZenDesk synchronisieren**.

10. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit ZenDesk synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in ZenDesk für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche „Speichern“, um alle Änderungen zu übernehmen.

11. Um den Azure AD-Bereitstellungsdienst für ZenDesk zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

12. Klicken Sie auf **Speichern**. 

Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die ZenDesk im Abschnitt „Benutzer und Gruppen“ zugewiesen sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 20 Minuten erfolgen, solange der Dienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Bereitstellungsdienst ausgeführt werden.

Weitere Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](active-directory-saas-provisioning-reporting.md)

