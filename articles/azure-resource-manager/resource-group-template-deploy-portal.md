---
title: Verwenden des Azure-Portals zum Bereitstellen von Azure-Ressourcen | Microsoft Docs
description: Verwenden Sie das Azure-Portal und Azure Resource Manager zum Bereitstellen Ihrer Ressourcen.
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 2c98a4aa-8d9f-4a0a-b764-214dbe8ed009
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: a4cac5834c667976b0a2d1f2748e4309a166d16a
ms.contentlocale: de-de
ms.lasthandoff: 08/11/2017

---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-Portal
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-template-deploy.md)
> * [Azure-Befehlszeilenschnittstelle](resource-group-template-deploy-cli.md)
> * [Portal](resource-group-template-deploy-portal.md)
> * [REST-API](resource-group-template-deploy-rest.md)
> 
> 

In diesem Thema wird veranschaulicht, wie Sie das [Azure-Portal](https://portal.azure.com) mit [Azure Resource Manager](resource-group-overview.md) verwenden, um Ihre Azure-Ressourcen bereitzustellen. Informationen zum Verwalten von Ressourcen finden Sie unter [Verwalten von Azure-Ressourcen über das Portal](resource-group-portal.md).

Das Portal und der Ressourcen-Manager werden derzeit nicht von allen Diensten unterstützt. Für diese Dienste müssen Sie das [klassische Portal](https://manage.windowsazure.com)verwenden. Den Status der einzelnen Dienste können Sie dem [Verfügbarkeitsdiagramm für das Azure-Portal](https://azure.microsoft.com/features/azure-portal/availability/)entnehmen.

## <a name="create-resource-group"></a>Ressourcengruppe erstellen
1. Wählen Sie zum Erstellen einer leeren Ressourcengruppe **Neu** > **Verwaltung** > **Ressourcengruppe** aus.
   
    ![Leere Ressourcengruppe erstellen](./media/resource-group-template-deploy-portal/create-empty-group.png)
2. Geben Sie einen Namen und einen Speicherort an, und wählen Sie ggf. ein Abonnement aus. Sie müssen einen Standort für die Ressourcengruppe angeben, da diese Metadaten zu den Ressourcen speichert. Aus Compliance-Gründen sollten Sie angeben, wo diese Metadaten gespeichert werden. Im Allgemeinen wird die Angabe eines Standorts empfohlen, an dem sich der Großteil Ihrer Ressourcen befindet. Durch die Verwendung des gleichen Standorts können Sie die Vorlage vereinfachen.
   
    ![Gruppenwerte festlegen](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a>Bereitstellen von Ressourcen über den Marketplace
Nachdem Sie eine Ressourcengruppe erstellt haben, können Sie Ressourcen dafür über den Marketplace bereitstellen. Der Marketplace bietet vordefinierte Lösungen für gängige Szenarien.

1. Wählen Sie zum Starten einer Bereitstellung **Neu** aus, und geben Sie den Ressourcentyp an, den Sie bereitstellen möchten. Suchen Sie anschließend die bestimmte Version der Ressource, die Sie bereitstellen möchten.
   
    ![Ressource bereitstellen](./media/resource-group-template-deploy-portal/deploy-resource.png)
2. Falls die gewünschte bereitzustellende Lösung nicht angezeigt wird, können Sie im Marketplace danach suchen.
   
    ![Marketplace durchsuchen](./media/resource-group-template-deploy-portal/search-resource.png)
3. Je nach ausgewähltem Ressourcentyp müssen Sie einige relevante Eigenschaften festlegen, bevor die Bereitstellung beginnen kann. Diese Optionen werden hier nicht angezeigt, da sie je nach Ressourcentyp variieren. Für alle Typen müssen Sie eine Zielressourcengruppe auswählen. Die folgende Abbildung zeigt, wie Sie eine Web-App erstellen und in der erstellten Ressourcengruppe bereitstellen.
   
    ![Erstellen einer Ressourcengruppe](./media/resource-group-template-deploy-portal/select-existing-group.png)
   
    Alternativ können Sie eine Ressourcengruppe beim Bereitstellen Ihrer Ressourcen erstellen. Wählen Sie **Neu erstellen** aus, und benennen Sie die Ressourcengruppe.
   
    ![neue Ressourcengruppe erstellen](./media/resource-group-template-deploy-portal/select-new-group.png)
4. Die Bereitstellung wird gestartet. Der Vorgang kann mehrere Minuten dauern. Nachdem die Bereitstellung abgeschlossen wurde, wird eine Benachrichtigung angezeigt.
   
    ![Benachrichtigung anzeigen](./media/resource-group-template-deploy-portal/view-notification.png)
5. Nach dem Bereitstellen Ihrer Ressourcen können Sie mit dem Befehl **Hinzufügen** auf dem Blatt „Ressourcengruppe“ einer Ressourcengruppe weitere Ressourcen hinzufügen.
   
    ![Ressource hinzufügen](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Bereitstellen von Ressourcen mithilfe einer benutzerdefinierten Vorlage
Wenn Sie eine Bereitstellung ausführen möchten, ohne eine der Vorlagen im Marketplace zu nutzen, können Sie eine angepasste Vorlage erstellen, mit der die Infrastruktur für Ihre Lösung definiert wird. Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](resource-group-authoring-templates.md).

1. Wählen Sie zum Bereitstellen einer benutzerdefinierten Vorlage über das Portal die Option **Neu** aus, suchen Sie nach **Vorlagenbereitstellung**, und wählen Sie diese Option aus.
   
    ![Vorlagenbereitstellung suchen](./media/resource-group-template-deploy-portal/search-template.png)
2. Wählen Sie in den verfügbaren Ressourcen **Vorlagenbereitstellung** aus.
   
    ![Vorlagenbereitstellung auswählen](./media/resource-group-template-deploy-portal/select-template.png)
3. Öffnen Sie nach dem Starten der Vorlagenbereitstellung die leere Vorlage, die zum Anpassen verfügbar ist.
   
    ![Vorlage erstellen](./media/resource-group-template-deploy-portal/show-custom-template.png)
   
    Fügen Sie im Editor die JSON-Syntax hinzu, die die Ressourcen definiert, die Sie bereitstellen möchten. Wenn Sie abschließend **Speichern** aus. Einen Leitfaden zum Schreiben der JSON-Syntax finden Sie unter [Resource Manager-Vorlage – Exemplarische Vorgehensweise](resource-manager-template-walkthrough.md).
   
    ![Bearbeiten der Vorlage](./media/resource-group-template-deploy-portal/edit-template.png)
4. Sie können auch in den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/)eine bereits vorhandene Vorlage auswählen. Diese Vorlagen sind ein Beitrag der Community. Sie decken viele häufig vorkommende Szenarien ab, und unter Umständen wurde bereits eine Vorlage für einen Fall hinzugefügt, der Ihrer Bereitstellung ähnelt. Sie können die Vorlagen nach Übereinstimmungen mit Ihrem Szenario durchsuchen.
   
    ![Schnellstartvorlage auswählen](./media/resource-group-template-deploy-portal/select-quickstart-template.png)
   
    Sie können die ausgewählte Vorlage im Editor anzeigen.
5. Nachdem Sie alle anderen Werte angegeben haben, wählen Sie **Erstellen** aus, um die Vorlage bereitzustellen. 
   
    ![Bereitstellen der Vorlage](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a>Bereitstellen von Ressourcen aus einer in Ihrem Konto gespeicherten Vorlage
Über das Portal können Sie eine Vorlage in Ihrem Azure-Konto speichern und später erneut bereitstellen. Weitere Informationen zur Verwendung dieser gespeicherten Vorlagen finden Sie unter [Erste Schritte mit privaten Vorlagen im Azure-Portal](../marketplace-consumer/mytemplates-getstarted.md).

1. Wählen Sie **Durchsuchen** > **Vorlagen**, um Ihre gespeicherten Vorlagen zu finden.
   
    ![Vorlagen durchsuchen](./media/resource-group-template-deploy-portal/browse-templates.png)
2. Wählen Sie in der Liste der Vorlagen, die in Ihrem Konto gespeichert sind, diejenige aus, mit der Sie arbeiten möchten.
   
    ![Gespeicherte Vorlagen](./media/resource-group-template-deploy-portal/saved-templates.png)
3. Wählen Sie **Bereitstellen** aus, um diese gespeicherte Vorlage erneut bereitzustellen.
   
    ![Bereitstellen der gespeicherten Vorlage](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Anzeigen von Überwachungsprotokollen finden Sie unter [Überwachen von Vorgängen mit Resource Manager](resource-group-audit.md).
* Informationen zum Beheben von Bereitstellungsfehlern finden Sie unter [Anzeigen von Bereitstellungsvorgängen](resource-manager-deployment-operations.md).
* Informationen zum Abrufen einer Vorlage aus einer Bereitstellung oder Ressourcengruppe finden Sie unter [Exportieren einer Azure Resource Manager-Vorlage aus vorhandenen Ressourcen](resource-manager-export-template.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).


