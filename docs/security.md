# Segurança — NexFinance

Este documento descreve as práticas e cuidados técnicos adotados no desenvolvimento do **NexFinance** para proteger as contas e as informações financeiras cadastradas.

---

## Práticas de Proteção Implementadas

### 1. Autenticação e Senhas
- **Armazenamento de senhas:** Nenhuma senha é salva em texto simples. O sistema utiliza a biblioteca `bcrypt` com geração de salt aleatório para gerar o hash seguro das credenciais.
- **Sessões protegidas:** As chamadas autenticadas utilizam tokens de acesso individuais para validar cada requisição.
- **Login com Google (OAuth 2.0):** Suporte ao Google Identity Services, permitindo que o usuário entre usando sua conta Google sem compartilhar a senha diretamente com a aplicação.

### 2. Gestão de Variáveis de Ambiente
- **Separação de chaves e segredos:** Chaves de API, credenciais e portas de rede ficam no arquivo `.env`, que é ignorado pelo Git através do `.gitignore`.
- **Exemplo de configuração:** O projeto disponibiliza apenas um `.env.example` sem valores reais para orientar a configuração local.

### 3. Validação e Tratamento de Dados
- **Prevenção contra XSS:** Textos informados pelo usuário (como nomes de despesas, descrições e anotações) passam por funções de escape antes de serem exibidos na tela.
- **Validação de entradas numéricas:** Tratamento específico para conversão de moedas e centavos, evitando erros de digitação e valores inválidos.

### 4. Isolamento por Usuário
- **Escopo restrito:** As rotas financeiras exigem autenticação obrigatória via cabeçalho `Authorization: Bearer <token>` e retornam somente os registros vinculados ao ID do usuário autenticado.

### 5. Cuidados no Deploy
- **Preservação de dados:** Os scripts de deploy para a hospedagem excluem pastas locais de dados para não sobrescrever as informações dos usuários que já estão em produção.
- **Backups pelo próprio usuário:** O sistema oferece a opção de baixar uma cópia completa dos lançamentos em formato JSON a qualquer momento.

---

> O código-fonte completo e a infraestrutura interna do backend permanecem em repositório privado. Este repositório público reúne apenas materiais voltados à apresentação do projeto.
