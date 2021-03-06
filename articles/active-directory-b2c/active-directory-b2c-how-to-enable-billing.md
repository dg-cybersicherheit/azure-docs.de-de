---
title: "Verknüpfen eines Azure-Abonnements mit Azure AD B2C | Microsoft-Dokumentation"
description: "Enthält eine Schritt-für-Schritt-Anleitung zur Vorgehensweise, mit der die Abrechnung für einen Azure AD B2C-Mandanten in einem Azure-Abonnement ermöglicht wird."
services: active-directory-b2c
documentationcenter: dev-center-name
author: rojasja
manager: mbaldwin
ms.service: active-directory-b2c
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/05/2016
ms.author: joroja
ms.translationtype: Human Translation
ms.sourcegitcommit: d9ae8e8948d82b9695d7d144d458fe8180294084
ms.openlocfilehash: 5b9955b2af7f20a79981315fa33a0eb5380a5465
ms.contentlocale: de-de
ms.lasthandoff: 05/23/2017


---
# <a name="linking-an-azure-subscription-to-an-azure-b2c-tenant-to-pay-for-usage-charges"></a>Verknüpfen eines Azure-Abonnements mit einem Azure B2C-Mandanten zum Begleichen von Nutzungsgebühren

Laufende Nutzungsgebühren für Azure Active Directory B2C (oder Azure AD B2C) werden unter einem Azure-Abonnement abgerechnet. Für den Mandantenadministrator ist es erforderlich, den Azure AD B2C-Mandanten explizit mit einem Azure-Abonnement zu verknüpfen, nachdem der eigentliche B2C-Mandant erstellt wurde.  Diese Verknüpfung wird erreicht, indem im Azure-Zielabonnement eine Azure AD-Ressource vom Typ „B2C-Mandant“ erstellt wird. Viele B2C-Mandanten können zusammen mit anderen Azure-Ressourcen (z.B. VMs, Datenspeicher, LogicApps) mit einem einzelnen Azure-Abonnement verknüpft werden.


> [!IMPORTANT]
> Die neuesten Informationen zur Abrechnung von Nutzungsgebühren und zu den Preisen für B2C finden Sie auf der folgenden Seite: [Azure Active Directory B2C Preise](
https://azure.microsoft.com/pricing/details/active-directory-b2c/)

## <a name="step-1---create-an-azure-ad-b2c-tenant"></a>Schritt 1: Erstellen eines Azure AD B2C-Mandanten
Die Erstellung des B2C-Mandanten muss zuerst abgeschlossen werden. Überspringen Sie diesen Schritt, falls Sie den B2C-Zielmandanten bereits erstellt haben. [Erste Schritte mit Azure AD B2C](active-directory-b2c-get-started.md)

## <a name="step-2---open-azure-portal-in-the-azure-ad-tenant-that-shows-your-azure-subscription"></a>Schritt 2: Öffnen des Azure-Portals im Azure AD-Mandanten, für den Ihr Azure-Abonnement angezeigt wird
Navigieren Sie zum [Azure-Portal](https://portal.azure.com). Wechseln Sie zu dem Azure AD-Mandanten, für den das Azure-Abonnement angezeigt wird, das Sie verwenden möchten. Dieser Azure AD-Mandant unterscheidet sich vom B2C-Mandanten. Klicken Sie im Azure-Portal oben rechts im Dashboard auf den Kontonamen, um den Azure AD-Mandanten auszuwählen. Ein Azure-Abonnement ist erforderlich, um den Vorgang fortsetzen zu können. [Erhalten eines Azure-Abonnements](https://account.windowsazure.com/signup?showCatalog=True)

![Wechseln zum Azure AD-Mandanten](./media/active-directory-b2c-how-to-enable-billing/SelectAzureADTenant.png)

## <a name="step-3---create-a-b2c-tenant-resource-in-azure-marketplace"></a>Schritt 3: Erstellen einer Ressource vom Typ „B2C-Mandant“ in Azure Marketplace
Öffnen Sie den Marketplace per Klick auf das Marketplace-Symbol, oder wählen Sie oben links im Dashboard das grüne Pluszeichen.  Suchen Sie nach Azure Active Directory B2C, und wählen Sie die Anwendung aus. Klicken Sie auf Erstellen.

![Wählen Sie den Marketplace aus.](./media/active-directory-b2c-how-to-enable-billing/marketplace.png)

![Suchen nach AD B2C](./media/active-directory-b2c-how-to-enable-billing/searchb2c.png)

Das Dialogfeld für die Erstellung der Azure AD B2C-Ressource enthält die folgenden Parameter:

1. Azure AD B2C Tenant (Azure AD B2C-Mandant): Wählen Sie in der Dropdownliste einen Azure AD B2C-Mandanten aus.  Es werden nur zulässige Azure AD B2C-Mandanten angezeigt.  Zulässige B2C-Mandanten erfüllen die folgenden Bedingungen: Sie sind der globale Administrator des B2C-Mandanten, und der B2C-Mandant ist derzeit keinem Azure-Abonnement zugeordnet.

2. Azure AD B2C Resource name (Name der Azure AD B2C-Ressource): Ist bereits ausgewählt und entspricht dem Domänennamen des B2C-Mandanten.

3. Subscription (Abonnement): Ein aktives Azure-Abonnement, für das Sie als Administrator oder Co-Administrator fungieren.  Einem Azure-Abonnement können mehrere Azure AD B2C-Mandanten hinzugefügt werden

4. Resource Group and Resource Group location (Ressourcengruppe und Ressourcengruppenstandort): Dieses Artefakt dient Ihnen als Unterstützung beim Organisieren mehrerer Azure-Ressourcen.  Diese Auswahl hat keine Auswirkung auf den Standort, die Leistung oder den Abrechnungsstatus Ihres B2C-Mandanten.

5. Nutzen Sie die Option „An Dashboard anheften“, um leicht auf die Abrechnungsinformationen und Einstellungen für den B2C-Mandanten zugreifen zu können. ![Erstellen der B2C-Ressource](./media/active-directory-b2c-how-to-enable-billing/createresourceb2c.png)

## <a name="step-4---manage-your-b2c-tenant-resources-optional"></a>Schritt 4: Verwalten von Ressourcen des B2C-Mandanten (optional)
Nach Abschluss der Bereitstellung wird in der Zielressourcengruppe und im dazugehörigen Azure-Abonnement eine neue Ressource vom Typ „B2C-Mandant“ erstellt.  Zusätzlich zu den anderen Azure-Ressourcen wird dann eine neue Ressource vom Typ „B2C-Mandant“ angezeigt.

![Erstellen einer B2C-Ressource](./media/active-directory-b2c-how-to-enable-billing/b2cresourcedashboard.png)

Wenn Sie auf die B2C-Mandantenressource klicken, haben Sie folgende Möglichkeiten:
- Klicken Sie auf den Abonnementnamen, um die Abrechnungsinformationen zu überprüfen. Siehe „Abrechnung und Nutzung“.
- Klicken Sie auf „Azure AD B2C Settings“ (Azure AD B2C-Einstellungen), um direkt auf dem Blatt mit den Einstellungen für den B2C-Mandanten eine neue Browserregisterkarte zu öffnen.
- Senden Sie eine Supportanfrage.
- Verschieben Sie die B2C-Mandantenressource in ein anderes Azure-Abonnement oder in eine andere Ressourcengruppe.  Mit dieser Auswahl geben Sie an, welches Azure-Abonnement die Nutzungsgebühren erhält.

![Einstellungen für die B2C-Ressource](./media/active-directory-b2c-how-to-enable-billing/b2cresourcesettings.png)

## <a name="known-issues"></a>Bekannte Probleme
- Löschen eines B2C-Mandanten. Wenn ein B2C-Mandanten erstellt, gelöscht und mit demselben Domänennamen neu erstellt wird, löschen Sie auch die Verknüpfungsressource mit demselben Domänennamen, und erstellen Sie sie neu.  Diese Verknüpfungsressource finden Sie über das Azure-Portal im Abonnementmandanten unter „Alle Ressourcen“.
- Selbstauferlegte Einschränkungen für den Standort regionaler Ressourcen.  In seltenen Fällen hat ein Benutzer möglicherweise eine regionale Einschränkung für die Erstellung von Azure-Ressourcen eingerichtet.  Diese Einschränkung kann die Erstellung der Verknüpfung zwischen einem Azure-Abonnement und einem B2C-Mandanten verhindern. Um das Problem zu lösen, lockern Sie diese Einschränkung.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie diese Schritte für die einzelnen B2C-Mandanten ausgeführt haben, wird die Abrechnung für Ihr Azure-Abonnement in Übereinstimmung mit Ihren Azure Direct- bzw. Enterprise Agreement-Details durchgeführt.
- Überprüfen der Nutzung und Abrechnung im ausgewählten Azure-Abonnement
- Überprüfen von ausführlichen täglichen Nutzungsberichten mit der [Verwendungsberichts-API](active-directory-b2c-reference-usage-reporting-api.md)

