# React 18 + TypeScript + ESLint 8 + Prettier + VSCode 설정 (전통 .eslintrc.json 방식)

## 1. CRA + TypeScript + React 18 프로젝트 생성

```bash
npx create-react-app . --template typescript
```

```bash
npm install react@18.2.0 react-dom@18.2.0
```

## 2. ESLint + Prettier 호환 패키지 설치 (버전 명시)

- CRA의 react-scripts@5.0.1과 호환되는 안정된 버전 조합

```bash
npm install -D \
eslint@8.56.0 \
@typescript-eslint/eslint-plugin@5.62.0 \
@typescript-eslint/parser@5.62.0 \
eslint-plugin-react@7.33.2 \
eslint-plugin-react-hooks@4.6.0 \
eslint-plugin-jsx-a11y@6.7.1 \
eslint-plugin-prettier@5.1.3 \
eslint-config-prettier@9.1.0 \
prettier@3.2.5
```

## 3. `.eslintrc.json`

```json
{
  "root": true,
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": 2020,
    "sourceType": "module",
    "ecmaFeatures": { "jsx": true }
  },
  "env": {
    "browser": true,
    "es2021": true,
    "node": true
  },
  "plugins": ["react", "react-hooks", "jsx-a11y", "@typescript-eslint", "prettier"],
  "extends": [
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:react-hooks/recommended",
    "plugin:jsx-a11y/recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:prettier/recommended"
  ],
  "rules": {
    "react/react-in-jsx-scope": "off",
    "prettier/prettier": "error",
    "@typescript-eslint/no-unused-vars": "warn"
  },
  "settings": {
    "react": {
      "version": "detect"
    }
  }
}
```

## 4. Prettier 설정

- `.prettierrc`

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 100,
  "tabWidth": 2,
  "arrowParens": "avoid"
}
```

## 5. `.eslintignore` 추가 (선택)

```
node_modules
build
dist
```

## 6. VSCode 연동

- source.fixAll: true는 최신 VSCode에서 오류 발생하므로 "explicit" 사용.
- 저장 시 ESLint 자동 수정은 안 되고, 수동 명령으로 실행해야 합니다.
- `.vscode/settings.json 생성`

```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll": "explicit"
  },
  "eslint.validate": ["javascript", "javascriptreact", "typescript", "typescriptreact"]
}
```

## 7. `package.json`에 Lint 스크립트 추가

```json
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "lint": "eslint \"src/**/*.{ts,tsx}\"",
    "lint:fix": "eslint \"src/**/*.{ts,tsx}\" --fix"
  },
```

## 8. 테스트 코드로 실제 확인

```tsx
const App = () => {
  const unused = 1;
  return <div tabIndex="0">hello</div>;
};
export default App;
```

- `npm run lint` 또는 `npx eslint src --ext .tsx`
  - `--fix`를 붙이면, 자동으로 고칠 수 있는 오류는 자동 수정됩니다
  - `- npx eslint src --ext .tsx --fix`
- VSCode에서 수동 명령 팔레트 실행: `Ctrl+Shift+P` → `ESLint: Fix all auto-fixable problems`
