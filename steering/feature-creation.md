# Criação de Feature (CRUD)

## Pré-requisitos

Para criar uma feature, você precisa saber:
1. **Nome da entidade** — ex: `account-stage`, `business-origin`
2. **Campos da entidade** — ex: `code`, `name`, `status`
3. **Domínio e serviço do projeto** — extrair do `package.json`

## Arquivos a Criar

Para uma feature chamada `{entity-kebab}` (ex: `account-stage`):

### 1. Interface da Entidade

`src/app/core/entities/{entity-kebab}.interface.ts`

```typescript
import { Status } from '../enums/status.enum';
import { BaseEntity } from './base-entity.interface';

export interface {EntityPascal} extends BaseEntity {
  id: string;
  code: number;
  name: string;
  status: Status | string;
  // adicionar campos específicos
}
```

### 2. Service da Entidade

`src/app/core/services/{entity-kebab}.service.ts`

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { EntityService, EntityListResponse, ListParams, TranslationService } from '@seniorsistemas/components-ai';
import { {EntityPascal} } from '../entities/{entity-kebab}.interface';

@Injectable({ providedIn: 'root' })
export class {EntityPascal}Service extends EntityService<{EntityPascal}> {
  protected entityUrl = '{domain}/{service}/entities/{entityCamel}';
  protected actionsUrl = '{domain}/{service}/actions';

  constructor(http: HttpClient, translationService: TranslationService) {
    super(http, translationService);
    this.initializeService();
  }

  override list(listParams: ListParams = {}): Observable<EntityListResponse<{EntityPascal}>> {
    const statusFilter = "status != 'DELETED'";
    const filterQuery = listParams.filterQuery
      ? `(${listParams.filterQuery}) and ${statusFilter}`
      : statusFilter;
    const sort = listParams.sort?.length ? listParams.sort : [{ field: 'code', order: 1 }];
    return super.list({ ...listParams, filterQuery, sort });
  }
}
```

### 3. Config da Feature

`src/app/features/{entity-kebab}/{entity-kebab}.config.ts`

```typescript
import { Validators } from '@angular/forms';
import { EntityListConfig } from '@seniorsistemas/components-ai';
import { Status } from '../../core/enums/status.enum';

export function get{EntityPascal}Config(translationService: any): EntityListConfig {
  const STATUS_OPTIONS = [
    { label: translationService.translate('{domain}.{service}.all'), value: null },
    { label: translationService.translate('{domain}.{service}.status_active'), value: Status.Active },
    { label: translationService.translate('{domain}.{service}.status_inactive'), value: Status.Inactive }
  ];

  const FORM_STATUS_OPTIONS = [
    { label: translationService.translate('{domain}.{service}.status_active'), value: Status.Active },
    { label: translationService.translate('{domain}.{service}.status_inactive'), value: Status.Inactive }
  ];

  return {
    entityName: translationService.translate('{domain}.{service}.{entity_snake_plural}'),
    entityIcon: 'pi pi-flag',
    subtitle: translationService.translate('{domain}.{service}.{entity_snake}_subtitle'),
    addButtonLabel: translationService.translate('{domain}.{service}.add'),
    exportEnabled: true,
    bulkDeleteEnabled: true,

    columns: [
      { field: 'code', header: translationService.translate('{domain}.{service}.code'), sortable: true, width: '20%', type: 'number' },
      { field: 'name', header: translationService.translate('{domain}.{service}.name'), sortable: true, width: '40%', type: 'text' },
      { field: 'status', header: translationService.translate('{domain}.{service}.status'), sortable: true, width: '20%', type: 'status' }
    ],

    filterFields: [
      { name: 'name', label: translationService.translate('{domain}.{service}.name'), type: 'text', cols: 6, placeholder: translationService.translate('{domain}.{service}.enter_name') },
      { name: 'status', label: translationService.translate('{domain}.{service}.status'), type: 'dropdown', cols: 6, options: STATUS_OPTIONS }
    ],

    formFields: [
      { name: 'name', label: translationService.translate('{domain}.{service}.name'), type: 'text', cols: 8, required: true, validators: [Validators.required, Validators.maxLength(100)], placeholder: translationService.translate('{domain}.{service}.enter_name') },
      { name: 'status', label: translationService.translate('{domain}.{service}.status'), type: 'dropdown', cols: 4, required: true, validators: [Validators.required], options: FORM_STATUS_OPTIONS }
    ]
  };
}
```

### 4. Component da Feature

`src/app/features/{entity-kebab}/{entity-kebab}.component.ts`

O componente segue o padrão standalone com:
- Imports de PrimeNG (TableModule, ButtonModule, TagModule, CardModule, ToolbarModule, etc.)
- Imports do components-ai (ExportDialogComponent, BulkDeleteDialogComponent, DynamicFormComponent, TranslatePipe)
- Providers: MessageService, ConfirmationService
- Injeção de: Service da entidade, MessageService, ConfirmationService, TranslationService
- Uso de StorageService para persistir filtros

### 5. Template da Feature

`src/app/features/{entity-kebab}/{entity-kebab}.component.html`

O template segue o padrão com:
- Card com toolbar (título, botões de ação)
- Painel de filtros com DynamicFormComponent mode="filter"
- Tabela PrimeNG com paginação server-side
- Coluna de status com Tag colorido
- Coluna de ações (editar, excluir)
- DynamicFormComponent mode="create"/"edit" em dialog
- ExportDialogComponent
- BulkDeleteDialogComponent

### 6. Rota

Adicionar em `src/app/app.routes.ts`:

```typescript
{
  path: '{entity-kebab}',
  loadComponent: () => import('./features/{entity-kebab}/{entity-kebab}.component')
    .then(m => m.{EntityPascal}Component),
  data: {
    breadcrumb: {
      title: '{domain}.{service}.breadcrumb_{entity_snake}',
      icon: 'pi pi-{icon}'
    }
  }
}
```

### 7. Traduções

Adicionar nos 3 arquivos de locale (`pt-BR.json`, `en-US.json`, `es-ES.json`):

```json
{
  "{domain}.{service}.{entity_snake}": "Nome Singular",
  "{domain}.{service}.{entity_snake_plural}": "Nome Plural",
  "{domain}.{service}.{entity_snake}_subtitle": "Gerencie os/as ...",
  "{domain}.{service}.breadcrumb_{entity_snake}": "Nome para Breadcrumb",
  "{domain}.{service}.{entity_snake}_new": "Cadastrar ...",
  "{domain}.{service}.{entity_snake}_edit": "Editar ..."
}
```

Após adicionar as traduções, executar `npm run translations` para atualizar o fallback.ts.

## Checklist

- [ ] Interface criada em `core/entities/`
- [ ] Service criado em `core/services/` (ou na feature)
- [ ] Config criado na feature
- [ ] Component criado na feature
- [ ] Template HTML criado
- [ ] Rota adicionada em `app.routes.ts`
- [ ] Traduções adicionadas nos 3 idiomas
- [ ] `npm run translations` executado
- [ ] Menu adicionado em `static/config.json` (se necessário)
