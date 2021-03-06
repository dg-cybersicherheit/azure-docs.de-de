---
title: "Konzeptioneller Überblick über benutzerdefinierte Domänennamen in Azure Active Directory | Microsoft-Dokumentation"
description: "Erläutert das Konzept der Verwendung von benutzerdefinierten Domänennamen in Azure Active Directory, einschließlich Verbundkonfiguration für einmaliges Anmelden."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: fd0c5def-0da2-43af-81bc-76f4cfe86afd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.translationtype: HT
ms.sourcegitcommit: 349fe8129b0f98b3ed43da5114b9d8882989c3b2
ms.openlocfilehash: 3c591680160101a91174868714392674c9aa7178
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---
# <a name="conceptual-overview-of-custom-domain-names-in-azure-active-directory"></a>Konzeptioneller Überblick über benutzerdefinierte Domänennamen in Azure Active Directory
Ein Domänenname kann ein wichtiger Bezeichner für viele Verzeichnisressourcen sein, z.B. als Teil folgender Elemente:

* Benutzername oder E-Mail-Adresse für einen Benutzer
* Adresse für einen Gruppe
* App-ID-URI für eine Anwendung

Eine Ressource in Azure Active Directory (Azure AD) kann einen Domänennamen enthalten, für den bereits geprüft wurde, ob er zu dem Verzeichnis gehört, das die Ressource enthält. Nur ein globaler Administrator kann Domänenverwaltungsaufgaben in Azure AD ausführen.

> [!IMPORTANT]
> Microsoft empfiehlt, für die Verwaltung von Azure AD anstelle des in diesem Artikel erwähnten klassischen Azure-Portals das [Azure AD Admin Center](https://aad.portal.azure.com) zu verwenden. Informationen zum Verwalten der Domänennamen im Azure AD Admin Center finden Sie unter [Verwalten von benutzerdefinierten Domänennamen in Azure Active Directory](active-directory-domains-manage-azure-portal.md).

Domänennamen in Azure AD sind global eindeutig. Ein benutzerdefinierter Domänenname kann nur von jeweils einem Azure AD-Mandanten verwendet werden. Wenn ein Azure AD-Verzeichnis einen Domänennamen überprüft hat, kann kein anderes Azure AD-Verzeichnis diesen Domänennamen überprüfen oder verwenden.

## <a name="initial-and-custom-domain-names"></a>Anfängliche und benutzerdefinierte Domänennamen
Jeder Domänenname in Azure AD ist entweder ein anfänglicher oder ein benutzerdefinierter Domänenname.

Jedes Azure AD-Verzeichnis enthält einen anfänglichen Domänennamen in der Form „contoso.onmicrosoft.com“. Die dritte Ebene des Domänennamens (in diesem Fall „contoso.onmicrosoft.com“) wurde bei der Erstellung des Verzeichnisses eingerichtet, üblicherweise durch den Administrator, der das Verzeichnis erstellt hat. Der anfängliche Domänenname für ein Verzeichnis kann nicht geändert oder gelöscht werden. Der anfängliche Domänenname ist zwar voll funktionsfähig, ist jedoch primär als Bootstrappingmechanismus gedacht, bis ein benutzerdefinierter Domänenname überprüft wurde.

In den meiden Produktionsumgebungen verfügt ein Verzeichnis über mindestens eine überprüfte benutzerdefinierte Domäne wie z. B. „contoso.com“. Diese benutzerdefinierte Domäne ist für die Endbenutzer sichtbar. Ein benutzerdefinierter Domänenname ist ein Domänenname im Besitz der Organisation (z.B. „contoso.com“), der von dieser Organisation beispielsweise zum Hosten ihrer Website verwendet wird. Dieser Domänenname ist den Mitarbeitern vertraut, da er Teil des Benutzernamens ist, mit dem sie sich beim Unternehmensnetzwerk anmelden oder den sie zum Senden und Empfangen von E-Mail verwenden.

Bevor der benutzerdefinierte Domänenname von Azure AD verwendet werden kann, muss er Ihrem Verzeichnis hinzugefügt sowie überprüft werden.

## <a name="verified-and-unverified-domain-names"></a>Überprüfte und nicht überprüfte Domänennamen
Der anfängliche Domänenname für ein Verzeichnis wird implizit als durch Azure AD überprüft ausgewertet. Wenn ein Administrator einen benutzerdefinierten Domänennamen zu einem Azure AD-Verzeichnis hinzufügt, befindet dieser sich zunächst in einem nicht überprüften Status. Azure AD lässt nicht zu, dass Verzeichnisressourcen einen nicht überprüften Domänennamen verwenden. Dadurch wird sichergestellt, dass nur ein einziges Verzeichnis einen bestimmten Domänennamen verwenden kann und dass der Organisation, die den Domänennamen verwendet, dieser Domänenname tatsächlich gehört.

Azure AD überprüft den Besitz eines Domänennamens durch Suchen nach einem bestimmten Eintrag in der DNS-Zonendatei (Domain Name Service) für den Domänennamen. Um den Besitz eines Domänennamens zu überprüfen, ruft ein Administrator den DNS-Eintrag aus Azure AD ab, nach dem Azure AD suchen wird, und fügt diesen Eintrag zur DNS-Zonendatei für den Domänennamen hinzu. Die DNS-Zonendatei wird von der Domänennamen-Registrierungsstelle für diese Domäne verwaltet. Die zum Überprüfen einer Domäne erforderlichen Schritte werden im Artikel zum [Hinzufügen einer benutzerdefinierten Domäne zu Ihrem Azure AD-Verzeichnis](active-directory-add-domain.md)beschrieben.

Das Hinzufügen eines DNS-Eintrags zur Zonendatei für den Domänenamen wirkt sich nicht auf andere Domänendienste wie z.B. E-Mail- oder Webhostingdienste aus.

## <a name="federated-and-managed-domain-names"></a>Verbund- und verwaltete Domänennamen
In Azure AD kann ein benutzerdefinierter Domänenname konfiguriert werden, um Benutzern die Verbundanmeldung beim lokalen Active Directory und Azure AD zu ermöglichen. Zum Konfigurieren einer Domäne für einen Verbund müssen sowohl privilegierte Ressourcen in Azure AD als auch Ihr Windows Server Active Directory aktualisiert werden. Die Konfiguration einer Verbunddomäne muss über Azure AD Connect oder mithilfe von PowerShell abgeschlossen werden. Das Hinzufügen einer benutzerdefinierten Domäne zu einem Verbund kann nicht über das klassische Azure-Portal initiiert werden. Sehen Sie sich das Video [Configuring AD FS for user sign-in with Azure AD Connect](http://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect) an, um mehr über das Konfigurieren der Active Directory-Verbunddienste für die Benutzeranmeldung über Azure AD Connect zu erfahren.

Domänen, bei denen es sich nicht um Verbunddomänen handelt, werden auch als verwaltete Domänen bezeichnet. Der anfängliche Domänenname für ein Azure AD-Verzeichnis wird implizit als verwaltete Domäne ausgewertet.

## <a name="primary-domain-names"></a>Primäre Domänennamen
Der primäre Domänenname für ein Verzeichnis ist der als Standardwert für den Benutzernamensbestandteil „Domäne“ vorab ausgewählte Domänenname. Dieser wird verwendet, wenn ein Administrator im [Azure-Portal](https://portal.azure.com/) oder in einem anderen Portal wie beispielsweise dem Office 365-Verwaltungsportal oder dem Microsoft Intune-Portal einen neuen Benutzer erstellt. Ein Verzeichnis kann nur über einen primären Domänennamen verfügen. Ein Administrator kann den primären Domänennamen in eine beliebige andere überprüfte benutzerdefinierte Domäne (keine Verbunddomäne) oder in die anfängliche Domäne ändern.

## <a name="domain-names-in-azure-ad-and-other-microsoft-online-services"></a>Domänennamen in Azure AD und anderen Microsoft Online Services
Ein Domänenname muss in Azure AD überprüft werden, bevor er von anderen Microsoft Online Services wie beispielsweise Exchange Online, SharePoint Online oder Intune verwendet werden kann. Für diese anderen Dienste ist es üblicherweise erforderlich, dass ein Administrator mindestens einen, für den jeweiligen Dienst spezifischen DNS-Eintrag hinzufügt.

Azure-Web-Apps verwenden eigene Mechanismen, um den Besitz einer Domäne zu überprüfen. Eine Domäne muss für die Verwendung mit einem Azure AD-Verzeichnis überprüft werden, selbst wenn sie zuvor zur Verwendung durch eine Azure-Web-App in einem auf diesem Azure AD-Verzeichnis basierenden Abonnement überprüft wurde. Eine Azure-Web-App kann einen Domänennamen verwenden, der in einem anderen Verzeichnis als dem Verzeichnis überprüft wurde, das die Web-App sichert.

## <a name="managing-domain-names"></a>Verwalten von Domänennamen
Aufgaben der Domänenverwaltung können über das klassische Azure-Portal oder mithilfe von PowerShell durchgeführt werden. Viele Aufgaben können in der Azure AD-Graph-API ausgeführt werden.

* [Hinzufügen und Überprüfen eines benutzerdefinierten Domänennamens](active-directory-add-domain.md)
* [Verwalten von Domänen im klassischen Azure-Portal](active-directory-add-manage-domain-names.md)
* [Verwenden von PowerShell zum Verwalten von Domänennamen in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Verwenden der Azure AD-Graph-API zum Verwalten von Domänennamen in Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)


