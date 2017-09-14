---
title: "Python용 Azure Key Vault 라이브러리"
description: "Python용 Azure Key Vault 라이브러리에 대한 참조 설명서"
author: lisawong19
keywords: "Azure, Python, SDK, API, 키, Key Vault, 인증, 비밀, 키, 보안"
manager: douge
ms.author: liwong
ms.date: 07/18/2017
ms.topic: article
ms.devlang: python
ms.service: keyvault
ms.openlocfilehash: 3eac46eb4d5d19273ead9f19b739f6fb6d72e5cc
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/18/2017
---
# <a name="azure-key-vault-libraries-for-python"></a>Python용 Azure Key Vault 라이브러리

## <a name="overview"></a>개요

클라이언트 라이브러리를 사용하여 Azure Key Vault에서 키와 비밀을 생성, 업데이트 및 삭제합니다.

Azure Key Vault 관리 라이브러리를 사용하여 키 자격 증명 모음을 만들고, 응용 프로그램에 권한을 부여하고, 권한을 관리합니다. 

[Azure Key Vault](/azure/key-vault/key-vault-whatis)에 대해 자세히 알아보세요.

## <a name="install-the-libraries"></a>라이브러리 설치

### <a name="client-library"></a>클라이언트 라이브러리
```bash
pip install azure-keyvault
```

## <a name="example"></a>예제
Key Vault에서 [JSON 웹 키](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18)를 검색합니다.

```python
from azure.keyvault import KeyVaultClient, KeyVaultAuthentication
from azure.common.credentials import ServicePrincipalCredentials

credentials = None

def auth_callack(server, resource, scope):
    credentials = credentials or ServicePrincipalCredentials(
        client_id = '', #client id
        secret = '',
        tenant = '',
        resource = resource
    )
    token = credentials.token
    return token['token_type'], token['access_token']

client = KeyVaultClient(KeyVaultAuthentication(auth_callack))

key_bundle = client.get_key(vault_url, key_name, key_version)
json_key = key_bundle.key
```
[!div class="nextstepaction"]
[클라이언트 API 탐색](/python/api/overview/azure/keyvault/clientlibrary)

### <a name="management-api"></a>관리 API
```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a>예제
다음 예제에서는 Azure Key Vault를 만드는 방법을 보여 줍니다. 

```python
from azure.mgmt.keyvault import KeyVaultManagementClient

GROUP_NAME = 'your_resource_group_name'
KV_NAME = 'your_key_vault_name'
#The object ID of the User or Application for access policies. Find this number in the portal
OBJECT_ID = '00000000-0000-0000-0000-000000000000'

kv_client = KeyVaultManagementClient(credentials, subscription_id)

vault = kv_client.vaults.create_or_update(
    GROUP_NAME,
    KV_NAME,
    {
        'location': 'eastus',
        'properties': {
            'sku': {
                'name': 'standard'
            },
            'tenant_id': os.environ['AZURE_TENANT_ID'],
            'access_policies': [{
                'tenant_id': os.environ['AZURE_TENANT_ID'],
                'object_id': OBJECT_ID,
                'permissions': {
                    'keys': ['all'],
                    'secrets': ['all']
                }
            }]
        }
    }
)
```
> [!div class="nextstepaction"]
> [관리 API 탐색](/python/api/azure.mgmt.keyvault)

> [!div class="nextstepaction"]
> [관리 API 탐색](/python/api/overview/azure/keyvault/managementlibrary)

## <a name="samples"></a>샘플
* [Key Vault 관리][1] 
* [Key Vault 복구(영문)][2]

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

Azure Key Vault 샘플의 [전체 목록](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault)을 봅니다. 