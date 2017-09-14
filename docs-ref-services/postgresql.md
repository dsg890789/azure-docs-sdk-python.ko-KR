---
title: "Python용 Azure PostgreSQL 라이브러리"
description: 
keywords: "Azure, Python, SDK, API, SQL, 데이터베이스, Postgres, PostgreSQL"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: postgresql
ms.openlocfilehash: e184efc276fb4e6d86504ab44e47340ce72e7006
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/18/2017
---
#<a name="azure-postgresql-libraries-for-python"></a>Python용 Azure PostgreSQL 라이브러리

## <a name="overview"></a>개요
ODBC 드라이버 및 pyodbc를 사용하여 데이터베이스에 연결하고 SQL 문을 직접 실행합니다.

[Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/)에 대해 자세히 알아보세요.

## <a name="client-odbc-driver-and-pyodbc"></a>Client ODBC 드라이버 및 pyodbc
Azure Database for PostgreSQL에 액세스하는 데 권장되는 클라이언트 라이브러리는 Microsoft [ODBC 드라이버 및 pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)입니다.

### <a name="example"></a>예제 

Azure Database for PostgreSQL에 연결하고 `SALES` 테이블의 모든 레코드를 선택합니다. Azure Portal에서 데이터베이스에 대한 ODBC 연결 문자열을 가져올 수 있습니다.

```python
import pyodbc

SERVER = 'YOUR_SERVER_NAME.postgres.database.azure.com'
DATABASE = 'YOUR_DB_NAME'
USERNAME = 'YOUR_USERNAME'
PASSWORD = 'YOUR_PASSWORD'

DRIVER = '{PostgreSQL ODBC Driver}'
cnxn = pyodbc.connect(
    'DRIVER=' + DRIVER + ';PORT=5432;SERVER=' + SERVER +
    ';PORT=5432;DATABASE=' + DATABASE + ';UID=' + USERNAME +
    ';PWD=' + PASSWORD)
cursor = cnxn.cursor()
selectsql = "SELECT * FROM SALES" # SALES is an example table name
cursor.execute(selectsql)
```

## <a name="management-api"></a>관리 API
### <a name="requirements"></a>요구 사항
Python용 PostgreSQL 관리 라이브러리를 설치해야 합니다.
```bash
pip install azure-mgmt-rdbms
```

관리 클라이언트에서 인증할 자격 증명을 얻는 방법에 대한 자세한 내용은 [Python SDK 인증](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate) 페이지를 참조하세요.

### <a name="example"></a>예제
이 예제에서는 기존 Postgres 서버에 새 Postgres 데이터베이스를 만듭니다.
```python
from azure.mgtm.rdbms.postgresql import PostgreSQLManagementClient

SUBSCRIPTION_ID = "YOUR_AZURE_SUBSCRIPTION_ID"
RESOURCE_GROUP = "YOUR_AZURE_RESOURCE_GROUP_WITH_POSTGRES"
POSTGRES_SERVER = "YOUR_POSTGRES_SERVER_NAME"
DB_NAME = "YOUR_DESIRED_DATABASE_NAME"

client = PostgreSQLManagementClient(credentials, SUBSCRIPTION_ID)

db_creation_poller = client.databases.create_or_update(
    resource_group_name=RESOURCE_GROUP,
    server_name=POSTGRES_SERVER, database_name=DB_NAME)
db = db_creation_poller.result()
```

> [!div class="nextstepaction"]
> [관리 API 탐색](/python/api/overview/azure/postgresql/managementlibrary)
