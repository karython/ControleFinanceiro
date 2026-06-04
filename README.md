# Finance Planner Pro

Aplicação web de controle financeiro pessoal com sincronização em nuvem via Firebase. Permite registrar receitas, despesas, parcelamentos, empréstimos e imposto de renda, com dashboards de KPIs, projeções de fluxo de caixa e diagnóstico inteligente.

## Funcionalidades

- **Dashboard Principal** — KPIs em tempo real: receitas, saídas totais, comprometimento de renda e saldo líquido projetado, com gráficos de projeção acumulada (6, 12 ou 24 meses) e composição de despesas
- **Receitas** — Cadastro com categoria, valor, data, recorrência e frequência
- **Despesas** — Cadastro com categoria, forma de pagamento, recorrência e frequência
- **Parcelamentos** — Controle de compras parceladas com avanço de parcela e saldo devedor restante
- **Empréstimos** — Registro com taxa de juros, cálculo automático de parcela (sistema Price) e controle de quitação
- **Imposto de Renda (IRPF)** — Parcelamento do IRPF com rastreamento de parcelas pagas
- **Simulador de Impacto** — Simula o efeito de uma nova parcela/empréstimo/financiamento sobre o comprometimento de renda antes de contratar
- **Reserva de Emergência** — Meta configurável com progresso e aporte mensal
- **Diagnóstico Inteligente** — Análise do endividamento total com classificação (Saudável / Atenção / Crítico) e sugestões personalizadas
- **Relatórios** — Exportação dos dados completos em JSON
- **Alertas automáticos** — Avisos de comprometimento de renda elevado, vencimentos próximos e projeção negativa
- **Autenticação** — Login/cadastro por e-mail (Firebase Auth) com sincronização de dados por usuário; modo local (sem conta) disponível como fallback
- **Tema claro/escuro** — Alternável pelo cabeçalho

## Tecnologias

| Camada | Tecnologia |
|--------|-----------|
| Framework | React 19 + TypeScript |
| Build | Vite 8 |
| Estilização | Tailwind CSS 4 |
| Ícones | Lucide React |
| Backend / Auth | Firebase 12 (Firestore + Authentication) |
| Deploy | Firebase Hosting |

## Pré-requisitos

- Node.js 18+
- npm 9+
- Projeto criado no [Firebase Console](https://console.firebase.google.com/) com **Authentication (e-mail/senha)** e **Firestore** habilitados

## Instalação e configuração

```bash
# Clone o repositório
git clone <url-do-repositorio>
cd finance-planner-pro

# Instale as dependências
npm install

# Configure as variáveis de ambiente
cp .env.example .env
```

Abra o arquivo `.env` e preencha com as credenciais do seu projeto Firebase (Firebase Console → Configurações do projeto → Seus aplicativos → SDK Config):

```env
VITE_FIREBASE_API_KEY=sua_api_key
VITE_FIREBASE_AUTH_DOMAIN=seu_projeto.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=seu_projeto
VITE_FIREBASE_STORAGE_BUCKET=seu_projeto.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=123456789
VITE_FIREBASE_APP_ID=1:123456789:web:abc123
VITE_APP_ID=finance-planner-pro-v1
```

## Scripts

```bash
npm run dev      # Servidor de desenvolvimento (http://localhost:5173)
npm run build    # Build de produção (saída em /dist)
npm run preview  # Visualizar o build localmente
npm run lint     # Análise estática com ESLint
```

## Deploy no Firebase Hosting

```bash
# Instale a CLI do Firebase (se necessário)
npm install -g firebase-tools

# Faça login e selecione o projeto
firebase login
firebase use --add

# Gere o build e publique
npm run build
firebase deploy
```

## Regras do Firestore

O arquivo `firestore.rules` já está configurado para isolamento por usuário — cada conta só acessa os próprios dados:

```
match /artifacts/{appId}/users/{userId}/{document=**} {
  allow read, write: if request.auth != null && request.auth.uid == userId;
}
```

Aplique as regras ao fazer o deploy: `firebase deploy --only firestore:rules`

## Modo offline / local

Caso o Firebase não esteja configurado ou o usuário opte por não criar conta, a aplicação funciona em modo local com dados de exemplo pré-carregados. Os dados não são persistidos entre sessões neste modo.

## Estrutura do projeto

```
finance-planner-pro/
├── public/            # Ícones e favicon
├── src/
│   ├── App.tsx        # Componente principal (toda a lógica e UI)
│   ├── App.css        # Estilos globais e componentes base
│   ├── index.css      # Reset e configuração do Tailwind
│   └── main.tsx       # Ponto de entrada
├── .env.example       # Modelo de variáveis de ambiente
├── firebase.json      # Configuração do Firebase Hosting
├── firestore.rules    # Regras de segurança do Firestore
└── vite.config.ts     # Configuração do Vite
```
