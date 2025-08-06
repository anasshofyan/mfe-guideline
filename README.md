# Micro Frontend Development Guide

_Complete Documentation for Frontend Development Standards_

---

## Table of Contents

1. [Overview](#overview)
2. [Project Structure](#project-structure)
3. [Package Management](#package-management)
4. [Code Guidelines](#code-guidelines)
5. [Naming Conventions](#naming-conventions)
6. [Component Architecture](#component-architecture)
7. [State Management](#state-management)
8. [Routing](#routing)
9. [Testing Strategy](#testing-strategy)
10. [Build & Deployment](#build--deployment)
11. [Development Tools](#development-tools)
12. [Git Workflow](#git-workflow)
13. [Performance Guidelines](#performance-guidelines)
14. [Security Guidelines](#security-guidelines)
15. [Internationalization](#internationalization)
16. [Accessibility](#accessibility)

---

## Overview

This guide establishes comprehensive standards for micro frontend development using modern React ecosystem. It follows RFC 2119 terminology for requirements (MUST, MUST NOT, SHOULD, SHOULD NOT, MAY).

### Key Principles

- **Modularity**: Each feature is self-contained
- **Consistency**: Unified coding standards across teams
- **Maintainability**: Clear structure and documentation
- **Performance**: Optimized bundle sizes and loading
- **Accessibility**: WCAG 2.1 compliance
- **Security**: Secure coding practices

---

## Project Structure

### Root Structure

```
my-microfrontend/
├── .github/                    # GitHub workflows and templates
│   ├── workflows/
│   │   ├── ci.yml
│   │   ├── deploy.yml
│   │   └── pr-checks.yml
│   └── PULL_REQUEST_TEMPLATE.md
├── .vscode/                    # VS Code configuration
│   ├── settings.json
│   ├── extensions.json
│   └── launch.json
├── config/                     # Build and environment configs
│   ├── webpack/
│   │   ├── webpack.common.js
│   │   ├── webpack.dev.js
│   │   ├── webpack.prod.js
│   │   └── webpack.analyze.js
│   ├── jest/
│   │   ├── jest.config.js
│   │   └── setupTests.js
│   └── env/
│       ├── .env.development
│       ├── .env.staging
│       └── .env.production
├── docs/                       # Documentation
│   ├── architecture/
│   ├── api/
│   └── deployment/
├── nginx/                      # Nginx configuration
├── public/                     # Static assets
│   ├── index.html
│   ├── favicon.ico
│   ├── manifest.json
│   └── assets/
├── scripts/                    # Build scripts
│   ├── build.sh
│   ├── deploy.sh
│   └── test.sh
├── src/                        # Source code
├── tools/                      # Development tools
│   ├── generators/
│   └── analyzers/
├── .editorconfig
├── .eslintrc.js
├── .gitignore
├── .prettierrc
├── commitlint.config.js
├── husky.config.js
├── lint-staged.config.js
├── package.json
├── README.md
├── tsconfig.json
└── yarn.lock
```

### Source Structure (`src/`)

```
src/
├── app/                        # App-level configuration
│   ├── App.tsx
│   ├── store.ts
│   ├── router.tsx
│   └── providers/
├── shared/                     # Shared utilities and components
│   ├── components/             # Reusable UI components
│   │   ├── ui/                 # Basic UI components
│   │   │   ├── Button/
│   │   │   │   ├── Button.tsx
│   │   │   │   ├── Button.test.tsx
│   │   │   │   ├── Button.stories.tsx
│   │   │   │   ├── Button.module.scss
│   │   │   │   └── index.ts
│   │   │   └── index.ts
│   │   ├── layout/             # Layout components
│   │   └── forms/              # Form components
│   ├── hooks/                  # Custom React hooks
│   │   ├── useApi.ts
│   │   ├── useDebounce.ts
│   │   └── index.ts
│   ├── utils/                  # Utility functions
│   │   ├── api.ts
│   │   ├── constants.ts
│   │   ├── helpers.ts
│   │   ├── validators.ts
│   │   └── index.ts
│   ├── types/                  # TypeScript type definitions
│   │   ├── api.ts
│   │   ├── common.ts
│   │   └── index.ts
│   ├── services/               # API services
│   │   ├── BaseService.ts
│   │   ├── HttpClient.ts
│   │   └── index.ts
│   └── styles/                 # Global styles
│       ├── globals.scss
│       ├── variables.scss
│       ├── mixins.scss
│       └── themes/
├── features/                   # Feature modules
│   ├── authentication/
│   │   ├── components/
│   │   │   ├── AuthenticationLoginForm/
│   │   │   │   ├── AuthenticationLoginForm.tsx
│   │   │   │   ├── AuthenticationLoginForm.test.tsx
│   │   │   │   ├── AuthenticationLoginForm.stories.tsx
│   │   │   │   ├── AuthenticationLoginForm.module.scss
│   │   │   │   └── index.ts
│   │   │   └── index.ts
│   │   ├── pages/
│   │   │   ├── AuthenticationLoginPage/
│   │   │   │   ├── AuthenticationLoginPage.tsx
│   │   │   │   ├── AuthenticationLoginPage.test.tsx
│   │   │   │   ├── AuthenticationLoginPage.module.scss
│   │   │   │   └── index.ts
│   │   │   └── index.ts
│   │   ├── hooks/
│   │   │   ├── useAuthentication.ts
│   │   │   └── index.ts
│   │   ├── services/
│   │   │   ├── AuthenticationService.ts
│   │   │   └── index.ts
│   │   ├── store/
│   │   │   ├── authentication.slice.ts
│   │   │   ├── authentication.asyncActions.ts
│   │   │   ├── authentication.selectors.ts
│   │   │   └── index.ts
│   │   ├── types/
│   │   │   ├── authentication.types.ts
│   │   │   └── index.ts
│   │   ├── utils/
│   │   │   ├── auth.utils.ts
│   │   │   └── index.ts
│   │   ├── locales/
│   │   │   ├── en.ts
│   │   │   ├── id.ts
│   │   │   └── index.ts
│   │   ├── constants/
│   │   │   ├── auth.constants.ts
│   │   │   └── index.ts
│   │   └── index.ts
│   └── userProfile/
├── assets/                     # Static assets
│   ├── images/
│   ├── icons/
│   ├── fonts/
│   └── videos/
└── bootstrap.tsx               # Application entry point
```

---

## Package Management

### Package.json Structure

```json
{
  "name": "@company/microfrontend-name",
  "version": "1.0.0",
  "description": "Micro frontend application",
  "scripts": {
    "dev": "webpack serve --mode development",
    "build": "webpack --mode production",
    "build:analyze": "webpack --mode production --env analyze",
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "test:e2e": "cypress run",
    "test:e2e:open": "cypress open",
    "lint": "eslint src --ext .ts,.tsx",
    "lint:fix": "eslint src --ext .ts,.tsx --fix",
    "type-check": "tsc --noEmit",
    "format": "prettier --write src/**/*.{ts,tsx,scss}",
    "storybook": "start-storybook -p 6006",
    "build-storybook": "build-storybook",
    "prepare": "husky install",
    "generate": "plop",
    "clean": "rimraf dist",
    "precommit": "lint-staged"
  }
}
```

### Dependencies Guidelines

- **Runtime Dependencies**: Only production-required packages
- **Dev Dependencies**: All development tools and testing libraries
- **Peer Dependencies**: Shared dependencies with host application
- **Exact Versions**: Use exact versions for critical dependencies

### Recommended Dependencies

```json
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.8.0",
    "@reduxjs/toolkit": "^1.9.0",
    "react-redux": "^8.0.0",
    "axios": "^1.3.0",
    "react-hook-form": "^7.43.0",
    "zod": "^3.20.0",
    "date-fns": "^2.29.0",
    "lodash": "^4.17.0",
    "classnames": "^2.3.0"
  },
  "devDependencies": {
    "@types/react": "^18.0.0",
    "@types/react-dom": "^18.0.0",
    "typescript": "^4.9.0",
    "webpack": "^5.75.0",
    "webpack-cli": "^5.0.0",
    "webpack-dev-server": "^4.11.0",
    "@babel/core": "^7.20.0",
    "@babel/preset-react": "^7.18.0",
    "@babel/preset-typescript": "^7.18.0",
    "babel-loader": "^9.1.0",
    "css-loader": "^6.7.0",
    "sass-loader": "^13.2.0",
    "style-loader": "^3.3.0",
    "html-webpack-plugin": "^5.5.0",
    "mini-css-extract-plugin": "^2.7.0",
    "eslint": "^8.33.0",
    "@typescript-eslint/eslint-plugin": "^5.50.0",
    "@typescript-eslint/parser": "^5.50.0",
    "eslint-plugin-react": "^7.32.0",
    "eslint-plugin-react-hooks": "^4.6.0",
    "prettier": "^2.8.0",
    "jest": "^29.4.0",
    "@testing-library/react": "^13.4.0",
    "@testing-library/jest-dom": "^5.16.0",
    "@testing-library/user-event": "^14.4.0",
    "cypress": "^12.5.0",
    "@storybook/react": "^6.5.0",
    "husky": "^8.0.0",
    "lint-staged": "^13.1.0",
    "commitlint": "^17.4.0",
    "plop": "^3.1.0"
  }
}
```

---

## Code Guidelines

### ESLint Configuration

#### .eslintrc.js

```javascript
module.exports = {
  root: true,
  env: {
    browser: true,
    es2021: true,
    node: true,
    jest: true,
  },
  extends: [
    "eslint:recommended",
    "@typescript-eslint/recommended",
    "plugin:react/recommended",
    "plugin:react-hooks/recommended",
    "plugin:jsx-a11y/recommended",
    "plugin:import/recommended",
    "plugin:import/typescript",
    "prettier", // Must be last to override other configs
  ],
  parser: "@typescript-eslint/parser",
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: "latest",
    sourceType: "module",
    project: "./tsconfig.json",
  },
  plugins: ["react", "@typescript-eslint", "react-hooks", "jsx-a11y", "import"],
  settings: {
    react: {
      version: "detect",
    },
    "import/resolver": {
      typescript: {
        alwaysTryTypes: true,
        project: "./tsconfig.json",
      },
    },
  },
  rules: {
    // React Rules
    "react/react-in-jsx-scope": "off", // Not needed in React 17+
    "react/prop-types": "off", // Using TypeScript instead
    "react/display-name": "error",
    "react/jsx-key": "error",
    "react/jsx-no-duplicate-props": "error",
    "react/jsx-no-undef": "error",
    "react/jsx-uses-react": "off", // Not needed in React 17+
    "react/jsx-uses-vars": "error",
    "react/no-danger": "warn",
    "react/no-deprecated": "error",
    "react/no-direct-mutation-state": "error",
    "react/no-unknown-property": "error",
    "react/self-closing-comp": "error",

    // React Hooks Rules
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn",

    // Component Naming Rules
    "react/jsx-pascal-case": [
      "error",
      {
        allowAllCaps: false,
        allowNamespace: false,
      },
    ],

    // Props Destructuring Rule
    "react/destructuring-assignment": [
      "error",
      "always",
      {
        ignoreClassFields: true,
        destructureInSignature: "always",
      },
    ],

    // TypeScript Rules
    "@typescript-eslint/no-unused-vars": [
      "error",
      {
        argsIgnorePattern: "^_",
        varsIgnorePattern: "^_",
      },
    ],
    "@typescript-eslint/no-explicit-any": "warn",
    "@typescript-eslint/explicit-function-return-type": "off",
    "@typescript-eslint/explicit-module-boundary-types": "off",
    "@typescript-eslint/no-non-null-assertion": "warn",
    "@typescript-eslint/prefer-const": "error",
    "@typescript-eslint/no-var-requires": "error",

    // Import/Export Rules - ENFORCES CODE STRUCTURE ORDER
    "import/order": [
      "error",
      {
        groups: [
          "builtin", // Node.js built-in modules
          "external", // External libraries (React, lodash, etc.)
          "internal", // Internal shared modules (@/shared)
          "parent", // Parent directory imports (../)
          "sibling", // Same directory imports (./)
          "index", // Index file imports
        ],
        pathGroups: [
          {
            pattern: "react",
            group: "external",
            position: "before",
          },
          {
            pattern: "react-*",
            group: "external",
            position: "before",
          },
          {
            pattern: "@/**",
            group: "internal",
            position: "before",
          },
          {
            pattern: "./**/*.scss",
            group: "index",
            position: "after",
          },
        ],
        pathGroupsExcludedImportTypes: ["react"],
        "newlines-between": "always",
        alphabetize: {
          order: "asc",
          caseInsensitive: true,
        },
      },
    ],
    "import/no-unresolved": "error",
    "import/no-default-export": "warn", // Prefer named exports
    "import/prefer-default-export": "off",
    "import/no-duplicates": "error",
    "import/no-cycle": "error",

    // General Rules
    "no-console": ["warn", { allow: ["warn", "error"] }],
    "no-debugger": "error",
    "no-alert": "error",
    "no-unused-vars": "off", // Use TypeScript version instead
    "prefer-const": "error",
    "no-var": "error",
    "object-shorthand": "error",
    "prefer-template": "error",

    // Accessibility Rules
    "jsx-a11y/alt-text": "error",
    "jsx-a11y/anchor-has-content": "error",
    "jsx-a11y/click-events-have-key-events": "error",
    "jsx-a11y/no-static-element-interactions": "error",

    // Custom Rules for Component Naming Standards
    "no-restricted-syntax": [
      "error",
      {
        selector: "ExportDefaultDeclaration",
        message:
          "Prefer named exports over default exports for better discoverability.",
      },
      {
        selector:
          'VariableDeclarator[id.name=/Component$/][init.type="ArrowFunctionExpression"]:not([id.name=/^[A-Z][a-zA-Z]*Component[A-Z][a-zA-Z]*$/])',
        message:
          "Component names must follow {FeatureName}Component{Action} pattern (e.g., UserProfileComponentEdit)",
      },
      {
        selector:
          'VariableDeclarator[id.name=/Page$/][init.type="ArrowFunctionExpression"]:not([id.name=/^[A-Z][a-zA-Z]*Page[A-Z][a-zA-Z]*$/])',
        message:
          "Page names must follow {FeatureName}Page{Action} pattern (e.g., UserProfilePageEdit)",
      },
    ],
  },
  overrides: [
    {
      // Allow default exports for specific file patterns
      files: ["src/pages/**/*.tsx", "src/app/**/*.tsx", "*.stories.tsx"],
      rules: {
        "import/no-default-export": "off",
        "no-restricted-syntax": "off",
      },
    },
    {
      // Relax rules for test files
      files: ["**/*.test.tsx", "**/*.test.ts", "**/__tests__/**/*"],
      rules: {
        "@typescript-eslint/no-explicit-any": "off",
        "no-console": "off",
      },
    },
  ],
};
```

### Prettier Configuration

#### .prettierrc

```json
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "quoteProps": "as-needed",
  "bracketSpacing": true,
  "bracketSameLine": false,
  "arrowParens": "always",
  "endOfLine": "lf",
  "jsxSingleQuote": true,
  "overrides": [
    {
      "files": "*.scss",
      "options": {
        "singleQuote": false
      }
    }
  ]
}
```

#### .prettierignore

```
# Dependencies
node_modules/
dist/
build/

# Generated files
coverage/
*.log
.env*

# Config files
package-lock.json
yarn.lock
```

### Husky Configuration

#### husky.config.js

```javascript
module.exports = {
  hooks: {
    "pre-commit": "lint-staged",
    "commit-msg": "commitlint -E HUSKY_GIT_PARAMS",
    "pre-push": "npm run type-check && npm run test:ci",
  },
};
```

### Lint-staged Configuration

#### lint-staged.config.js

```javascript
module.exports = {
  // TypeScript and JavaScript files
  "*.{ts,tsx,js,jsx}": [
    // 1. ESLint check and fix
    "eslint --fix --max-warnings=0",
    // 2. Prettier format
    "prettier --write",
    // 3. Type check (only for TS files)
    () => "tsc --noEmit",
    // 4. Run tests related to staged files
    "jest --findRelatedTests --passWithNoTests",
  ],

  // SCSS and CSS files
  "*.{scss,css}": [
    // 1. Stylelint check and fix
    "stylelint --fix",
    // 2. Prettier format
    "prettier --write",
  ],

  // JSON files
  "*.json": ["prettier --write"],

  // Markdown files
  "*.md": [
    "prettier --write",
    // Optional: Check markdown links
    "markdown-link-check",
  ],

  // Package.json specific
  "package.json": [
    // Sort package.json
    "sort-package-json",
    "prettier --write",
  ],
};
```

### Stylelint Configuration

#### .stylelintrc.js

````javascript
module.exports = {
  extends: [
    'stylelint-config-standard-scss',
    'stylelint-config-prettier-scss',
  ],
  plugins: [
    'stylelint-scss',
    'stylelint-order',
  ],
  rules: {
    // SCSS specific rules
    'scss/at-rule-no-unknown': true,
    'scss/selector-no-redundant-nesting-selector': true,
    'scss/no-duplicate-dollar-variables': true,

    // BEM methodology enforcement
    'selector-class-pattern': [
      '^[a-z]([a-z0-9-]+)?(__([a-z0-9]+-?)+)?(--([a-z0-9]+-?)+){0,2}# Micro Frontend Development Guide
*Complete Documentation for Frontend Development Standards*

---

### TypeScript Configuration
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "lib": ["DOM", "DOM.Iterable", "ES6"],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",
    "baseUrl": "src",
    "paths": {
      "@/*": ["*"],
      "@/shared/*": ["shared/*"],
      "@/features/*": ["features/*"],
      "@/assets/*": ["assets/*"]
    }
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist", "build"]
}
````

### Import/Export Guidelines

#### ✅ Preferred Patterns

```typescript
// Named exports (preferred)
export const UserProfileComponent = () => {
  /* */
};
export const validateEmail = (email: string) => {
  /* */
};

// Re-exports in index files
export { UserProfileComponent } from "./UserProfileComponent";
export { validateEmail } from "./utils";

// Import patterns
import { UserProfileComponent } from "@/features/userProfile";
import { Button, Input } from "@/shared/components/ui";
import { validateEmail } from "@/shared/utils";
```

#### ❌ Avoid These Patterns

```typescript
// Default exports (avoid except for code splitting)
export default UserProfileComponent;

// Wildcard imports (avoid)
import * as utils from "./utils";

// Deep imports (avoid)
import { Button } from "@/shared/components/ui/Button/Button";
```

### Code Style Rules

#### Variables and Functions

```typescript
// ✅ Good
const isAuthenticated = true;
const userProfile = { name: "John", email: "john@example.com" };
const calculateTotalPrice = (items: CartItem[]) => {
  /* */
};

// ❌ Bad
const IsAuthenticated = true;
const user_profile = { name: "John", email: "john@example.com" };
const calculate_total_price = (items: CartItem[]) => {
  /* */
};
```

#### Component Structure

```typescript
// ✅ Recommended structure
import React, { useState, useEffect, useCallback } from "react";
import { useSelector, useDispatch } from "react-redux";
import { Button, Input } from "@/shared/components/ui";
import { validateEmail } from "@/shared/utils";
import { UserProfileService } from "../services";
import styles from "./UserProfileComponentEdit.module.scss";

interface UserProfileComponentEditProps {
  userId: string;
  onSave?: (profile: UserProfile) => void;
  className?: string;
}

export const UserProfileComponentEdit: React.FC<
  UserProfileComponentEditProps
> = ({ userId, onSave, className }) => {
  // State declarations
  const [profile, setProfile] = useState<UserProfile | null>(null);
  const [isLoading, setIsLoading] = useState(false);
  const [errors, setErrors] = useState<Record<string, string>>({});

  // Redux selectors
  const currentUser = useSelector(selectCurrentUser);
  const dispatch = useDispatch();

  // Callbacks
  const handleSave = useCallback(async () => {
    if (!profile) return;

    setIsLoading(true);
    try {
      await UserProfileService.update(userId, profile);
      onSave?.(profile);
    } catch (error) {
      setErrors({ general: "Failed to save profile" });
    } finally {
      setIsLoading(false);
    }
  }, [profile, userId, onSave]);

  // Effects
  useEffect(() => {
    const loadProfile = async () => {
      try {
        const data = await UserProfileService.getById(userId);
        setProfile(data);
      } catch (error) {
        setErrors({ general: "Failed to load profile" });
      }
    };

    loadProfile();
  }, [userId]);

  // Early returns
  if (!profile) {
    return <div className={styles.loading}>Loading...</div>;
  }

  // Render
  return (
    <div className={`${styles.container} ${className || ""}`}>
      <form onSubmit={handleSave}>
        <Input
          label="Email"
          value={profile.email}
          onChange={(value) => setProfile({ ...profile, email: value })}
          error={errors.email}
          required
        />
        <Button
          type="submit"
          loading={isLoading}
          disabled={!validateEmail(profile.email)}
        >
          Save Profile
        </Button>
      </form>
    </div>
  );
};
```

---

## Naming Conventions

### File and Directory Naming

- **Components**: `PascalCase` with descriptive names
- **Files**: Match component name exactly
- **Directories**: `PascalCase` for components, `camelCase` for utilities
- **Test files**: `ComponentName.test.tsx`
- **Story files**: `ComponentName.stories.tsx`
- **Style files**: `ComponentName.module.scss`

### Component Naming Patterns

```typescript
// Components
{FeatureName}Component{Action} // UserProfileComponentEdit
{FeatureName}Component{Type}   // UserProfileComponentCard

// Pages
{FeatureName}Page{Action}      // UserProfilePageEdit
{FeatureName}Page{Type}        // UserProfilePageList

// Services
{FeatureName}Service           // UserProfileService
{FeatureName}ApiService        // UserProfileApiService

// Store
{featureName}Slice             // userProfileSlice
{featureName}AsyncActions      // userProfileAsyncActions
{featureName}Selectors         // userProfileSelectors

// Hooks
use{FeatureName}{Action}       // useUserProfileForm
use{ActionName}                // useDebounce

// Utils
{actionName}{Target}           // validateEmail
{target}{Action}               // emailValidator
```

### CSS/SCSS Naming (BEM)

```scss
// Block__Element--Modifier pattern
.UserProfileComponent {
  &__header {
    display: flex;
    align-items: center;

    &--centered {
      justify-content: center;
    }
  }

  &__avatar {
    width: 48px;
    height: 48px;
    border-radius: 50%;

    &--large {
      width: 96px;
      height: 96px;
    }
  }

  &__content {
    padding: 1rem;

    &--compact {
      padding: 0.5rem;
    }
  }
}
```

---

## Component Architecture

### Component Categories

#### 1. UI Components (Dumb/Presentational)

```typescript
// Pure presentational component
interface ButtonProps {
  children: React.ReactNode;
  variant?: "primary" | "secondary" | "danger";
  size?: "small" | "medium" | "large";
  loading?: boolean;
  disabled?: boolean;
  onClick?: () => void;
}

export const Button: React.FC<ButtonProps> = ({
  children,
  variant = "primary",
  size = "medium",
  loading = false,
  disabled = false,
  onClick,
}) => {
  return (
    <button
      className={`btn btn--${variant} btn--${size}`}
      disabled={disabled || loading}
      onClick={onClick}
    >
      {loading ? <Spinner /> : children}
    </button>
  );
};
```

#### 2. Feature Components (Smart/Container)

```typescript
// Connected to state and business logic
export const UserProfileComponentEdit: React.FC<Props> = ({ userId }) => {
  const dispatch = useDispatch();
  const { profile, loading, error } = useSelector(selectUserProfile);

  const handleSubmit = useCallback(
    async (data: UserProfileData) => {
      await dispatch(updateUserProfile({ userId, data }));
    },
    [dispatch, userId]
  );

  return (
    <UserProfileForm
      initialData={profile}
      loading={loading}
      error={error}
      onSubmit={handleSubmit}
    />
  );
};
```

#### 3. Layout Components

```typescript
export const PageLayout: React.FC<{
  children: React.ReactNode;
  sidebar?: React.ReactNode;
  header?: React.ReactNode;
}> = ({ children, sidebar, header }) => {
  return (
    <div className="page-layout">
      {header && <header className="page-layout__header">{header}</header>}
      <div className="page-layout__body">
        {sidebar && <aside className="page-layout__sidebar">{sidebar}</aside>}
        <main className="page-layout__content">{children}</main>
      </div>
    </div>
  );
};
```

### Component Composition Patterns

#### Higher-Order Components (HOCs)

```typescript
// withAuth HOC
export const withAuth = <P extends object>(
  Component: React.ComponentType<P>
) => {
  return (props: P) => {
    const { isAuthenticated, user } = useAuth();

    if (!isAuthenticated) {
      return <Navigate to="/login" />;
    }

    return <Component {...props} user={user} />;
  };
};

// Usage
export const ProtectedUserProfile = withAuth(UserProfileComponent);
```

#### Custom Hooks Pattern

```typescript
// useUserProfile hook
export const useUserProfile = (userId: string) => {
  const [profile, setProfile] = useState<UserProfile | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  const updateProfile = useCallback(
    async (data: Partial<UserProfile>) => {
      try {
        setLoading(true);
        const updated = await UserProfileService.update(userId, data);
        setProfile(updated);
        return updated;
      } catch (err) {
        setError(err.message);
        throw err;
      } finally {
        setLoading(false);
      }
    },
    [userId]
  );

  useEffect(() => {
    const loadProfile = async () => {
      try {
        setLoading(true);
        const data = await UserProfileService.getById(userId);
        setProfile(data);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    loadProfile();
  }, [userId]);

  return { profile, loading, error, updateProfile };
};
```

---

## State Management

### Redux Toolkit Structure

#### Slice Definition

```typescript
// userProfileSlice.ts
import { createSlice, createAsyncThunk, PayloadAction } from "@reduxjs/toolkit";
import { UserProfileService } from "../services";

export interface UserProfileState {
  profiles: Record<string, UserProfile>;
  loading: boolean;
  error: string | null;
  currentProfileId: string | null;
}

const initialState: UserProfileState = {
  profiles: {},
  loading: false,
  error: null,
  currentProfileId: null,
};

export const fetchUserProfile = createAsyncThunk(
  "userProfile/fetch",
  async (userId: string, { rejectWithValue }) => {
    try {
      const profile = await UserProfileService.getById(userId);
      return { userId, profile };
    } catch (error) {
      return rejectWithValue(error.message);
    }
  }
);

export const updateUserProfile = createAsyncThunk(
  "userProfile/update",
  async ({ userId, data }: { userId: string; data: Partial<UserProfile> }) => {
    const profile = await UserProfileService.update(userId, data);
    return { userId, profile };
  }
);

const userProfileSlice = createSlice({
  name: "userProfile",
  initialState,
  reducers: {
    setCurrentProfile: (state, action: PayloadAction<string>) => {
      state.currentProfileId = action.payload;
    },
    clearError: (state) => {
      state.error = null;
    },
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchUserProfile.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(fetchUserProfile.fulfilled, (state, action) => {
        state.loading = false;
        state.profiles[action.payload.userId] = action.payload.profile;
      })
      .addCase(fetchUserProfile.rejected, (state, action) => {
        state.loading = false;
        state.error = action.payload as string;
      })
      .addCase(updateUserProfile.fulfilled, (state, action) => {
        state.profiles[action.payload.userId] = action.payload.profile;
      });
  },
});

export const { setCurrentProfile, clearError } = userProfileSlice.actions;
export default userProfileSlice.reducer;
```

#### Selectors

```typescript
// userProfileSelectors.ts
import { createSelector } from "@reduxjs/toolkit";
import { RootState } from "@/app/store";

const selectUserProfileState = (state: RootState) => state.userProfile;

export const selectUserProfiles = createSelector(
  [selectUserProfileState],
  (userProfile) => userProfile.profiles
);

export const selectCurrentProfile = createSelector(
  [selectUserProfileState, selectUserProfiles],
  (userProfile, profiles) =>
    userProfile.currentProfileId ? profiles[userProfile.currentProfileId] : null
);

export const selectUserProfileById = (userId: string) =>
  createSelector([selectUserProfiles], (profiles) => profiles[userId]);

export const selectUserProfileLoading = createSelector(
  [selectUserProfileState],
  (userProfile) => userProfile.loading
);

export const selectUserProfileError = createSelector(
  [selectUserProfileState],
  (userProfile) => userProfile.error
);
```

### Store Configuration

```typescript
// store.ts
import { configureStore } from "@reduxjs/toolkit";
import { setupListeners } from "@reduxjs/toolkit/query";
import userProfileReducer from "@/features/userProfile/store/userProfileSlice";
import authenticationReducer from "@/features/authentication/store/authenticationSlice";

export const store = configureStore({
  reducer: {
    userProfile: userProfileReducer,
    authentication: authenticationReducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      serializableCheck: {
        ignoredActions: ["persist/PERSIST", "persist/REHYDRATE"],
      },
    }),
  devTools: process.env.NODE_ENV !== "production",
});

setupListeners(store.dispatch);

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

---

## Routing & Path Guidelines

### URL Structure Standards

The application follows a hierarchical URL structure that reflects the business domain and feature organization:

```
/{service}/{module}/{tab}/{action}/{id?}
```

#### Path Components Explanation

1. **Service**: High-level business domain (kebab-case)
2. **Module**: Specific feature within the service (kebab-case)
3. **Tab**: Data view or categorization (kebab-case)
4. **Action**: Operation being performed (kebab-case)
5. **ID**: Resource identifier (optional, dynamic parameter)

#### Path Examples

```text
#List pages or To be Approved
/fund-accounting/portfolio-contract
/fund-accounting/investment-report
/user-management/user-profile

#Detail pages
/fund-accounting/portfolio-contract/detail/{id}
/fund-accounting/investment-report/detail/{id}
/user-management/user-profile/detail/{id}
#Detail for To be Approved
/fund-accounting/portfolio-contract/list-tobe-approved/detail/{id}
/fund-accounting/investment-report/list-tobe-approved/detail/{id}
/user-management/user-profile/list-tobe-approved/detail/{id}

#Action pages
/fund-accounting/portfolio-contract/{This is optional: if there are more than two tabs, each tab (e.g. Tab 1, Tab2)}/create
/fund-accounting/portfolio-contract/{This is optional: if there are more than two tabs, each tab (e.g. Tab 1, Tab2)}/edit/{id}
/fund-accounting/portfolio-contract/{This is optional: if there are more than two tabs, each tab (e.g. Tab 1, Tab2)}/duplicate/{id}

#Complex nested paths
/fund-accounting/portfolio-contract/list-tobe-approved/detail/{contractId}/documents/{documentId}
/user-management/user-profile/detail/{userId}/permissions/edit
```

### Route Configuration

#### Route Definition Structure

```typescript
// routes/fundAccounting.routes.ts
export const fundAccountingRoutes = [
  {
    path: "/fund-accounting",
    element: <FundAccountingLayout />,
    children: [
      {
        path: "portfolio-contract",
        children: [
          {
            path: "list",
            element: <PortfolioContractPageList />,
          },
          {
            path: "list/detail/:id",
            element: <PortfolioContractPageDetail />,
          },
          {
            path: "list/create",
            element: <PortfolioContractPageCreate />,
          },
          {
            path: "list/edit/:id",
            element: <PortfolioContractPageEdit />,
          },
          {
            path: "list-tobe-approved",
            element: <PortfolioContractPageTobeApproved />,
          },
          {
            path: "list-tobe-approved/detail/:id",
            element: <PortfolioContractPageTobeApprovedDetail />,
          },
          {
            path: "list-approved",
            element: <PortfolioContractPageApproved />,
          },
          {
            path: "list-approved/detail/:id",
            element: <PortfolioContractPageApprovedDetail />,
          },
        ],
      },
    ],
  },
];
```

#### Route Constants

```typescript
// constants/routes.ts
export const ROUTES = {
  FUND_ACCOUNTING: {
    PORTFOLIO_CONTRACT: {
      LIST: "/fund-accounting/portfolio-contract/list",
      DETAIL: (id: string) =>
        `/fund-accounting/portfolio-contract/list/detail/${id}`,
      CREATE: "/fund-accounting/portfolio-contract/list/create",
      EDIT: (id: string) =>
        `/fund-accounting/portfolio-contract/list/edit/${id}`,
      TOBE_APPROVED: {
        LIST: "/fund-accounting/portfolio-contract/list-tobe-approved",
        DETAIL: (id: string) =>
          `/fund-accounting/portfolio-contract/list-tobe-approved/detail/${id}`,
      },
      APPROVED: {
        LIST: "/fund-accounting/portfolio-contract/list-approved",
        DETAIL: (id: string) =>
          `/fund-accounting/portfolio-contract/list-approved/detail/${id}`,
      },
    },
  },
  USER_MANAGEMENT: {
    USER_PROFILE: {
      LIST: "/user-management/user-profile/list",
      DETAIL: (id: string) => `/user-management/user-profile/list/detail/${id}`,
      CREATE: "/user-management/user-profile/list/create",
      EDIT: (id: string) => `/user-management/user-profile/list/edit/${id}`,
    },
  },
} as const;
```

#### Navigation Helpers

```typescript
// hooks/useNavigation.ts
import { useNavigate } from "react-router-dom";
import { ROUTES } from "@/constants/routes";

export const useAppNavigation = () => {
  const navigate = useNavigate();

  return {
    goToPortfolioContractList: () =>
      navigate(ROUTES.FUND_ACCOUNTING.PORTFOLIO_CONTRACT.LIST),
    goToPortfolioContractDetail: (id: string) =>
      navigate(ROUTES.FUND_ACCOUNTING.PORTFOLIO_CONTRACT.DETAIL(id)),
    goToPortfolioContractTobeApprovedDetail: (id: string) =>
      navigate(
        ROUTES.FUND_ACCOUNTING.PORTFOLIO_CONTRACT.TOBE_APPROVED.DETAIL(id)
      ),
    goBack: () => navigate(-1),
  };
};
```

### Path Naming Conventions

#### ✅ Correct Path Patterns

```typescript
// Service and module names: kebab-case
/fund-accounting/filoooprt -
  contract / list / user -
  management / user -
  profile / list / reporting -
  service / financial -
  report /
    list /
    // Tab variations: descriptive, kebab-case
    fund -
  accounting / portfolio -
  contract / list -
  pending / fund -
  accounting / portfolio -
  contract / list -
  tobe -
  approved / fund -
  accounting / portfolio -
  contract / list -
  approved / fund -
  accounting / portfolio -
  contract / list -
  rejected /
    // Actions: clear, kebab-case
    fund -
  accounting / portfolio -
  contract / list / create / fund -
  accounting / portfolio -
  contract / list / edit / { id } / fund -
  accounting / portfolio -
  contract / list / duplicate / { id } / fund -
  accounting / portfolio -
  contract / list / archive / { id };
```

#### ❌ Incorrect Path Patterns

```typescript
// Avoid camelCase in URLs
/fundAccounting/portfolioContract/list ❌
/fund_accounting/portfolio_contract/list ❌

// Avoid unclear abbreviations
/fa/pc/list ❌
/fund-acc/port-cont/list ❌

// Avoid inconsistent patterns
/fund-accounting/portfolio-contract/pending-list ❌
/fund-accounting/portfolio-contract/list_pending ❌
```

---

## React Component Code Structure

### Component Code Organization Order

Every React component MUST follow this specific order for consistency and readability:

```typescript
// 1. IMPORTS - External libraries (React, third-party)
import React, { useState, useEffect, useCallback, useMemo } from "react";
import { useNavigate, useParams } from "react-router-dom";
import { useDispatch, useSelector } from "react-redux";
import { useForm, Controller } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import moment from "moment";
import _ from "lodash";

// 2. IMPORTS - Internal shared utilities
import { Button, Input, Modal } from "@/shared/components/ui";
import { useDebounce, useApi } from "@/shared/hooks";
import { formatCurrency, validateEmail } from "@/shared/utils";
import { ApiResponse, UserProfile } from "@/shared/types";

// 3. IMPORTS - Internal absolute (same folder/feature)
import { PortfolioContractService } from "./services";
import { portfolioContractSchema } from "./validation";
import { PortfolioContractComponentForm } from "./components";

// 4. IMPORTS - Styles (always last)
import styles from "./PortfolioContractPageEdit.module.scss";

// 5. CONSTANTS - Component-level constants
const FORM_VALIDATION_RULES = {
  contractNumber: { required: true, minLength: 5 },
  contractValue: { required: true, min: 1000 },
} as const;

const STATUS_OPTIONS = [
  { value: "draft", label: "Draft" },
  { value: "pending", label: "Pending Approval" },
  { value: "approved", label: "Approved" },
] as const;

const DEFAULT_FORM_VALUES = {
  contractNumber: "",
  contractValue: 0,
  status: "draft",
} as const;

// Component interface
interface PortfolioContractPageEditProps {
  contractId?: string;
  onSave?: (contract: PortfolioContract) => void;
  onCancel?: () => void;
  className?: string;
}

export const PortfolioContractPageEdit: React.FC<
  PortfolioContractPageEditProps
> = ({ contractId, onSave, onCancel, className }) => {
  // 6. HOOKS - i18n, Redux, Selectors, Router, Form (in this order)

  // 6.1 i18n hooks
  const { t } = useTranslation("portfolioContract");

  // 6.2 Redux hooks
  const dispatch = useDispatch();
  const currentUser = useSelector(selectCurrentUser);
  const isLoading = useSelector(selectPortfolioContractLoading);
  const error = useSelector(selectPortfolioContractError);

  // 6.3 Router hooks
  const navigate = useNavigate();
  const { id: paramId } = useParams<{ id: string }>();
  const contractIdToUse = contractId || paramId;

  // 6.4 Form hooks
  const {
    control,
    handleSubmit,
    reset,
    watch,
    formState: { errors, isDirty, isValid },
  } = useForm<PortfolioContractFormData>({
    resolver: zodResolver(portfolioContractSchema),
    defaultValues: DEFAULT_FORM_VALUES,
  });

  // 6.5 Custom hooks
  const { data: contractData, loading: contractLoading } = useApi(
    () => PortfolioContractService.getById(contractIdToUse!),
    [contractIdToUse]
  );
  const debouncedContractValue = useDebounce(watch("contractValue"), 500);

  // 7. LOCAL STATE - useState declarations
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [showConfirmModal, setShowConfirmModal] = useState(false);
  const [validationErrors, setValidationErrors] = useState<
    Record<string, string>
  >({});
  const [lastSavedAt, setLastSavedAt] = useState<Date | null>(null);

  // 8. DERIVED/COMPUTED VALUES - useMemo, calculations
  const isEditMode = useMemo(() => Boolean(contractIdToUse), [contractIdToUse]);

  const formattedContractValue = useMemo(
    () => formatCurrency(debouncedContractValue),
    [debouncedContractValue]
  );

  const canSave = useMemo(
    () => isValid && isDirty && !isSubmitting && !contractLoading,
    [isValid, isDirty, isSubmitting, contractLoading]
  );

  const pageTitle = useMemo(
    () => (isEditMode ? t("editContract") : t("createContract")),
    [isEditMode, t]
  );

  // 9. EFFECTS - useEffect, useLayoutEffect (grouped by purpose)

  // 9.1 Data loading effects
  useEffect(() => {
    if (contractData) {
      reset(contractData);
      setLastSavedAt(new Date());
    }
  }, [contractData, reset]);

  // 9.2 Validation effects
  useEffect(() => {
    if (debouncedContractValue > 0) {
      setValidationErrors((prev) => {
        const { contractValue, ...rest } = prev;
        return rest;
      });
    }
  }, [debouncedContractValue]);

  // 9.3 Cleanup effects
  useEffect(() => {
    return () => {
      // Cleanup any pending operations
      setIsSubmitting(false);
      setShowConfirmModal(false);
    };
  }, []);

  // 10. EVENT HANDLERS - All event handling functions
  const handleFormSubmit = useCallback(
    async (data: PortfolioContractFormData) => {
      setIsSubmitting(true);
      setValidationErrors({});

      try {
        let result: PortfolioContract;

        if (isEditMode) {
          result = await PortfolioContractService.update(
            contractIdToUse!,
            data
          );
        } else {
          result = await PortfolioContractService.create(data);
        }

        setLastSavedAt(new Date());
        onSave?.(result);

        if (!isEditMode) {
          navigate(
            `/fund-accounting/portfolio-contract/list/detail/${result.id}`
          );
        }
      } catch (error) {
        if (error.validation) {
          setValidationErrors(error.validation);
        } else {
          setValidationErrors({ general: error.message });
        }
      } finally {
        setIsSubmitting(false);
      }
    },
    [isEditMode, contractIdToUse, onSave, navigate]
  );

  const handleCancel = useCallback(() => {
    if (isDirty) {
      setShowConfirmModal(true);
    } else {
      onCancel?.();
      navigate(-1);
    }
  }, [isDirty, onCancel, navigate]);

  const handleConfirmCancel = useCallback(() => {
    setShowConfirmModal(false);
    onCancel?.();
    navigate(-1);
  }, [onCancel, navigate]);

  const handleFieldChange = useCallback((field: string, value: any) => {
    // Custom field change logic if needed
    console.log(`Field ${field} changed to:`, value);
  }, []);

  // 11. CONDITIONAL RENDERING - Early returns
  if (contractLoading) {
    return (
      <div className={styles.loading} data-testid="loading-spinner">
        <span>{t("loadingContract")}</span>
      </div>
    );
  }

  if (error) {
    return (
      <div className={styles.error} data-testid="error-message">
        <p>{error}</p>
        <Button onClick={() => navigate(-1)}>{t("goBack")}</Button>
      </div>
    );
  }

  // 12. MAIN RENDER - Component JSX
  return (
    <div className={`${styles.container} ${className || ""}`}>
      <header className={styles.header}>
        <h1 className={styles.title}>{pageTitle}</h1>
        {lastSavedAt && (
          <span className={styles.lastSaved}>
            {t("lastSaved")}: {moment(lastSavedAt).format("HH:mm:ss")}
          </span>
        )}
      </header>

      <form onSubmit={handleSubmit(handleFormSubmit)} className={styles.form}>
        <PortfolioContractComponentForm
          control={control}
          errors={errors}
          validationErrors={validationErrors}
          onFieldChange={handleFieldChange}
          isEditMode={isEditMode}
        />

        <div className={styles.formattedValue}>
          {t("formattedValue")}: {formattedContractValue}
        </div>

        <footer className={styles.actions}>
          <Button
            type="button"
            variant="secondary"
            onClick={handleCancel}
            disabled={isSubmitting}
          >
            {t("cancel")}
          </Button>
          <Button
            type="submit"
            variant="primary"
            loading={isSubmitting}
            disabled={!canSave}
          >
            {isEditMode ? t("updateContract") : t("createContract")}
          </Button>
        </footer>
      </form>

      <Modal
        isOpen={showConfirmModal}
        onClose={() => setShowConfirmModal(false)}
        title={t("confirmCancel")}
      >
        <p>{t("unsavedChangesWarning")}</p>
        <div className={styles.modalActions}>
          <Button
            variant="secondary"
            onClick={() => setShowConfirmModal(false)}
          >
            {t("continueEditing")}
          </Button>
          <Button variant="danger" onClick={handleConfirmCancel}>
            {t("discardChanges")}
          </Button>
        </div>
      </Modal>
    </div>
  );
};
```

### Component Code Structure Rules

#### 1. Import Order Rules

```typescript
// ✅ Correct import order
import React, { useState } from "react"; // React first
import { useNavigate } from "react-router-dom"; // Third-party libraries
import { Button } from "@/shared/components"; // Internal shared
import { UserService } from "./services"; // Same folder relative
import styles from "./Component.module.scss"; // Styles last

// ❌ Incorrect import order
import styles from "./Component.module.scss"; // ❌ Styles not last
import { UserService } from "./services"; // ❌ Mixed order
import React from "react"; // ❌ React not first
```

#### 2. Constants Naming

```typescript
// ✅ Correct constants
const API_ENDPOINTS = {
  USERS: "/api/users",
  CONTRACTS: "/api/contracts",
} as const;

const VALIDATION_RULES = {
  EMAIL: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
  PHONE: /^\+?[\d\s-()]+$/,
} as const;

const DEFAULT_PAGE_SIZE = 20;
const MAX_FILE_SIZE = 5 * 1024 * 1024; // 5MB

// ❌ Incorrect constants
const apiEndpoints = {
  // ❌ Should be CONSTANT_CASE
  users: "/api/users",
};
const ValidationRules = {
  // ❌ Should be CONSTANT_CASE
  email: /pattern/,
};
```

#### 3. Export Patterns

##### Default Export (Avoid except for pages/lazy loading)

```typescript
// ✅ Acceptable for pages and lazy-loaded components
const UserProfilePageEdit = () => {
  /* */
};
export default UserProfilePageEdit;

// ✅ Acceptable for code splitting
export default lazy(() => import("./HeavyComponent"));

// ❌ Avoid for regular components
const Button = () => {
  /* */
};
export default Button; // ❌ Use named export instead
```

##### Named Exports (Preferred)

```typescript
// ✅ Preferred pattern
export const Button = () => {
  /* */
};
export const Input = () => {
  /* */
};
export const Modal = () => {
  /* */
};

// ✅ Multiple exports
const validateEmail = (email: string) => {
  /* */
};
const formatCurrency = (amount: number) => {
  /* */
};
const calculateTax = (amount: number) => {
  /* */
};

export { validateEmail, formatCurrency, calculateTax };
```

##### Re-exports (Index files)

```typescript
// ✅ Named re-exports in index.ts
export { Button } from "./Button";
export { Input } from "./Input";
export { Modal } from "./Modal";

// ✅ Selective re-exports
export { validateEmail, formatCurrency } from "./utils";

// ✅ Re-export with rename
export { UserProfileComponent as ProfileComponent } from "./UserProfileComponent";

// ❌ Avoid default re-exports
export { default as Button } from "./Button"; // ❌
export { default } from "./Button"; // ❌
```

---

## Testing Strategy

### Unit Testing with Jest and Testing Library

#### Component Testing

```typescript
// UserProfileComponent.test.tsx
import { render, screen, fireEvent, waitFor } from "@testing-library/react";
import { Provider } from "react-redux";
import { configureStore } from "@reduxjs/toolkit";
import { UserProfileComponent } from "./UserProfileComponent";
import userProfileReducer from "../store/userProfileSlice";

const createMockStore = (initialState = {}) => {
  return configureStore({
    reducer: {
      userProfile: userProfileReducer,
    },
    preloadedState: initialState,
  });
};

const renderWithProvider = (
  component: React.ReactElement,
  initialState = {}
) => {
  const store = createMockStore(initialState);
  return render(<Provider store={store}>{component}</Provider>);
};

describe("UserProfileComponent", () => {
  const mockProfile = {
    id: "1",
    name: "John Doe",
    email: "john@example.com",
  };

  it("renders user profile information", () => {
    renderWithProvider(<UserProfileComponent userId="1" />, {
      userProfile: {
        profiles: { "1": mockProfile },
        loading: false,
        error: null,
      },
    });

    expect(screen.getByText("John Doe")).toBeInTheDocument();
    expect(screen.getByText("john@example.com")).toBeInTheDocument();
  });

  it("shows loading state", () => {
    renderWithProvider(<UserProfileComponent userId="1" />, {
      userProfile: {
        profiles: {},
        loading: true,
        error: null,
      },
    });

    expect(screen.getByTestId("loading-spinner")).toBeInTheDocument();
  });

  it("handles edit button click", async () => {
    const onEdit = jest.fn();
    renderWithProvider(<UserProfileComponent userId="1" onEdit={onEdit} />, {
      userProfile: {
        profiles: { "1": mockProfile },
        loading: false,
        error: null,
      },
    });

    fireEvent.click(screen.getByRole("button", { name: /edit/i }));

    await waitFor(() => {
      expect(onEdit).toHaveBeenCalledWith(mockProfile);
    });
  });
});
```

#### Hook Testing

```typescript
// useUserProfile.test.ts
import { renderHook, act } from '@testing-library/react';
import { useUserProfile } from './useUserProfile';
import { UserProfileService } from '../services';

jest.mock('../services');

describe('useUserProfile', () => {
  const mockProfile = {
    id: '1',
    name: 'John Doe',
    email: 'john@example.com',
  };

  beforeEach(() => {
    jest.clearAllMocks();
  });

  it('loads user profile on mount', async () => {
    (UserProfileService.getById as jest.Mock).mockResolvedValue(mockProfile);

    const { result } = renderHook(() => useUserProfile('1'));

    expect(result.current.loading).toBe(true);

    await act(async () => {
      await new Promise(resolve => setTimeout(resolve, 0));
    });

    expect(result.current.loading).toBe(false);
    expect(result.current.profile).toEqual(mockProfile);
    expect(UserProfileService.getById).toHaveBeenCalledWith('1');
  });

  it('handles update profile', async () => {
    const updatedProfile = { ...mockProfile, name: 'Jane Doe' };
    (UserProfileService.getById as jest.Mock).mockResolvedValue(mockProfile);
    (UserProfileService.update as jest.Mock).mockResolvedValue(updatedProfile);

    const { result } = renderHook(() => useUserProfile('1'));

    await act(async () => {
      await new Promise(resolve => setTimeout(resolve, 0));
    });

    await act(async () => {
      await result.current.updateProfile({ name: 'Jane Doe' });
    });

    expect(result.current.profile).toEqual(updatedProfile);
    expect(UserProfileService.update).toHaveBeenCalledWith('1', { name: 'Jane Doe' });
```

---

**Document Info**

- **Author:** Anas Shofyan MF
- **Created:** 2025-08-01
- **Last Revised:** -
- **Contact:** anas@praisindo.com
- **Role:** Sr. Frontend Engineer
- **Location:** Jakarta, Indonesia

---
