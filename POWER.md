---
name: "senior-frontend-architecture"
displayName: "Senior Frontend Architecture"
description: "Guia completo da arquitetura frontend Angular da Senior Sistemas. Padrões de features, criação de projetos, uso do components-ai, traduções i18n e convenções de código."
keywords: ["angular", "frontend", "senior", "components-ai", "crmx", "architecture", "project-creation", "i18n"]
author: "Leonardo Cardoso"
---

# Senior Frontend Architecture

## Overview

Este power documenta a arquitetura completa dos projetos frontend Angular da Senior Sistemas. Ele guia o Kiro para:

- Entender a estrutura de pastas e padrões de código
- Criar novos projetos do zero seguindo as convenções
- Criar features (CRUD) seguindo o padrão estabelecido
- Usar corretamente a biblioteca `@seniorsistemas/components-ai`
- Seguir o padrão de traduções i18n

## Available Steering Files

- **project-creation.md** — Guia completo para criar um novo projeto frontend do zero
- **feature-creation.md** — Guia para criar novas features (CRUD) dentro de um projeto existente

## Convenção de Nomenclatura

### Projeto

O nome do projeto segue o padrão: `<dominio>-<servico>-frontend`

Exemplos:
- `crmx-account-frontend` (domínio: crmx, serviço: account)
- `crmx-business-frontend` (domínio: crmx, serviço: business)

### Onde domínio e serviço aparecem

| Local | Padrão | Exemplo |
|-------|--------|---------|
| Pasta do projeto | `<dominio>-<servico>-frontend` | `crmx-account-frontend` |
| package.json `name` | `<dominio>-<servico>-frontend` | `crmx-account-frontend` |
| package.json `project.domain` | `<dominio>` | `crmx` |
| package.json `project.service` | `<servico>` | `account` |
| package.json `project.serviceDependencies` | `<dominio>.<servico>` | `crmx.account` |
| Chaves de tradução | `<dominio>.<servico>.<chave>` | `crmx.account.save` |
| locale.config.ts `keyPrefix` | `<dominio>.<servico>` | `crmx.account` |
| URL das entidades (service) | `<dominio>/<servico>/entities/<entity>` | `crmx/account/entities/accountStage` |
| URL das actions (service) | `<dominio>/<servico>/actions` | `crmx/account/actions` |
| static/config.json `domain` | `<dominio>` | `crmx` |
| sonar-project.properties | `<servico>-frontend` | `account-frontend` |
| angular.json project name | `<dominio>-<servico>-frontend` | `crmx-account-frontend` |
| index.html `<title>` | `<servico>-frontend` | `account-frontend` |
| app.component.ts `title` | `<dominio>-<servico>-frontend` | `crmx-account-frontend` |
| Auth resources | `res://senior.com.br/<dominio>/<servico>/entities/<entity>` | `res://senior.com.br/crmx/account/entities/accountStage` |

### IMPORTANTE: Ao criar um projeto

Quando o usuário solicitar a criação de um projeto e NÃO informar o domínio e o serviço, você DEVE perguntar:

1. Qual o **domínio**? (ex: crmx, hcm, erp)
2. Qual o **serviço**? (ex: account, business, payroll)

Sem essas informações, não é possível criar o projeto corretamente.

## Estrutura de Pastas

```
<dominio>-<servico>-frontend/
├── src/
│   ├── app/
│   │   ├── core/                          # Lógica de negócio central
│   │   │   ├── entities/                  # Interfaces TypeScript das entidades
│   │   │   │   ├── <entity>.interface.ts
│   │   │   │   └── base-entity.interface.ts  # Re-export do components-ai
│   │   │   ├── enums/                     # Enums do domínio
│   │   │   │   └── status.enum.ts
│   │   │   └── services/                  # Services das entidades (CRUD)
│   │   │       └── <entity>.service.ts
│   │   ├── features/                      # Módulos de funcionalidade
│   │   │   ├── main/                      # Painel de desenvolvimento
│   │   │   └── <feature>/                 # Feature CRUD
│   │   │       ├── <feature>.component.ts
│   │   │       ├── <feature>.component.html
│   │   │       ├── <feature>.config.ts
│   │   │       └── <feature>.service.ts   # (opcional, se não usar core/services)
│   │   ├── locale/                        # Traduções i18n
│   │   │   ├── pt-BR.json
│   │   │   ├── en-US.json
│   │   │   ├── es-ES.json
│   │   │   ├── fallback.ts               # Gerado automaticamente
│   │   │   ├── index.ts                  # Exports e mapa de traduções
│   │   │   └── locale.config.ts          # Configuração de locale
│   │   ├── shared/                        # Compartilhado entre features
│   │   │   ├── components/
│   │   │   │   └── theme-toggle/
│   │   │   ├── directives/
│   │   │   ├── services/
│   │   │   ├── styles/
│   │   │   │   └── entity-list.shared.scss
│   │   │   └── utils/
│   │   ├── app.component.ts
│   │   ├── app.component.html
│   │   ├── app.component.scss
│   │   ├── app.config.ts
│   │   └── app.routes.ts
│   ├── assets/
│   │   └── tailwind.css
│   ├── environments/
│   │   ├── environment.default.ts
│   │   ├── environment.ts
│   │   └── environment.prod.ts
│   ├── static/
│   │   └── config.json                   # Menu e autorização
│   ├── index.html
│   ├── main.ts
│   └── styles.scss
├── public/
│   └── favicon.ico
├── angular.json
├── eslint.config.js
├── package.json
├── server.js
├── sonar-project.properties
├── tailwind.config.js
├── translations.js
├── tsconfig.json
├── tsconfig.app.json
├── .editorconfig
├── .gitignore
└── .gitlab-ci.yml
```

## Stack Tecnológica

| Tecnologia | Versão | Uso |
|-----------|--------|-----|
| Angular | 18.2.x | Framework principal |
| PrimeNG | 18.x | Componentes UI |
| Tailwind CSS | 3.4.x | Utilitários CSS |
| tailwindcss-primeui | - | Plugin Tailwind para PrimeNG |
| @seniorsistemas/components-ai | latest | Biblioteca de componentes compartilhados |
| RxJS | 7.8.x | Programação reativa |
| XLSX | 0.18.x | Exportação Excel |

## Biblioteca @seniorsistemas/components-ai

A biblioteca fornece componentes, serviços e utilitários reutilizáveis. A documentação completa dos componentes está na pasta `docs/` da biblioteca.

### Componentes Principais

| Componente | Seletor | Uso |
|-----------|---------|-----|
| DynamicFormComponent | `sia-dynamic-form` | Formulários dinâmicos (create, edit, filter, inline-edit) |
| ExportDialogComponent | `sia-export-dialog` | Exportação Excel/PDF |
| BulkDeleteDialogComponent | `sia-bulk-delete-dialog` | Exclusão em massa |
| BreadcrumbComponent | `sia-breadcrumb` | Navegação breadcrumb |

### Serviços Principais

| Serviço | Uso |
|---------|-----|
| EntityService\<T\> | Base genérica para CRUD (extend obrigatório) |
| TranslationService | Gerenciamento de i18n |
| AuthService | Autenticação |
| ThemeService | Tema claro/escuro |
| PermissionService | Controle de permissões |

### Pipes e Diretivas

| Item | Uso |
|------|-----|
| TranslatePipe | `{{ 'chave' \| translate }}` |
| DocumentMaskDirective | Máscara CPF/CNPJ |
| PhoneMaskDirective | Máscara telefone |
| MoneyMaskDirective | Máscara moeda |

### Provider Obrigatório

No `app.config.ts`, usar:
```typescript
import { provideSeniorPrimeNG, apiInterceptor } from '@seniorsistemas/components-ai';

provideSeniorPrimeNG({ darkModeSelector: '.app-dark' }),
provideHttpClient(withInterceptors([apiInterceptor])),
```

### Import de Estilos

No `styles.scss`:
```scss
@import '@seniorsistemas/components-ai/src/lib/styles/index.scss';
```

## Padrão de Traduções (i18n)

### Formato das Chaves

Todas as chaves seguem o padrão **flat** (sem aninhamento):

```
<dominio>.<servico>.<chave_em_snake_case>
```

Exemplos:
```json
{
  "crmx.account.save": "Salvar",
  "crmx.account.account_stages": "Estágios",
  "crmx.account.breadcrumb_account_stage": "Estágio de Conta",
  "crmx.account.entity_created_success": "{{entityName}} criado com sucesso"
}
```

### Categorias de Chaves

| Categoria | Padrão | Exemplo |
|-----------|--------|---------|
| Ações gerais | `<d>.<s>.save`, `cancel`, `delete`, `edit`, `add` | `crmx.account.save` |
| Entidade singular | `<d>.<s>.<entity_name>` | `crmx.account.account_stage` |
| Entidade plural | `<d>.<s>.<entity_name>s` | `crmx.account.account_stages` |
| Breadcrumb | `<d>.<s>.breadcrumb_<entity>` | `crmx.account.breadcrumb_account_stage` |
| Subtítulo | `<d>.<s>.<entity>_subtitle` | `crmx.account.account_stage_subtitle` |
| Novo/Editar | `<d>.<s>.<entity>_new`, `<entity>_edit` | `crmx.account.account_stage_new` |
| Status | `<d>.<s>.status_active`, `status_inactive` | `crmx.account.status_active` |
| Erros | `<d>.<s>.error_<contexto>` | `crmx.account.error_loading_data` |
| Exportação | `<d>.<s>.export_<contexto>` | `crmx.account.export_button` |
| Bulk delete | `<d>.<s>.bulk_delete_<contexto>` | `crmx.account.bulk_delete_message` |
| Interpolação | `{{variavel}}` | `"{{entityName}} criado com sucesso"` |

### Idiomas Suportados

- `pt-BR.json` — Português (principal)
- `en-US.json` — Inglês
- `es-ES.json` — Espanhol

### Arquivo fallback.ts

Gerado automaticamente pelo script `translations.js` a partir do `pt-BR.json`. Executado no build via `npm run translations`.

## Padrão de Services

Todos os services estendem `EntityService<T>` do components-ai:

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { EntityService, TranslationService } from '@seniorsistemas/components-ai';
import { MyEntity } from '../../core/entities/my-entity.interface';

@Injectable({ providedIn: 'root' })
export class MyEntityService extends EntityService<MyEntity> {
  protected entityUrl = '<dominio>/<servico>/entities/<entityCamelCase>';
  protected actionsUrl = '<dominio>/<servico>/actions';

  constructor(http: HttpClient, translationService: TranslationService) {
    super(http, translationService);
    this.initializeService();
  }
}
```

## Padrão de Rotas

- Standalone components com lazy loading
- Hash-based routing (`withHashLocation()`)
- Breadcrumb via `data` na rota

```typescript
{
  path: '<entity-kebab>',
  loadComponent: () => import('./features/<entity-kebab>/<entity-kebab>.component')
    .then(m => m.<EntityPascal>Component),
  data: {
    breadcrumb: {
      title: '<dominio>.<servico>.breadcrumb_<entity_snake>',
      icon: 'pi pi-<icon>'
    }
  }
}
```

## Padrão de Entities (Interfaces)

```typescript
import { Status } from '../enums/status.enum';
import { BaseEntity } from './base-entity.interface';

export interface MyEntity extends BaseEntity {
  id: string;
  code: number;
  name: string;
  status: Status | string;
}
```

## Configuração de Feature (Config)

Cada feature tem um arquivo `*.config.ts` que define colunas, filtros e campos do formulário:

```typescript
import { EntityListConfig } from '@seniorsistemas/components-ai';

export function getMyEntityConfig(translationService: any): EntityListConfig {
  return {
    entityName: translationService.translate('<dominio>.<servico>.<entity_plural>'),
    entityIcon: 'pi pi-<icon>',
    subtitle: translationService.translate('<dominio>.<servico>.<entity>_subtitle'),
    addButtonLabel: translationService.translate('<dominio>.<servico>.add'),
    exportEnabled: true,
    bulkDeleteEnabled: true,
    columns: [ /* ... */ ],
    filterFields: [ /* ... */ ],
    formFields: [ /* ... */ ],
  };
}
```

## CI/CD

```yaml
stages:
- validate
- generate
- compile
- report
- sast
- sca
- pre-release
- release
- delivery
- publish
- deploy

include:
  project: $SCI_PROJECT_TEMPLATES
  file:
  - flow/validate/complete.gitlab-ci.yml
  - flow/release/complete.gitlab-ci.yml
  - flow/release/semantic.gitlab-ci.yml
  - flow/deploy/prod.gitlab-ci.yml
```

## Configurações de Qualidade

### ESLint
- TypeScript ESLint + Angular ESLint
- Component selector: `app-` prefix, kebab-case
- Directive selector: `app` prefix, camelCase
- Template accessibility checks habilitados

### SonarQube
- Linguagem: TypeScript
- Cobertura: LCOV
- Exclusões: spec files, environments, enums, routing, modules

### EditorConfig
- Charset: UTF-8
- Indent: 2 espaços
- Quotes: Single quotes para TypeScript
- Final newline: Sempre

## Referências

- [Angular 18 Docs](https://angular.dev)
- [PrimeNG 18](https://primeng.org)
- [Tailwind CSS](https://tailwindcss.com)
- Documentação do components-ai: `components-ai/projects/components-ai/docs/`
