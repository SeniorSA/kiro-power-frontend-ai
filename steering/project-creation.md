# Criação de Projeto Frontend

## Pré-requisitos

Antes de criar o projeto, você DEVE ter:
1. **Domínio** — ex: `crmx`, `hcm`, `erp`
2. **Serviço** — ex: `account`, `business`, `payroll`

Se o usuário não informar, PERGUNTE antes de prosseguir.

## Variáveis de Substituição

Ao longo deste guia, substitua:
- `{domain}` → domínio (ex: `crmx`)
- `{service}` → serviço (ex: `account`)
- `{Domain}` → domínio PascalCase (ex: `Crmx`)
- `{Service}` → serviço PascalCase (ex: `Account`)

## Passo a Passo

### 1. Criar estrutura de pastas

```
{domain}-{service}-frontend/
├── src/
│   ├── app/
│   │   ├── core/
│   │   │   ├── entities/
│   │   │   ├── enums/
│   │   │   └── services/
│   │   ├── features/
│   │   │   └── main/
│   │   ├── locale/
│   │   ├── shared/
│   │   │   ├── components/
│   │   │   │   └── theme-toggle/
│   │   │   ├── directives/
│   │   │   ├── services/
│   │   │   ├── styles/
│   │   │   └── utils/
│   │   ├── app.component.ts
│   │   ├── app.component.html
│   │   ├── app.component.scss
│   │   ├── app.config.ts
│   │   └── app.routes.ts
│   ├── assets/
│   │   └── tailwind.css
│   ├── environments/
│   ├── static/
│   ├── index.html
│   ├── main.ts
│   └── styles.scss
├── public/
│   └── favicon.ico
└── (config files)
```

### 2. package.json

```json
{
  "name": "{domain}-{service}-frontend",
  "version": "1.0.0",
  "project": {
    "app": "gestao-relacionamento",
    "domain": "{domain}",
    "service": "{service}",
    "serviceDependencies": [
      {
        "service": "{domain}.{service}",
        "version": "1.0.0"
      }
    ]
  },
  "scripts": {
    "ng": "ng",
    "start": "node server.js",
    "translations": "node translations.js",
    "build": "npm run translations && ng build --configuration production && npm run postbuild",
    "postbuild": "node -e \"const fs=require('fs');const path=require('path');const src='dist/browser';const dest='dist';if(fs.existsSync(src)){fs.readdirSync(src).forEach(file=>{const srcPath=path.join(src,file);const destPath=path.join(dest,file);if(fs.statSync(srcPath).isDirectory()){fs.cpSync(srcPath,destPath,{recursive:true})}else{fs.copyFileSync(srcPath,destPath)}});fs.rmSync(src,{recursive:true,force:true})}\"",
    "watch": "ng build --watch --configuration development",
    "test": "ng test --watch=false"
  },
  "private": true,
  "dependencies": {
    "@angular/animations": "^18.2.0",
    "@angular/cdk": "^18.2.14",
    "@angular/common": "^18.2.0",
    "@angular/compiler": "^18.2.0",
    "@angular/core": "^18.2.0",
    "@angular/forms": "^18.2.0",
    "@angular/localize": "^18.2.0",
    "@angular/platform-browser": "^18.2.0",
    "@angular/platform-browser-dynamic": "^18.2.0",
    "@angular/router": "^18.2.0",
    "@primeuix/themes": "^2.0.0",
    "chart.js": "^4.5.1",
    "primeng": "^18.0.2",
    "primeicons": "^7.0.0",
    "rxjs": "~7.8.0",
    "tslib": "^2.3.0",
    "xlsx": "^0.18.5",
    "zone.js": "~0.14.10",
    "@seniorsistemas/components-ai": "latest"
  },
  "devDependencies": {
    "@angular-devkit/build-angular": "^18.2.0",
    "@angular/cli": "^18.2.0",
    "@angular/compiler-cli": "^18.2.0",
    "@eslint/js": "^9.20.0",
    "angular-eslint": "^19.1.0",
    "eslint": "^9.20.0",
    "jasmine-core": "~5.2.0",
    "karma": "~6.4.0",
    "karma-chrome-launcher": "~3.2.0",
    "karma-coverage": "~2.2.0",
    "karma-jasmine": "~5.1.0",
    "karma-jasmine-html-reporter": "~2.1.0",
    "tailwindcss": "^3.4.19",
    "tailwindcss-primeui": "^0.3.4",
    "typescript": "~5.5.2",
    "typescript-eslint": "^8.24.0"
  }
}
```

### 3. angular.json

```json
{
  "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
  "version": 1,
  "cli": { "packageManager": "npm" },
  "newProjectRoot": "projects",
  "projects": {
    "{domain}-{service}-frontend": {
      "projectType": "application",
      "schematics": {
        "@schematics/angular:component": { "style": "scss" }
      },
      "root": "",
      "sourceRoot": "src",
      "prefix": "app",
      "architect": {
        "build": {
          "builder": "@angular-devkit/build-angular:application",
          "options": {
            "outputPath": { "base": "dist" },
            "index": "src/index.html",
            "browser": "src/main.ts",
            "polyfills": ["zone.js", "@angular/localize/init"],
            "tsConfig": "tsconfig.app.json",
            "inlineStyleLanguage": "scss",
            "assets": [
              { "glob": "favicon.ico", "input": "public", "output": "/" },
              { "glob": ".htaccess", "input": "src", "output": "/" },
              { "glob": "**/*", "input": "src/static", "output": "/" }
            ],
            "styles": ["src/styles.scss", "src/assets/tailwind.css"],
            "scripts": []
          },
          "configurations": {
            "production": {
              "budgets": [
                { "type": "initial", "maximumWarning": "2MB", "maximumError": "5MB" },
                { "type": "anyComponentStyle", "maximumWarning": "10kB", "maximumError": "25kB" }
              ],
              "fileReplacements": [
                { "replace": "src/environments/environment.ts", "with": "src/environments/environment.prod.ts" }
              ],
              "outputHashing": "all"
            },
            "development": {
              "optimization": false,
              "extractLicenses": false,
              "sourceMap": true
            }
          },
          "defaultConfiguration": "production"
        },
        "serve": {
          "builder": "@angular-devkit/build-angular:dev-server",
          "configurations": {
            "production": { "buildTarget": "{domain}-{service}-frontend:build:production" },
            "development": { "buildTarget": "{domain}-{service}-frontend:build:development" }
          },
          "defaultConfiguration": "development"
        },
        "test": {
          "builder": "@angular-devkit/build-angular:karma",
          "options": {
            "polyfills": ["zone.js", "zone.js/testing", "@angular/localize/init"],
            "tsConfig": "tsconfig.spec.json",
            "karmaConfig": "karma.conf.js",
            "inlineStyleLanguage": "scss",
            "assets": [
              { "glob": "favicon.ico", "input": "public", "output": "/" },
              { "glob": "**/*", "input": "src/static", "output": "/" }
            ],
            "styles": ["src/styles.scss", "src/assets/tailwind.css"],
            "scripts": []
          }
        }
      }
    }
  }
}
```

### 4. tsconfig.json

```json
{
  "compileOnSave": false,
  "compilerOptions": {
    "outDir": "./dist/out-tsc",
    "strict": true,
    "noImplicitOverride": true,
    "noPropertyAccessFromIndexSignature": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "skipLibCheck": true,
    "isolatedModules": true,
    "esModuleInterop": true,
    "sourceMap": true,
    "declaration": false,
    "experimentalDecorators": true,
    "moduleResolution": "bundler",
    "importHelpers": true,
    "target": "ES2022",
    "module": "ES2022",
    "resolveJsonModule": true,
    "allowSyntheticDefaultImports": true,
    "lib": ["ES2022", "dom"]
  },
  "exclude": ["**/*.template.ts"],
  "angularCompilerOptions": {
    "enableI18nLegacyMessageIdFormat": false,
    "strictInjectionParameters": true,
    "strictInputAccessModifiers": true,
    "strictTemplates": true
  }
}
```

### 5. tsconfig.app.json

```json
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "outDir": "./out-tsc/app",
    "types": ["@angular/localize"]
  },
  "files": ["src/main.ts"],
  "include": ["src/**/*.d.ts"]
}
```

### 6. src/main.ts

```typescript
/// <reference types="@angular/localize" />

import { bootstrapApplication } from '@angular/platform-browser';
import { appConfig } from './app/app.config';
import { AppComponent } from './app/app.component';

bootstrapApplication(AppComponent, appConfig)
  .catch((err) => console.error(err));
```

### 7. src/app/app.config.ts

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
          const translations = await import(`./locale/${language}.json`);
          return translations.default || translations;
        }
      }
    }
  ]
};
```

### 8. src/app/app.routes.ts

```typescript
import { Routes } from '@angular/router';

export const routes: Routes = [
  {
    path: '',
    redirectTo: 'main',
    pathMatch: 'full'
  },
  {
    path: 'main',
    loadComponent: () => import('./features/main/main.component').then(m => m.MainComponent),
    data: {
      breadcrumb: {
        title: '{domain}.{service}.breadcrumb_development_panel',
        icon: 'pi pi-home'
      }
    }
  }
];
```

### 9. src/app/app.component.ts

```typescript
import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { RouterOutlet } from '@angular/router';
import { TranslationService, AuthService, ThemeService, BreadcrumbComponent } from '@seniorsistemas/components-ai';
import { TranslatePipe } from '@seniorsistemas/components-ai';
import { ToastModule } from 'primeng/toast';
import { environment } from '../environments/environment';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [CommonModule, RouterOutlet, BreadcrumbComponent, TranslatePipe, ToastModule],
  templateUrl: './app.component.html',
  styleUrl: './app.component.scss'
})
export class AppComponent implements OnInit {
  title = '{domain}-{service}-frontend';
  isProduction = environment.production;

  constructor(
    public translationService: TranslationService,
    private authService: AuthService,
    private themeService: ThemeService
  ) {}

  async ngOnInit() {
    try {
      await this.translationService.syncWithToken();
    } catch (error) {
      console.warn('Could not sync language with token:', error);
    }
  }
}
```

### 10. src/app/app.component.html

```html
<div class="layout-wrapper">
  <header class="app-header">
    <div class="header-content">
      <div class="header-left">
        <h1 class="app-title">{{ '{domain}.{service}.app_title' | translate }} | {Domain}X</h1>
      </div>
    </div>
  </header>

  <div class="layout-main-container">
    <div class="layout-main">
      <sia-breadcrumb [isProduction]="isProduction"></sia-breadcrumb>
      <router-outlet></router-outlet>
    </div>
  </div>
</div>

<p-toast></p-toast>
```

### 11. Locale Files

#### src/app/locale/pt-BR.json (mínimo)

```json
{
  "{domain}.{service}.app_title": "Nome da Aplicação",
  "{domain}.{service}.save": "Salvar",
  "{domain}.{service}.cancel": "Cancelar",
  "{domain}.{service}.delete": "Excluir",
  "{domain}.{service}.edit": "Editar",
  "{domain}.{service}.add": "Cadastrar",
  "{domain}.{service}.search": "Pesquisar",
  "{domain}.{service}.filter": "Filtrar",
  "{domain}.{service}.clear": "Limpar",
  "{domain}.{service}.confirm": "Confirmar",
  "{domain}.{service}.yes": "Sim",
  "{domain}.{service}.no": "Não",
  "{domain}.{service}.ok": "OK",
  "{domain}.{service}.close": "Fechar",
  "{domain}.{service}.loading": "Carregando...",
  "{domain}.{service}.error": "Erro",
  "{domain}.{service}.success": "Sucesso",
  "{domain}.{service}.warning": "Aviso",
  "{domain}.{service}.back": "Voltar",
  "{domain}.{service}.new": "Cadastrar",
  "{domain}.{service}.all": "Todos",
  "{domain}.{service}.code": "Código",
  "{domain}.{service}.name": "Nome",
  "{domain}.{service}.status": "Status",
  "{domain}.{service}.status_active": "Ativo",
  "{domain}.{service}.status_inactive": "Inativo",
  "{domain}.{service}.description": "Descrição",
  "{domain}.{service}.actions": "Ações",
  "{domain}.{service}.required_field": "Campo obrigatório",
  "{domain}.{service}.save_success": "Dados salvos com sucesso",
  "{domain}.{service}.delete_success": "Item excluído com sucesso",
  "{domain}.{service}.delete_confirm": "Tem certeza que deseja excluir este item?",
  "{domain}.{service}.no_records_found": "Nenhum registro encontrado",
  "{domain}.{service}.operation_success": "Operação realizada com sucesso",
  "{domain}.{service}.operation_error": "Erro ao realizar operação",
  "{domain}.{service}.entity_created_success": "{{entityName}} criado com sucesso",
  "{domain}.{service}.entity_updated_success": "{{entityName}} atualizado com sucesso",
  "{domain}.{service}.entity_deleted_success": "{{entityName}} excluído com sucesso",
  "{domain}.{service}.confirm_delete": "Confirmar Exclusão",
  "{domain}.{service}.confirm_delete_message": "Deseja realmente excluir {{itemName}}?",
  "{domain}.{service}.bulk_delete_message": "Você está prestes a excluir {{count}} registro(s).",
  "{domain}.{service}.export_button": "Exportar",
  "{domain}.{service}.pagination_template": "Mostrando {first} a {last} de {totalRecords} registros",
  "{domain}.{service}.breadcrumb_development_panel": "Painel de Desenvolvimento",
  "{domain}.{service}.development_panel": "Painel de Desenvolvimento",
  "{domain}.{service}.quick_access_subtitle": "Acesso rápido às rotas principais e informações do sistema",
  "{domain}.{service}.project_information": "Informações do Projeto",
  "{domain}.{service}.main_routes": "Rotas Principais",
  "{domain}.{service}.select_language": "Selecionar idioma"
}
```

#### src/app/locale/en-US.json

Mesmo formato, com valores em inglês.

#### src/app/locale/es-ES.json

Mesmo formato, com valores em espanhol.

#### src/app/locale/locale.config.ts

```typescript
import { SupportedLanguage } from '@seniorsistemas/components-ai';

export const LOCALE_CONFIG = {
  basePath: './app/locale',
  fileExtension: '.json',
  languageFiles: {
    'pt-BR': 'pt-BR',
    'en-US': 'en-US',
    'es-ES': 'es-ES'
  } as Record<SupportedLanguage, string>,
  requiredKeys: [
    '{domain}.{service}.app_title',
    '{domain}.{service}.save',
    '{domain}.{service}.cancel',
    '{domain}.{service}.delete',
    '{domain}.{service}.select_language'
  ],
  keyPrefix: '{domain}.{service}'
};

export function getTranslationFilePath(language: SupportedLanguage): string {
  const fileName = LOCALE_CONFIG.languageFiles[language];
  return `${LOCALE_CONFIG.basePath}/${fileName}${LOCALE_CONFIG.fileExtension}`;
}

export function isValidTranslationKey(key: string): boolean {
  return key.startsWith(LOCALE_CONFIG.keyPrefix + '.');
}
```

#### src/app/locale/index.ts

```typescript
export { default as ptBR } from './pt-BR.json';
export { default as enUS } from './en-US.json';
export { default as esES } from './es-ES.json';

export const translations = {
  'pt-BR': () => import('./pt-BR.json'),
  'en-US': () => import('./en-US.json'),
  'es-ES': () => import('./es-ES.json')
};

export const availableLanguages = ['pt-BR', 'en-US', 'es-ES'] as const;
```

### 12. Core Files

#### src/app/core/enums/status.enum.ts

```typescript
export enum Status {
  Active = 'ACTIVE',
  Inactive = 'INACTIVE',
  Deleted = 'DELETED'
}

export const StatusTranslationKeys: Record<string, string> = {
  'ACTIVE': '{domain}.{service}.status_active',
  'INACTIVE': '{domain}.{service}.status_inactive',
  'DELETED': '{domain}.{service}.status_deleted'
};

export const StatusSeverity: Record<string, 'success' | 'warn' | 'danger'> = {
  'ACTIVE': 'success',
  'INACTIVE': 'warn',
  'DELETED': 'danger'
};
```

#### src/app/core/entities/base-entity.interface.ts

```typescript
export type { BaseEntity } from '@seniorsistemas/components-ai';
```

### 13. Environment Files

#### src/environments/environment.default.ts

```typescript
import project from '../../package.json';

export const environment: any = {
  version: project.version,
  name: project.name,
  project: project.project,
  production: false,
  ignorePermissions: false,
};
```

#### src/environments/environment.ts

```typescript
import { environment as def } from './environment.default';
export const environment: any = { ...def };
```

#### src/environments/environment.prod.ts

```typescript
import { environment as def } from './environment.default';
export const environment: any = { ...def, production: true };
```

### 14. Config Files Raiz

#### server.js

```javascript
const os = require('os');
const cli = require('@angular/cli').default;

const HOST = `${os.hostname().toLowerCase()}.interno.senior.com.br`;
cli({ cliArgs: ['serve', '--open', '--host', HOST, '--public-host', HOST, '--port', '0', '--ssl', 'true', ...process.argv.slice(2)] });
```

#### translations.js

```javascript
const fs = require('fs');
const fallback = require('./src/app/locale/pt-BR.json');
const translationPath = 'src/app/locale/fallback.ts';

fs.writeFile(
  `${translationPath}`,
  `export const fallback: any = ${JSON.stringify(fallback, null, 4)};\n`,
  (err) => err && console.error(`Error writing file ${translationPath}: ${err}`)
);
```

#### tailwind.config.js

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{html,ts}"],
  darkMode: ['selector', '.app-dark'],
  plugins: [require('tailwindcss-primeui')],
}
```

#### eslint.config.js

```javascript
const eslint = require("@eslint/js");
const { defineConfig } = require("eslint/config");
const tseslint = require("typescript-eslint");
const angular = require("angular-eslint");

module.exports = defineConfig([
  {
    files: ["**/*.ts"],
    extends: [eslint.configs.recommended, tseslint.configs.recommended, tseslint.configs.stylistic, angular.configs.tsRecommended],
    processor: angular.processInlineTemplates,
    rules: {
      "@angular-eslint/directive-selector": ["error", { type: "attribute", prefix: "app", style: "camelCase" }],
      "@angular-eslint/component-selector": ["error", { type: "element", prefix: "app", style: "kebab-case" }],
    },
  },
  {
    files: ["**/*.html"],
    extends: [angular.configs.templateRecommended, angular.configs.templateAccessibility],
    rules: {},
  }
]);
```

#### .editorconfig

```
root = true

[*]
charset = utf-8
indent_style = space
indent_size = 2
insert_final_newline = true
trim_trailing_whitespace = true

[*.ts]
quote_type = single
ij_typescript_use_double_quotes = false

[*.md]
max_line_length = off
trim_trailing_whitespace = false
```

#### sonar-project.properties

```
sonar.projectKey={service}-frontend
sonar.projectName={service}-frontend
sonar.sourceEncoding=UTF-8

sonar.language=ts
sonar.sources=src
sonar.exclusions=**/*.spec.ts
sonar.tests=src
sonar.test.inclusions=**/*.spec.ts
sonar.coverage.exclusions=src/*,**/*.js,**/*environment*.ts,**/*module.ts,**/*routing.ts,**/*enum.ts,**/*.d.ts

sonar.typescript.lcov.reportPaths=coverage/lcov.info
```

#### .gitlab-ci.yml

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

#### src/index.html

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>{service}-frontend</title>
  <base href="./">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta http-equiv="expires" content="0">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
  <script>window["__Zone_enable_cross_context_check"] = true;</script>
  <script src="https://cdn.senior.com.br/hotkeys/index.js"></script>
</head>
<body>
  <app-root></app-root>
</body>
</html>
```

#### src/styles.scss

```scss
@import "primeicons/primeicons.css";
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
@import '@seniorsistemas/components-ai/src/lib/styles/index.scss';

:root {
  --p-surface-ground: var(--p-surface-50);
  --font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;
}

.app-dark {
  --p-surface-ground: var(--p-surface-950);
}

* { box-sizing: border-box; }
html { font-size: 14px; }
body { margin: 0; font-family: var(--font-family); -webkit-font-smoothing: antialiased; }
html, body { height: 100%; background-color: var(--p-surface-100); color: var(--p-text-color); }

::-webkit-scrollbar { width: 8px; height: 8px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: rgba(148, 163, 184, 0.3); border-radius: 4px; }
::-webkit-scrollbar-thumb:hover { background: rgba(148, 163, 184, 0.5); }

.p-breadcrumb { background: transparent !important; }

.p-datatable {
  th.actions-column, td.actions-column {
    min-width: 100px; max-width: 100px; width: 100px; text-align: center;
  }
}
```

#### src/assets/tailwind.css

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

#### src/static/config.json (mínimo)

```json
{
  "domain": "{domain}",
  "service": "{service}",
  "menu": {
    "children": []
  }
}
```

### 15. Após criar todos os arquivos

1. Execute `npm install`
2. Execute `npm run translations` para gerar o fallback.ts
3. Execute `npm start` para verificar se tudo funciona

O projeto estará limpo, sem features, pronto para começar o desenvolvimento.
