<properties
	pageTitle="Gerenciar o controle de acesso com base em função com a API REST"
	description="Gerenciar o controle de acesso com base em função com a API REST"
	services="active-directory"
	documentationCenter="na"
	authors="kgremban"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="multiple"
	ms.tgt_pltfrm="rest-api"
	ms.devlang="na"
	ms.topic="article"
	ms.date="01/25/2016"
	ms.author="kgremban"/>

# Gerenciar o controle de acesso com base em função com a API REST

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST API](role-based-access-control-manage-access-rest.md)

## Lista todas as atribuições de função

Lista todas as atribuições de função no escopo e sub-escopos especificados.

Para listar as atribuições de função, é necessário ter acesso à operação `Microsoft.Authorization/roleAssignments/read` no escopo. Todas as funções internas recebem acesso a essa operação. Para saber mais sobre as atribuições de função e o gerenciamento de acesso para recursos do Azure, veja [Controle de Acesso baseado em função do Azure](role-based-access-control-configure.md).

### Solicitação

Use o método **GET** com o seguinte URI:

	https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&filter={filter}

Dentro do URI, faça as seguintes substituições para personalizar sua solicitação:

Substitua *{scope}* pelo escopo para o qual deseja listar as atribuições de função. Os exemplos a seguir mostram como especificar o escopo para diferentes níveis:

| Nível | *{Scope}* |
|-------|-----------|
| Assinatura | /subscriptions/{subscription-id} |
| Grupo de recursos | /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1 |
| Recurso | /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1 |

Substitua *{api-version}* por 2015-07-01.

Substitua *{filter}* pela condição que deseja aplicar a fim de filtrar a lista de atribuições de função. As seguintes condições são suportadas.


| Condição | *{Filter}* | Substitua |
|-----------|------------|---------|
| Para atribuições de função da lista para apenas o escopo especificado, não incluindo as atribuições de função em sub-escopos. | `atScope()` | |
| Para listar atribuições de função somente para um usuário, grupo ou aplicativo específico | `principalId%20eq%20'{objectId}'` | Substitua *{objectId}* pela objectId do AD do Azure do usuário, do grupo ou da entidade de serviço. Por exemplo, `&filter=principalId%20eq%20'3a477f6a-6739-4b93-84aa-3be3f8c8e7c2'` |
| Para listar atribuições de função somente para um usuário específico incluindo aqueles atribuídas a grupos dos quais o usuário é membro | `assignedTo('{objectId}')` | Substitua *{objectId}* pelo objectId do AD do Azure do usuário. Por exemplo, `&filter=assignedTo('3a477f6a-6739-4b93-84aa-3be3f8c8e7c2')` |



### Resposta

Código de status: 200

```
{
  "value": [
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "principalId": "2f9d4375-cbf1-48e8-83c9-2a0be4cb33fb",
        "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
        "createdOn": "2015-10-08T07:28:24.3905077Z",
        "updatedOn": "2015-10-08T07:28:24.3905077Z",
        "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
        "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/baa6e199-ad19-4667-b768-623fde31aedd",
      "type": "Microsoft.Authorization/roleAssignments",
      "name": "baa6e199-ad19-4667-b768-623fde31aedd"
    }
  ],
  "nextLink": null
}

```

## Obter informações sobre uma atribuição de função

Obtém informações sobre uma atribuição de função única especificada pelo identificador de atribuição de função.

Para saber mais sobre uma atribuição de função, você deve ter acesso à operação `Microsoft.Authorization/roleAssignments/read`. Todas as funções internas recebem acesso a essa operação. Para saber mais sobre as atribuições de função e o gerenciamento de acesso para recursos do Azure, veja [Controle de Acesso baseado em função do Azure](role-based-access-control-configure.md).

### Solicitação

Use o método **GET** com o seguinte URI:

	https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Dentro do URI, faça as seguintes substituições para personalizar sua solicitação:

Substitua *{scope}* pelo escopo para o qual deseja listar as atribuições de função. Os exemplos a seguir mostram como especificar o escopo para diferentes níveis:

| Nível | *{Scope}* |
|-------|-----------|
| Assinatura | /subscriptions/{subscription-id} |
| Grupo de recursos | /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1 |
| Recurso | /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1 |

Substitua *{role-assignment-id}* pelo identificador GUID da atribuição de função.

Substitua *{api-version}* por 2015-07-01.

### Resposta

Código de status: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "principalId": "672f1afa-526a-4ef6-819c-975c7cd79022",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "createdOn": "2015-10-05T08:36:26.4014813Z",
    "updatedOn": "2015-10-05T08:36:26.4014813Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/196965ae-6088-4121-a92a-f1e33fdcc73e",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "196965ae-6088-4121-a92a-f1e33fdcc73e"
}

```

## Criar uma atribuição de função

Crie uma atribuição de função no escopo especificado para a entidade especificada, concedendo a função especificada.

Para criar uma atribuição de função, você deve ter acesso à operação `Microsoft.Authorization/roleAssignments/write`. Das funções internas, somente *Proprietário* e *Administrador do Acesso de Usuário* recebem permissão para acessar essa operação. Para saber mais sobre as atribuições de função e o gerenciamento de acesso para recursos do Azure, veja [Controle de acesso baseado em função do Azure](role-based-access-control-configure.md)

### Solicitação

Use o método **PUT** com o seguinte URI:

	https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Dentro do URI, faça as seguintes substituições para personalizar sua solicitação:

Substitua *{scope}* pelo escopo para o qual deseja criar as atribuições de função. Quando você cria uma atribuição de função em um escopo pai, todos os escopos filho herdam a mesma atribuição de função. Os exemplos a seguir mostram como especificar o escopo para diferentes níveis:

| Nível | *{Scope}* |
|-------|-----------|
| Assinatura | /subscriptions/{subscription-id} |
| Grupo de recursos | /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1 |
| Recurso | /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1 |

Substitua *{role-assignment-id}* por um novo GUID. Ele será usado como o identificador GUID da nova atribuição de função.

Substitua *{api-version}* por 2015-07-01.

Para o corpo da solicitação, forneça os valores no seguinte formato:

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| Nome do elemento | Obrigatório | Tipo | Descrição |
|------------------|----------|--------|-------------|
| roleDefinitionId | Sim | Cadeia de caracteres | O identificador da função que deve ser atribuído. O formato do identificador é: `{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}` |
| principalId | Sim | Cadeia de caracteres | ID de objeto da entidade do AD do Azure (usuário, grupo ou entidade de serviço) para o qual a função deve ser atribuída. |

### Resposta

Código de status: 201

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-16T00:27:19.6447515Z",
    "updatedOn": "2015-12-16T00:27:19.6447515Z",
    "createdBy": null,
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/2e9e86c8-0e91-4958-b21f-20f51f27bab2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "2e9e86c8-0e91-4958-b21f-20f51f27bab2"
}

```

## Excluir uma atribuição de função

Exclua uma atribuição de função no escopo especificado.

Para excluir uma atribuição de função, você deve ter acesso à operação `Microsoft.Authorization/roleAssignments/delete`. Das funções internas, somente *Proprietário* e *Administrador do Acesso de Usuário* recebem permissão para acessar essa operação. Para saber mais sobre as atribuições de função e o gerenciamento de acesso para recursos do Azure, veja [Controle de acesso baseado em função do Azure](role-based-access-control-configure.md)

### Solicitação

Use o método **DELETE** com o seguinte URI:

	https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Dentro do URI, faça as seguintes substituições para personalizar sua solicitação:

Substitua *{scope}* pelo escopo para o qual deseja criar as atribuições de função. Os exemplos a seguir mostram como especificar o escopo para diferentes níveis:

| Nível | *{Scope}* |
|-------|-----------|
| Assinatura | /subscriptions/{subscription-id} |
| Grupo de recursos | /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1 |
| Recurso | /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1 |

Substitua *{role-assignment-id}* pelo GUID da id da atribuição de função.

Substitua *{api-version}* por 2015-07-01.

### Resposta

Código de status: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-17T23:21:40.8921564Z",
    "updatedOn": "2015-12-17T23:21:40.8921564Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/5eec22ee-ea5c-431e-8f41-82c560706fd2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "5eec22ee-ea5c-431e-8f41-82c560706fd2"
}

```

## Lista todas as Funções

Lista todas as funções que estão disponíveis para atribuição no escopo especificado.

Para listar as funções, é necessário ter acesso à operação `Microsoft.Authorization/roleDefinitions/read` no escopo. Todas as funções internas recebem acesso a essa operação. Para saber mais sobre as atribuições de função e o gerenciamento de acesso para recursos do Azure, veja [Controle de Acesso baseado em função do Azure](role-based-access-control-configure.md).

### Solicitação

Use o método **GET** com o seguinte URI:

	https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&filter={filter}

Dentro do URI, faça as seguintes substituições para personalizar sua solicitação:

Substitua *{scope}* pelo escopo para o qual deseja listar as funções. Os exemplos a seguir mostram como especificar o escopo para diferentes níveis:

| Nível | *{Scope}* |
|-------|-----------|
| Assinatura | /subscriptions/{subscription-id} |
| Grupo de recursos | /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1 |
| Recurso | /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1 |

Substitua *{api-version}* por 2015-07-01.

Substitua *{filter}* pela condição que você deseja aplicar a fim de filtrar a lista de funções. As seguintes condições são suportadas.

| Condição | *{Filter}* | Substitua |
|-----------|------------|---------|
| Para listar funções disponíveis para atribuição no escopo especificado e qualquer de seus escopos filhos. | `atScopeAndBelow()` | |
| Para procurar uma função usando o nome de exibição exato. | `roleName%20eq%20'{role-display-name}'` | Substitua *{role-display-name}* pela forma codificada da URL do nome de exibição exato da função. Por exemplo, `$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |



### Resposta

Código de status: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## Obter informações sobre uma Função

Obtém informações sobre uma função única especificada pelo identificador de definição da função. Para obter informações sobre uma única função usando seu nome de exibição, consulte Lista todas as funções e o filtro roleName.

Para saber mais sobre uma função, você deve ter acesso à operação `Microsoft.Authorization/roleDefinitions/read`. Todas as funções internas recebem acesso a essa operação. Para saber mais sobre as atribuições de função e o gerenciamento de acesso para recursos do Azure, veja [Controle de Acesso baseado em função do Azure](role-based-access-control-configure.md).

### Solicitação

Use o método **GET** com o seguinte URI:

	https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Dentro do URI, faça as seguintes substituições para personalizar sua solicitação:

Substitua *{scope}* pelo escopo para o qual deseja listar as atribuições de função. Os exemplos a seguir mostram como especificar o escopo para diferentes níveis:

| Nível | *{Scope}* |
|-------|-----------|
| Assinatura | /subscriptions/{subscription-id} |
| Grupo de recursos | /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1 |
| Recurso | /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1 |

Substitua *{role-definition-id}* pelo identificador GUID de definição da função. Substitua *{api-version}* por 2015-07-01.

### Resposta

Código de status: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## Criar uma Função personalizada
Criar uma função personalizada.

Para criar uma função personalizada, você deve ter acesso à operação `Microsoft.Authorization/roleDefinitions/write` em todos os seus `AssignableScopes`. Das funções internas, somente *Proprietário* e *Administrador do Acesso de Usuário* recebem permissão para acessar essa operação. Para saber mais sobre as atribuições de função e o gerenciamento de acesso para recursos do Azure, veja [Controle de acesso baseado em função do Azure](role-based-access-control-configure.md).

### Solicitação

Use o método **PUT** com o seguinte URI:

	https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Dentro do URI, faça as seguintes substituições para personalizar sua solicitação:

Substitua *{scope}* pela primeira *AssignableScope* da função personalizada. Os exemplos a seguir mostram como especificar o escopo para diferentes níveis.

| Nível | *{Scope}* |
|-------|-----------|
| Assinatura | /subscriptions/{subscription-id} |
| Grupo de recursos | /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1 |
| Recurso | /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1 |

Substitua *{role-definition-id}* por um novo GUID. Ele será usado como o identificador GUID da nova função personalizada.

Substitua *{api-version}* por 2015-07-01.

Para o corpo da solicitação, forneça os valores no seguinte formato:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Nome do elemento | Obrigatório | Tipo | Descrição |
|--------------|----------|------|-------------|
| name | Sim | Cadeia de caracteres | Identificador GUID da função personalizada. |
| properties.roleName | Sim | Cadeia de caracteres | Nome de exibição da função personalizada. Tamanho máximo de 128 caracteres. |
| properties.description | Não | Cadeia de caracteres | Descrição da função personalizada. Tamanho máximo de 1024 caracteres. |
| properties.type | Sim | Cadeia de caracteres | Defina como "CustomRole". |
| properties.permissions.actions | Sim | Cadeia de caracteres | Uma matriz de cadeias de caracteres de ação especificando as operações para as quais a função personalizada concede acesso. |
| properties.permissions.notActions | Não | Cadeia de caracteres | Uma matriz de cadeias de caracteres de ação especificando as operações que devem ser excluídas das operações para as quais a função personalizada concede acesso. |
| properties.assignableScopes | Sim | Cadeia de caracteres | Uma matriz de escopos no qual a função personalizada pode ser usada para gerenciamento de acesso. |

### Resposta

Código de status: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## Atualizar uma Função personalizada

Modificar uma função personalizada.

Para modificar uma função personalizada, você deve ter acesso à operação `Microsoft.Authorization/roleDefinitions/write` em todos os seus `AssignableScopes`. Das funções internas, somente *Proprietário* e *Administrador do Acesso de Usuário* recebem permissão para acessar essa operação. Para saber mais sobre as atribuições de função e o gerenciamento de acesso para recursos do Azure, veja [Controle de acesso baseado em função do Azure](role-based-access-control-configure.md).

### Solicitação

Use o método **PUT** com o seguinte URI:

	https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Dentro do URI, faça as seguintes substituições para personalizar sua solicitação:

Substitua *{scope}* pela primeira *AssignableScope* da função personalizada. Os exemplos a seguir mostram como especificar o escopo para diferentes níveis:

| Nível | *{Scope}* |
|-------|-----------|
| Assinatura | /subscriptions/{subscription-id} |
| Grupo de recursos | /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1 |
| Recurso | /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1 |

Substitua *{role-definition-id}* pelo identificador GUID da função personalizada a ser atualizada.

Substitua *{api-version}* por 2015-07-01.

Para o corpo da solicitação, forneça os valores no seguinte formato:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Nome do elemento | Obrigatório | Tipo | Descrição |
|--------------|----------|------|-------------|
| name | Sim | Cadeia de caracteres | Identificador GUID da função personalizada a ser atualizada. |
| properties.roleName | Sim | Cadeia de caracteres | Nome de exibição da função personalizada a ser atualizada. |
| properties.description | Não | Cadeia de caracteres | Nome de exibição da função personalizada atualizada. |
| properties.type | Sim | Cadeia de caracteres | Defina como "CustomRole". |
| properties.permissions.actions | Sim | Cadeia de caracteres | Uma matriz de cadeias de caracteres de ação especificando as operações para as quais a função personalizada atualizada concede acesso. |
| properties.permissions.notActions | Não | Cadeia de caracteres | Uma matriz de cadeias de caracteres de ação especificando as operações que devem ser excluídas das operações para as quais a função personalizada atualizada concede acesso. |
| properties.assignableScopes | Sim | Cadeia de caracteres | Uma matriz de escopos na qual a função personalizada atualizada pode ser usada para gerenciamento de acesso. |

### Resposta

Código de status: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## Excluir uma Função personalizada

Excluir uma função personalizada.

Para excluir uma função personalizada, você deve ter acesso à operação `Microsoft.Authorization/roleDefinitions/delete` em todos os seus `AssignableScopes`. Das funções internas, somente *Proprietário* e *Administrador do Acesso de Usuário* recebem permissão para acessar essa operação. Para saber mais sobre as atribuições de função e o gerenciamento de acesso para recursos do Azure, veja [Controle de acesso baseado em função do Azure](role-based-access-control-configure.md).

### Solicitação

Use o método **DELETE** com o seguinte URI:

	https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Dentro do URI, faça as seguintes substituições para personalizar sua solicitação:

Substitua *{scope}* pelo escopo para o qual deseja excluir as atribuições de função. Os exemplos a seguir mostram como especificar o escopo para diferentes níveis:

| Nível | *{Scope}* |
|-------|-----------|
| Assinatura | /subscriptions/{subscription-id} |
| Grupo de recursos | /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1 |
| Recurso | /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1 |

Substitua *{role-definition-id}* pela definição de função do GUID da função personalizada a ser excluída.

Substitua *{api-version}* por 2015-07-01.

### Resposta

Código de status: 200

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-16T00:07:02.9236555Z",
    "updatedOn": "2015-12-16T00:07:02.9236555Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/0bd62a70-e1b8-4e0b-a7c2-75cab365c95b",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "0bd62a70-e1b8-4e0b-a7c2-75cab365c95b"
}

```

<!---HONumber=AcomDC_0128_2016-->