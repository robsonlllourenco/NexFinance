# 🛡️ Diretrizes e Arquitetura de Segurança — NexFinance

A integridade dos dados e a privacidade financeira dos usuários constituem os pilares fundamentais da engenharia do **NexFinance**. Este documento detalha, em alto nível conceitual, as práticas e salvaguardas implementadas.

---

## 🔒 Pilares de Segurança

### 1. Autenticação e Gestão de Sessões
- **Criptografia Unidirecional de Senhas:** Senhas nunca são armazenadas em texto simples. O sistema utiliza algoritmos robustos de hash criptográfico com salt aleatório (`bcrypt`), prevenindo ataques de dicionário e rainbow tables.
- **Tokens de Acesso Seguros:** As sessões de autenticação utilizam tokens assinados criptograficamente, com tempos de expiração controlados e renovação segura.
- **Autenticação Federada (OAuth 2.0 / Google Identity):** Integração com o Google Identity Services para login seguro, transferindo a verificação de identidade para a infraestrutura do Google e garantindo que credenciais externas nunca passem pelos servidores do aplicativo.

### 2. Proteção de Credenciais e Variáveis de Ambiente
- **Separação Rigorosa de Segredos:** Chaves de API, segredos de assinatura e credenciais de serviços externos residem exclusivamente em arquivos de ambiente (`.env`) mantidos fora do controle de versão e protegidos por `.gitignore`.
- **Injeção de Configuração em Tempo de Execução:** Em ambientes produtivos (ex: Square Cloud), variáveis são injetadas diretamente pelo ambiente operacional isolado.

### 3. Sanitização e Validação de Entradas
- **Defesa Contra Injeção (XSS / Code Injection):** Todas as entradas do usuário (descrições, categorias, notas) passam por escape HTML e validação estrita antes da renderização e persistência.
- **Validação de Tipos Numéricos e Datas:** Valores monetários são sanitizados e parseados por rotinas dedicadas que impedem discrepâncias com pontuações ou caracteres inesperados.

### 4. Isolamento Multi-usuário
- **Segregação de Dados:** Cada usuário autenticado possui seu escopo financeiro estritamente isolado por ID exclusivo (`userId`). Nenhuma consulta ou mutação pode cruzar o limite entre usuários diferentes.
- **Controle de Acesso em Nível de Endpoint:** Todas as rotas de finanças exigem autenticação obrigatória via cabeçalho `Authorization: Bearer <token>`.

### 5. Resiliência e Prevenção de Perda de Dados
- **Proteção do Banco de Dados no Deploy:** Os scripts de empacotamento e deploy excluem automaticamente os arquivos de banco de dados em produção, impedindo que publicações sobrescrevam os dados reais dos usuários.
- **Backups Criptografados e Portabilidade:** O usuário tem a possibilidade de realizar downloads de backups completos de seus próprios registros a qualquer momento em formato JSON estruturado.

---

> **Nota:** Por motivos éticos e de proteção patrimonial dos usuários, implementações algorítmicas específicas, rotas de infraestrutura interna e credenciais de serviços nunca são expostas publicamente.
