---
title: Azure Stack でのマルチテナント
description: Azure Stack で複数の Azure Active Directory ディレクトリをサポートする方法を説明します。
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/04/2019
ms.author: patricka
ms.reviewer: bryanr
ms.lastreviewed: 03/04/2019
ms.openlocfilehash: 16d915ff6ce0f787febbdc4be4d41e1c2e714d7f
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2019
ms.locfileid: "57336667"
---
# <a name="multi-tenancy-in-azure-stack"></a>Azure Stack でのマルチテナント

*適用対象:Azure Stack 統合システムと Azure Stack Development Kit*

複数の Azure Active Directory (Azure AD) テナントのユーザーが Azure Stack のサービスを使用できるように Azure Stack を構成することができます。 ここでは、次のシナリオを例に説明します。

- あなたは contoso.onmicrosoft.com のサービス管理者です。ここに Azure Stack がインストールされています。
- メアリーは、ゲスト ユーザーがいる fabrikam.onmicrosoft.com のディレクトリ管理者です。
- メアリーの会社は、あなたの会社から IaaS および PaaS サービスを受けており、ゲスト ディレクトリ (fabrikam.onmicrosoft.com) のユーザーが、contoso.onmicrosoft.com にある Azure Stack のリソースにサインインして使用できるようにする必要があります。

このガイドでは、このシナリオに基づいて、Azure Stack でマルチテナントを構成するために必要な手順を説明します。 このシナリオでは、Fabrikam のユーザーが Contoso の Azure Stack デプロイのサービスにサインインして使用できるようにするための手順を、あなたとメアリーがそれぞれ完了する必要があります。  

## <a name="enable-multi-tenancy"></a>マルチテナントの有効化

Azure Stack でマルチテナントを構成する前に、いくつかの前提条件を考慮する必要があります。
  
 - Azure Stack がインストールされているディレクトリ (Contoso) とゲスト ディレクトリ (Fabrikam) の両方に対して、あなたとメアリーが連携して管理上の手順を実行する必要があります。  
 - Azure Stack 用 PowerShell の[インストール](azure-stack-powershell-install.md)と[構成](azure-stack-powershell-configure-admin.md)が済んでいることを確認してください。
 - [Azure Stack Tools をダウンロード](azure-stack-powershell-download.md)して、Connect モジュールと Identity モジュールをインポートします。

    ```PowerShell  
    Import-Module .\Connect\AzureStack.Connect.psm1
    Import-Module .\Identity\AzureStack.Identity.psm1
    ```

### <a name="configure-azure-stack-directory"></a>Azure Stack ディレクトリの構成

このセクションでは、Fabrikam の Azure AD ディレクトリのテナントからのサインインを許可するように Azure Stack を構成します。

ゲスト ディレクトリ テナントのユーザーとサービス プリンシパルを受け入れるように Azure Resource Manager を構成して、ゲスト ディレクトリ テナント (Fabrikam) を Azure Stack にオンボードします。

Contoso.onmicrosoft.com のサービス管理者は、次のコマンドを実行します。

```PowerShell  
## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
$adminARMEndpoint = "https://adminmanagement.local.azurestack.external"

## Replace the value below with the Azure Stack directory
$azureStackDirectoryTenant = "contoso.onmicrosoft.com"

## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantToBeOnboarded = "fabrikam.onmicrosoft.com"

## Replace the value below with the name of the resource group in which the directory tenant registration resource should be created (resource group must already exist).
$ResourceGroupName = "system.local"

## Replace the value below with the region location of the resource group. 
$location = "local"

Register-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
 -DirectoryTenantName $azureStackDirectoryTenant `
 -GuestDirectoryTenantName $guestDirectoryTenantToBeOnboarded `
 -Location $location `
 -ResourceGroupName $ResourceGroupName
```

### <a name="configure-guest-directory"></a>ゲスト ディレクトリの構成

Azure Stack 管理者/オペレーターが Azure Stack で使用される Fabrikam ディレクトリを有効にしたら、Mary は Azure Stack を Fabrikam のディレクトリ テナントに登録する必要があります。

#### <a name="registering-azure-stack-with-the-guest-directory"></a>ゲスト ディレクトリへの Azure Stack の登録

Fabrikam のディレクトリ管理者である Mary は、ゲスト ディレクトリ fabrikam.onmicrosoft.com で次のコマンドを実行します。

```PowerShell
## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
$tenantARMEndpoint = "https://management.local.azurestack.external"
    
## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantName = "fabrikam.onmicrosoft.com"

Register-AzSWithMyDirectoryTenant `
 -TenantResourceManagerEndpoint $tenantARMEndpoint `
 -DirectoryTenantName $guestDirectoryTenantName `
 -Verbose 
```

> [!IMPORTANT]
> Azure Stack 管理者が今後新しいサービスや更新プログラムをインストールした場合には、このスクリプトをもう一度実行する必要があります。
>
> このスクリプトは、ディレクトリ内の Azure Stack アプリケーションの状態を確認するためにいつでも実行できます。
>
> VM をマネージド ディスク内に作成する操作 (1808 更新プログラムで導入) で問題が起きた場合は新しい**ディスク リソース プロバイダー**が追加されており、このスクリプトをもう一度実行する必要があります。

### <a name="direct-users-to-sign-in"></a>ユーザーをサインインに誘導する

あなたとメアリーの両方が Fabrikam ディレクトリのオンボード手順を完了したので、メアリーは Fabrikam のユーザーをサインインに誘導することができます。  Fabrikam のユーザー (つまり、fabrikam.onmicrosoft.com のサフィックスを持つユーザー) は、 https://portal.local.azurestack.external にアクセスしてサインインします。  

メアリーは、Fabrikam ディレクトリの[外部プリンシパル](../role-based-access-control/rbac-and-directory-admin-roles.md) (つまり、fabrikam.onmicrosoft.com のサフィックスを持たない Fabrikam ディレクトリのユーザー) はすべて、 https://portal.local.azurestack.external/fabrikam.onmicrosoft.com を使用してサインインするよう誘導します。  この URL を使用しない場合、これらのユーザーは自分の既定のディレクトリ (Fabrikam) に送信され、管理者が同意していないことを示すエラーを受信します。

## <a name="disable-multi-tenancy"></a>マルチテナントの無効化

Azure Stack 内に複数のテナントを持つ必要がなくなった場合は、次の手順を実行してマルチ テナントを無効にできます。

1. ゲスト ディレクトリの管理者 (このシナリオでは Mary) として、*Unregister-AzsWithMyDirectoryTenant* を実行します。 このコマンドレットは、新しいディレクトリからすべての Azure Stack アプリケーションをアンインストールします。

    ``` PowerShell
    ## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
    $tenantARMEndpoint = "https://management.local.azurestack.external"
        
    ## Replace the value below with the guest tenant directory. 
    $guestDirectoryTenantName = "fabrikam.onmicrosoft.com"
    
    Unregister-AzsWithMyDirectoryTenant `
     -TenantResourceManagerEndpoint $tenantARMEndpoint `
     -DirectoryTenantName $guestDirectoryTenantName `
     -Verbose 
    ```

2. Azure Stack のサービス管理者 (このシナリオではあなた) として、*Unregister-AzSGuestDirectoryTenant* を実行します。 

    ``` PowerShell  
    ## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
    $adminARMEndpoint = "https://adminmanagement.local.azurestack.external"
    
    ## Replace the value below with the Azure Stack directory
    $azureStackDirectoryTenant = "contoso.onmicrosoft.com"
    
    ## Replace the value below with the guest tenant directory. 
    $guestDirectoryTenantToBeDecommissioned = "fabrikam.onmicrosoft.com"
    
    ## Replace the value below with the name of the resource group in which the directory tenant registration resource should be created (resource group must already exist).
    $ResourceGroupName = "system.local"
    
    Unregister-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
     -DirectoryTenantName $azureStackDirectoryTenant `
     -GuestDirectoryTenantName $guestDirectoryTenantToBeDecommissioned `
     -ResourceGroupName $ResourceGroupName
    ```

    > [!WARNING]
    > マルチ テナントを無効にする手順は、正しい順序で実行する必要があります。 手順 2 を先に実行した場合、手順 1 は失敗します。

## <a name="next-steps"></a>次の手順

- [委任されたプロバイダーの管理](azure-stack-delegated-provider.md)
- [Azure Stack の主要概念](azure-stack-key-features.md)
- [クラウド サービス プロバイダーとして Azure Stack の使用状況と課金を管理する](azure-stack-add-manage-billing-as-a-csp.md)
- [Azure Stack に使用量と課金のためのテナントを追加する](azure-stack-csp-howto-register-tenants.md)
