# Configuração do Firebase para o Portal ePol

## 📋 Visão Geral

O portal agora possui integração com **Firebase Firestore** para persistência de dados. Isso permite que os dados sejam mantidos mesmo após fechar o navegador e sincronizados entre diferentes dispositivos.

### Coleções do Firebase:
- **servidores**: Cadastro de servidores disponíveis
- **grades**: Grades semanais importadas (CSV/Excel)
- **alocacoes**: Alocações de servidores na aba "Montar Grade"

## 🔧 Como Configurar o Firebase

### Passo 1: Criar um Projeto no Firebase

1. Acesse [Firebase Console](https://console.firebase.google.com/)
2. Clique em **"Adicionar projeto"** ou **"Create a project"**
3. Dê um nome ao projeto (ex: `escala-epol-anp`)
4. (Opcional) Desabilite o Google Analytics se não for necessário
5. Clique em **"Criar projeto"**

### Passo 2: Configurar o Firestore Database

1. No menu lateral, clique em **"Firestore Database"**
2. Clique em **"Criar banco de dados"**
3. Escolha o modo:
   - **Modo de produção**: Mais seguro, requer configuração de regras
   - **Modo de teste**: Permite leitura/gravação por 30 dias (recomendado para início)
4. Escolha a localização (recomendado: `southamerica-east1` para Brasil)
5. Clique em **"Ativar"**

### Passo 3: Obter as Credenciais do Firebase

1. No Firebase Console, clique no ícone de **engrenagem** ⚙️ ao lado de "Visão geral do projeto"
2. Selecione **"Configurações do projeto"**
3. Role até a seção **"Seus aplicativos"**
4. Clique no ícone **`</>`** (Web) para adicionar um app da Web
5. Dê um nome ao app (ex: `Portal ePol Web`)
6. **NÃO** marque "Configurar Firebase Hosting" (a menos que queira usar)
7. Clique em **"Registrar app"**
8. Copie as configurações do Firebase que aparecem

### Passo 4: Adicionar as Credenciais no Código

1. Abra o arquivo `index.html`
2. Localize a seção de configuração do Firebase (por volta da linha 1883):

```javascript
const firebaseConfig = {
  apiKey: "SUA_API_KEY_AQUI",
  authDomain: "seu-projeto.firebaseapp.com",
  projectId: "seu-projeto-id",
  storageBucket: "seu-projeto.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123def456"
};
```

3. Substitua pelos valores copiados do Firebase Console:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyAbc123xyz...",
  authDomain: "portal-epol.firebaseapp.com",
  projectId: "portal-epol",
  storageBucket: "portal-epol.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abc123def456789"
};
```

4. Salve o arquivo

### Passo 5: Configurar Regras de Segurança (Importante!)

⚠️ **Atenção**: As regras padrão do "modo de teste" expiram em 30 dias!

#### 📍 Como Encontrar a Aba de Regras:

**Método 1: Via Firestore Database (Principal)**

1. No Firebase Console, no **menu lateral esquerdo**, clique em **"Firestore Database"**
   - Pode aparecer como "Firestore Database" ou apenas "Firestore"
   
2. Você verá uma tela com o banco de dados. **No topo da tela**, procure pelas abas:
   - **Data** (Dados) - onde você vê os documentos
   - **Rules** (Regras) - ← **ESTA É A ABA QUE VOCÊ PRECISA**
   - **Indexes** (Índices)
   - **Usage** (Uso)

3. Clique na aba **"Rules"** ou **"Regras"**

**Método 2: Via Menu Direto (Alternativa)**

Se não encontrar a aba, tente:

1. No menu lateral esquerdo, role para baixo
2. Procure por uma seção chamada **"Firestore Database"** ou **"Build"**
3. Expanda se necessário (clique na setinha)
4. Deve aparecer uma opção chamada **"Regras"** ou **"Rules"**
5. Clique nela

**Método 3: URL Direta**

Acesse diretamente via URL (substitua `SEU-PROJETO-ID` pelo ID do seu projeto):

```
https://console.firebase.google.com/project/SEU-PROJETO-ID/firestore/rules
```

#### ⚙️ Configurando as Regras:

Depois de encontrar a aba/página de Regras:

1. Você verá um **editor de texto** com código
2. **Delete todo o conteúdo** existente
3. **Cole uma das regras abaixo** (escolha conforme sua necessidade)

---

**🟢 Opção A: Para TESTES e DESENVOLVIMENTO** (Recomendado para começar)

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Coleção de servidores
    match /servidores/{document=**} {
      allow read, write: if true;
    }
    // Coleção de grades semanais
    match /grades/{document=**} {
      allow read, write: if true;
    }
    // Coleção de alocações (Montar Grade)
    match /alocacoes/{document=**} {
      allow read, write: if true;
    }
  }
}
```

✅ **Use quando**: Estiver testando, sem preocupação com segurança ainda  
⚠️ **Atenção**: Qualquer pessoa pode acessar! Use só para desenvolvimento

---

**🟡 Opção B: Para USO INTERNO** (Acesso apenas autenticado)

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Coleção de servidores
    match /servidores/{document=**} {
      allow read, write: if request.auth != null;
    }
    // Coleção de grades semanais
    match /grades/{document=**} {
      allow read, write: if request.auth != null;
    }
    // Coleção de alocações (Montar Grade)
    match /alocacoes/{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

✅ **Use quando**: Quiser que apenas usuários logados acessem  
📝 **Requer**: Configurar Firebase Authentication (veja seção abaixo)

---

**🔴 Opção C: Para PRODUÇÃO** (Apenas e-mails @pf.gov.br)

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Coleção de servidores
    match /servidores/{document=**} {
      allow read, write: if request.auth != null && 
                             request.auth.token.email.matches('.*@pf.gov.br$');
    }
    // Coleção de grades semanais
    match /grades/{document=**} {
      allow read, write: if request.auth != null && 
                             request.auth.token.email.matches('.*@pf.gov.br$');
    }
    // Coleção de alocações (Montar Grade)
    match /alocacoes/{document=**} {
      allow read, write: if request.auth != null && 
                             request.auth.token.email.matches('.*@pf.gov.br$');
    }
  }
}
```

✅ **Use quando**: Produção - apenas servidores da PF podem acessar  
🔒 **Mais seguro**: Valida e-mail institucional

---

4. Depois de colar a regra escolhida, clique no botão **"Publicar"** ou **"Publish"** (no topo do editor)

5. Aguarde a mensagem de confirmação: **"Regras publicadas com sucesso"**

#### 🆘 Se Ainda Não Encontrou a Aba:

**Possível causa**: Você pode estar vendo o **Realtime Database** ao invés do **Firestore Database**

✅ **Solução**: 
1. Verifique se você está em **"Firestore Database"** (NoSQL)
2. NÃO confunda com **"Realtime Database"** (é outro produto)
3. No menu lateral, certifique-se de clicar em **"Firestore Database"**

**Printscreen da interface** (localização das abas):
```
┌─────────────────────────────────────────────────────┐
│  Firestore Database                                  │
├─────┬──────┬─────────┬──────────────────────────────┤
│ Data│Rules │ Indexes │ Usage       [+ Start collection]│
│     │  ↑   │         │                               │
│   CLIQUE AQUI                                        │
└──────────────────────────────────────────────────────┘
```

#### 📧 Configurar Autenticação (Para Opções B e C)

Se escolheu Opção B ou C, configure a autenticação:

1. No menu lateral, clique em **"Authentication"** ou **"Autenticação"**
2. Clique em **"Get started"** ou **"Começar"**
3. Escolha um método:
   - **Email/Password** (E-mail/senha): Mais simples
   - **Google**: Login com conta Google
4. Ative o método escolhido e salve
5. Na aba **"Users"**, adicione os usuários autorizados (e-mail e senha)

## 🎯 Funcionalidades Implementadas

### CRUD Completo de Servidores

- ✅ **Criar**: Adicionar novos servidores manualmente
- ✅ **Ler**: Visualizar lista de servidores com filtros
- ✅ **Atualizar**: Editar informações de servidores existentes
- ✅ **Excluir**: Remover servidores do sistema

### Importação e Persistência de Dados

- ✅ **Importação de Servidores (Excel)**: Todos os servidores são salvos automaticamente no Firebase
- ✅ **Importação de Grade Semanal (CSV/Excel)**: Grade completa salva no Firebase
- ✅ **Sincronização Automática**: Dados mesclados sem duplicação
- ✅ **Histórico de Grades**: Múltiplas semanas podem ser armazenadas

### Persistência Dual

O sistema utiliza duas camadas de persistência:

1. **Firebase Firestore** (primário)
   - Dados sincronizados na nuvem
   - Acessível de qualquer dispositivo
   - Requer conexão com internet
   - Duas coleções principais: `servidores`, `grades` e `alocacoes`

2. **LocalStorage** (backup/fallback)
   - Dados armazenados localmente no navegador
   - Funciona offline
   - Específico de cada dispositivo/navegador

### Sincronização Inteligente

- Servidores do Excel + servidores do Firebase são mesclados automaticamente
- Grade da semana salva automaticamente ao importar CSV/Excel
- Não há duplicação de dados (baseado em ID ou nome+cargo)
- Se o Firebase estiver offline, salva apenas no LocalStorage
- Toast de feedback para todas as operações

## 🚀 Usando o Sistema

### Importar Servidores (Excel)

1. Vá para a aba **"Carregar Arquivos"**
2. Faça upload do arquivo **"Alocações ePol.xlsx"**
3. Aguarde a mensagem: **"✅ Firebase: X novo(s), Y atualizado(s)"**
4. Todos os servidores são salvos automaticamente no Firebase

### Importar Grade da Semana (CSV/Excel)

1. Vá para a aba **"Carregar Arquivos"**
2. Faça upload do arquivo **"semana XX.csv"**
3. Aguarde a mensagem: **"✅ Grade salva no Firebase: X registros"**
4. A grade completa é salva automaticamente no Firebase

### Adicionar Novo Servidor

1. Vá para a aba **"Servidores"**
2. Clique no botão **"Novo Servidor"**
3. Preencha os campos obrigatórios (Nome, Cargo, Função)
4. Clique em **"Salvar"**

### Editar Servidor

1. Na tabela de servidores, clique no botão **✏️ (Editar)**
2. Modifique os campos necessários
3. Clique em **"Salvar"**

### Excluir Servidor

1. Na tabela de servidores, clique no botão **🗑️ (Excluir)**
2. Confirme a exclusão

### Filtrar Servidores

Use os campos de filtro para:
- Filtrar por Função (Professor/Monitor)
- Filtrar por Cargo (EPF/APF/PCF)
- Filtrar por data de disponibilidade
- Buscar por nome

## 📊 Estrutura de Dados no Firebase

### Coleção: servidores

Os servidores são armazenados na coleção `servidores` com a seguinte estrutura:

```javascript
{
  id: "local_1234567890_abc123",  // ID único gerado automaticamente
  nome: "João da Silva",           // Nome completo
  cargo: "EPF",                    // Cargo (EPF/APF/PCF/DPF/PPF)
  funcao: "PROFESSOR EPOL",        // Função (PROFESSOR EPOL/MONITOR EPOL/etc)
  lotacao: "SR/PF/DF",             // Lotação
  dtIni: "2026-01-15",             // Data inicial de disponibilidade (opcional)
  dtFim: "2026-12-31",             // Data final de disponibilidade (opcional)
  tel: "(61) 99999-9999",          // Telefone (opcional)
  email: "joao.silva@pf.gov.br",  // E-mail (opcional)
  origem: "MANUAL"                 // Origem do cadastro (EXCEL/MANUAL)
}
```

### Coleção: grades

As grades semanais são armazenadas na coleção `grades` com a seguinte estrutura:

```javascript
{
  semana: "semana 13",                    // Identificador da semana
  dataImportacao: "2026-04-14T10:30:00Z", // Data/hora da importação
  totalRegistros: 150,                     // Total de linhas na grade
  linhas: [                                // Array com todas as linhas
    {
      data: "2026-04-14",                 // Data da aula
      turno: "Manhã",                     // Turno (Manhã/Tarde/Noite)
      horario: "08:00-10:00",             // Horário
      modulo: "Módulo 1",                 // Módulo do curso
      codAula: "A001",                    // Código da aula
      funcao: "Prof. epol",               // Função (Prof. epol/Monitor epol)
      servidor: "João da Silva",          // Nome do servidor alocado (ou vazio)
      turma: "K (11)",                    // Turma
      slotKey: "2026-04-14|08:00|A001..." // Chave única do slot
    },
    // ... mais linhas
  ]
}
```

**Nota**: Cada documento na coleção `grades` representa uma semana completa. O ID do documento é gerado automaticamente baseado no nome do arquivo (ex: `semana-13`, `semana-14`).

### Coleção: alocacoes

As alocações de servidores (aba "Montar Grade") são armazenadas na coleção `alocacoes`:

```javascript
{
  semana: "semana 13",                    // Identificador da semana
  dataAtualizacao: "2026-04-14T15:45:00Z", // Data/hora da última atualização
  alocacoes: {                            // Objeto com as alocações
    "2026-04-14|08:00|A001|Prof. epol|K (11)|0": "João da Silva",
    "2026-04-14|10:00|A002|Monitor epol|K (11)|0": "Maria Santos",
    // ... mais alocações (slotKey → nome do servidor)
  }
}
```

**Nota**: 
- A chave `slotKey` é única para cada vaga e combina: data, horário, código da aula, função, turma e índice
- O valor é sempre o nome completo do servidor alocado
- Quando você remove uma alocação, a chave é deletada do objeto
- Cada semana tem seu próprio documento (ID baseado no nome da semana)

## 🔒 Segurança e Boas Práticas

### Para Uso em Produção

1. **Ative autenticação de usuários:**
   - Configure Firebase Authentication
   - Adicione login com e-mail/senha ou Google
   - Atualize as regras do Firestore para `if request.auth != null`

2. **Proteja suas credenciais:**
   - Não comite o arquivo com as credenciais em repositório público
   - Use variáveis de ambiente se possível
   - Restrinja o acesso por domínio no Firebase Console

3. **Monitore o uso:**
   - Acesse o painel do Firebase para ver estatísticas de uso
   - Firebase oferece cota gratuita generosa:
     - 50.000 leituras/dia
     - 20.000 gravações/dia
     - 20.000 exclusões/dia
     - 1 GB de armazenamento

### Limites do Plano Gratuito

O Firebase Spark (gratuito) oferece:
- ✅ 50.000 leituras por dia
- ✅ 20.000 gravações por dia
- ✅ 20.000 exclusões por dia
- ✅ 1 GB de armazenamento
- ✅ 10 GB/mês de transferência de dados

Suficiente para uso interno em organizações pequenas/médias.

## 🆘 Solução de Problemas

### Firebase não conecta

1. Verifique se as credenciais estão corretas
2. Abra o Console do navegador (F12) e procure por erros
3. Verifique se o Firestore está ativado no Firebase Console

### Dados não aparecem

1. Verifique as regras de segurança do Firestore
2. Certifique-se de que a coleção `servidores` existe
3. Limpe o cache do navegador e recarregue a página

### Modo Offline

- Se o Firebase estiver offline/desconfigurado, o sistema funciona normalmente
- Usa apenas LocalStorage como fallback
- Mensagem "Firebase não configurado" aparece no console

## 📝 Alternativas ao Firebase

Se preferir outras opções de banco de dados:

1. **Supabase** (PostgreSQL na nuvem)
   - Open source
   - PostgreSQL grátis
   - API REST e Real-time

2. **MongoDB Atlas** (NoSQL)
   - 512 MB grátis
   - Fácil integração

3. **PocketBase** (Self-hosted)
   - Banco SQLite
   - Backend em Go
   - Hospedagem própria

## 📚 Recursos Adicionais

- [Documentação do Firebase](https://firebase.google.com/docs)
- [Guia do Firestore](https://firebase.google.com/docs/firestore)
- [Configurar Regras de Segurança](https://firebase.google.com/docs/firestore/security/get-started)
- [Limites e Cotas](https://firebase.google.com/docs/firestore/quotas)

---

**Desenvolvido para a Polícia Federal - Academia Nacional de Polícia**
