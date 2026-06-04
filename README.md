# HM Finance

Gestão pessoal de créditos, amortização e cash flow.  
Stack: HTML vanilla + Firebase Firestore + GitHub Pages.

---

## Deploy em 5 passos

### 1. Firebase — criar projeto

1. Vai a [console.firebase.google.com](https://console.firebase.google.com)
2. **Add project** → nome: `hm-finance` → continua
3. No menu lateral: **Build → Firestore Database → Create database**
   - Escolhe **production mode**
   - Região: `europe-west1`
4. **Build → Authentication → Get started → Anonymous → Enable**
5. Vai a **Project Settings** (ícone ⚙️) → **Your apps** → **Web app** (ícone `</>`)
   - Regista a app, copia o `firebaseConfig`

### 2. Firestore — regras de segurança

Em **Firestore → Rules**, substitui pelo seguinte:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/{document=**} {
      allow read, write: if true;
    }
  }
}
```

> Segurança baseada em PIN — os dados são isolados por hash do PIN.

### 3. Substituir credenciais no index.html

Abre `index.html` e encontra este bloco (linha ~180):

```js
const FIREBASE_CONFIG = {
  apiKey: "COLE_AQUI_API_KEY",
  authDomain: "COLE_AQUI.firebaseapp.com",
  projectId: "COLE_AQUI_PROJECT_ID",
  ...
};
```

Substitui pelos valores reais do Firebase Console.

### 4. GitHub Pages

```bash
git init
git add .
git commit -m "init: hm-finance"
git branch -M main
git remote add origin https://github.com/hmlm90020798-alt/hm-finance.git
git push -u origin main
```

No repositório GitHub:
- **Settings → Pages → Source: Deploy from branch → main / root → Save**
- Em ~2 minutos: `https://hmlm90020798-alt.github.io/hm-finance/`

### 5. Primeiro acesso

- Abre o URL
- Define o teu PIN (mínimo 4 dígitos)
- Os dados ficam associados ao PIN e sincronizados via Firebase

---

## Estrutura

```
hm-finance/
├── index.html      ← app completa (single file)
└── README.md
```

## Funcionalidades

- **Visão** — dashboard com métricas, alertas automáticos, cash flow
- **Créditos** — registo com decomposição juros/capital, progresso de liquidação
- **Amortizar** — simulador de amortização antecipada + impacto de variação Euribor
- **Gastos** — registo diário por categoria
- **Cash flow** — rendimentos, despesas fixas, saldo livre, gráfico, taxa de esforço
- **Exportar** — resumo em texto para análise com Claude
