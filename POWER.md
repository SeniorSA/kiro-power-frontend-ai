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
│   ├── locale/                            # Traduções i18n (FORA de app/)
│   │   ├── pt-BR.json                    # Português (idioma principal)
│   │   ├── en-US.json                    # Inglês
│   │   ├── es-ES.json                    # Espanhol
│   │   ├── fallback.ts                   # Gerado automaticamente pelo translations.js
│   │   ├── index.ts                      # Exports e mapa de traduções
│   │   └── locale.config.ts              # Configuração e utilitários de validação
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
{
  provide: 'TranslationLoader',
  useValue: {
    loadTranslations: async (language: string) => {
      const translations = await import(`../locale/${language}.json`);
      return translations.default || translations;
    }
  }
}
```

### Import de Estilos

No `styles.scss`:
```scss
@import '@seniorsistemas/components-ai/src/lib/styles/index.scss';
```

## Padrão de Traduções (i18n)

### Localização dos Arquivos

Os arquivos de tradução ficam em `src/locale/` (fora de `src/app/`):

```
src/
├── app/
│   └── ...
├── locale/
│   ├── pt-BR.json          # Português (idioma principal/fallback)
│   ├── en-US.json           # Inglês
│   ├── es-ES.json           # Espanhol
│   ├── fallback.ts          # Gerado automaticamente
│   ├── index.ts             # Exports e mapa de traduções
│   └── locale.config.ts     # Configuração e utilitários
```

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
| Título do app | `<d>.<s>.app_title` | `crmx.account.app_title` |
| Ações gerais | `<d>.<s>.save`, `cancel`, `delete`, `edit`, `add` | `crmx.account.save` |
| Entidade singular | `<d>.<s>.<entity_name>` | `crmx.account.account_stage` |
| Entidade plural | `<d>.<s>.<entity_name>s` | `crmx.account.account_stages` |
| Breadcrumb | `<d>.<s>.breadcrumb_<entity>` | `crmx.account.breadcrumb_account_stage` |
| Subtítulo | `<d>.<s>.<entity>_subtitle` | `crmx.account.account_stage_subtitle` |
| Novo/Editar | `<d>.<s>.<entity>_new`, `<entity>_edit` | `crmx.account.account_stage_new` |
| Status | `<d>.<s>.status_active`, `status_inactive`, `status_deleted` | `crmx.account.status_active` |
| Erros | `<d>.<s>.error_<contexto>` | `crmx.account.error_loading_data` |
| Erros de página | `<d>.<s>.error_page_not_found_title`, `error_access_denied_title`, `error_unauthorized_title` | `crmx.account.error_page_not_found_title` |
| Exportação | `<d>.<s>.export_<contexto>` | `crmx.account.export_button` |
| Bulk delete | `<d>.<s>.bulk_delete_<contexto>` | `crmx.account.bulk_delete_message` |
| Filtros | `<d>.<s>.filter_search_placeholder`, `filter_select_placeholder` | `crmx.account.filter_search_placeholder` |
| Formulários | `<d>.<s>.enter_<field>`, `select_<field>` | `crmx.account.enter_name` |
| Validação | `<d>.<s>.required_field`, `min_length_error`, `max_length_error` | `crmx.account.required_field` |
| CRUD genérico | `<d>.<s>.entity_created_success`, `entity_updated_success`, `entity_deleted_success` | `crmx.account.entity_created_success` |
| Registros | `<d>.<s>.record_created_successfully`, `record_deleted_successfully` | `crmx.account.record_created_successfully` |
| Paginação | `<d>.<s>.pagination_template` | `crmx.account.pagination_template` |
| Interpolação | `{{variavel}}` | `"{{entityName}} criado com sucesso"` |
| Interpolação PrimeNG | `{variavel}` | `"Mostrando {first} a {last} de {totalRecords} registros"` |

### Idiomas Suportados

- `pt-BR.json` — Português (principal, usado como base para fallback)
- `en-US.json` — Inglês
- `es-ES.json` — Espanhol

### Arquivo locale.config.ts

Configuração e utilitários de validação de traduções. Importa `SupportedLanguage` do components-ai:

```typescript
import { SupportedLanguage } from '@seniorsistemas/components-ai';

export const LOCALE_CONFIG = {
  basePath: './locale',
  fileExtension: '.json',
  languageFiles: {
    'pt-BR': 'pt-BR',
    'en-US': 'en-US',
    'es-ES': 'es-ES'
  } as Record<SupportedLanguage, string>,
  requiredKeys: [
    '<dominio>.<servico>.app_title',
    '<dominio>.<servico>.save',
    '<dominio>.<servico>.cancel',
    '<dominio>.<servico>.delete',
    '<dominio>.<servico>.select_language'
  ],
  keyPrefix: '<dominio>.<servico>'
};

// Utilitário para gerar o caminho completo do arquivo de tradução
export function getTranslationFilePath(language: SupportedLanguage): string {
  const fileName = LOCALE_CONFIG.languageFiles[language];
  return `${LOCALE_CONFIG.basePath}/${fileName}${LOCALE_CONFIG.fileExtension}`;
}

// Utilitário para validar se uma chave segue o padrão correto
export function isValidTranslationKey(key: string): boolean {
  return key.startsWith(LOCALE_CONFIG.keyPrefix + '.');
}

// Utilitário para gerar uma chave de tradução válida
export function createTranslationKey(subKey: string): string {
  return `${LOCALE_CONFIG.keyPrefix}.${subKey}`;
}

// Utilitário para validar se um arquivo de tradução tem a estrutura correta
export function validateTranslationFile(translations: any): { isValid: boolean; errors: string[] } {
  const errors: string[] = [];
  if (!translations || typeof translations !== 'object') {
    errors.push('Arquivo de tradução deve ser um objeto JSON válido');
    return { isValid: false, errors };
  }
  for (const requiredKey of LOCALE_CONFIG.requiredKeys) {
    if (!translations[requiredKey]) {
      errors.push(`Chave obrigatória ausente: ${requiredKey}`);
    }
  }
  for (const key in translations) {
    if (key.startsWith('_')) continue;
    if (!isValidTranslationKey(key)) {
      errors.push(`Chave inválida (não segue o padrão): ${key}`);
    }
    if (typeof translations[key] !== 'string') {
      errors.push(`Valor deve ser string para a chave: ${key}`);
    }
  }
  return { isValid: errors.length === 0, errors };
}

// Utilitário para verificar se a estrutura é plana (não aninhada)
export function isFlatStructure(obj: any): boolean {
  for (const key in obj) {
    if (key.startsWith('_')) continue;
    if (typeof obj[key] === 'object' && obj[key] !== null) {
      return false;
    }
  }
  return true;
}
```

### Arquivo index.ts

Exporta os arquivos de tradução para uso direto e lazy loading:

```typescript
// Exportações dos arquivos de tradução para facilitar importações
export { default as ptBR } from './pt-BR.json';
export { default as enUS } from './en-US.json';
export { default as esES } from './es-ES.json';

// Mapa de traduções para uso programático (lazy loading)
export const translations = {
  'pt-BR': () => import('./pt-BR.json'),
  'en-US': () => import('./en-US.json'),
  'es-ES': () => import('./es-ES.json')
};

// Lista de idiomas disponíveis
export const availableLanguages = ['pt-BR', 'en-US', 'es-ES'] as const;
```

### Arquivo fallback.ts

Gerado automaticamente pelo script `translations.js` a partir do `pt-BR.json`. Executado no build via `npm run translations`.

O arquivo gerado tem o formato:
```typescript
export const fallback: any = {
    "<dominio>.<servico>.save": "Salvar",
    "<dominio>.<servico>.cancel": "Cancelar",
    // ... todas as chaves do pt-BR.json
};
```

### Script translations.js (raiz do projeto)

```javascript
const fs = require('fs');
const fallback = require('./src/locale/pt-BR.json');
const translationPath = 'src/locale/fallback.ts';

fs.writeFile(
    `${translationPath}`,
    `export const fallback: any = ${JSON.stringify(fallback, null, 4)};
`,
    (err) => err && console.error(`Error writing file ${translationPath}: ${err}`)
);
```

### Integração no app.config.ts

O carregamento de traduções é configurado via provider `TranslationLoader` com dynamic import:

```typescript
import { ApplicationConfig, provideZoneChangeDetection } from '@angular/core';
import { provideRouter, withHashLocation } from '@angular/router';
import { provideAnimations } from '@angular/platform-browser/animations';
import { provideHttpClient, withInterceptors } from '@angular/common/http';
import { provideSeniorPrimeNG, apiInterceptor } from '@seniorsistemas/components-ai';

import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [
    provideZoneChangeDetection({ eventCoalescing: true }),
    provideRouter(routes, withHashLocation()),
    provideAnimations(),
    provideHttpClient(withInterceptors([apiInterceptor])),
    provideSeniorPrimeNG({
      darkModeSelector: '.app-dark'
    }),
    {
      provide: 'TranslationLoader',
      useValue: {
        loadTranslations: async (language: string) => {
          const translations = await import(`../locale/${language}.json`);
          return translations.default || translations;
        }
      }
    }
  ]
};
```

Note que o import usa `../locale/` (relativo a `src/app/`), pois a pasta `locale/` fica em `src/locale/`.

### Uso no Código

```typescript
// Via TranslationService (injetado)
translationService.translate('<dominio>.<servico>.save');

// Com interpolação
translationService.translate('<dominio>.<servico>.entity_created_success', { entityName: 'Conta' });

// Via pipe no template
{{ '<dominio>.<servico>.save' | translate }}
```

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
