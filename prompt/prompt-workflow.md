# Especificação do Workflow n8n — Gestão de Obras Musicais

Quero projetar uma automação no n8n para gerenciar o recebimento, organização, acompanhamento e comunicação de solicitações relacionadas a obras musicais em uma Editora de Música.

## 1. CONTEXTO DO PROCESSO

A Editora de Música recebe informações de autores/compositores por meio de um formulário online.

O processo envolve o recebimento da solicitação, validação das informações, organização dos dados, criação de documentos, acompanhamento pela equipe responsável e comunicação com o solicitante.

Como o volume de solicitações pode ser elevado e algumas etapas podem exigir análise ou intervenção humana, o processo deverá utilizar uma fila de solicitações para organizar a ordem de atendimento.

O objetivo é reduzir tarefas manuais, evitar perda de informações, proporcionar maior visibilidade à equipe e manter o solicitante informado sobre o andamento de sua solicitação.

---

## 2. OBJETIVO DA AUTOMAÇÃO

Criar um workflow no n8n capaz de:

- Receber novas solicitações enviadas pelo formulário;
- Validar os dados recebidos;
- Identificar informações obrigatórias ausentes;
- Criar um registro único para cada solicitação;
- Inserir solicitações válidas em uma fila de atendimento;
- Controlar o status e o andamento das solicitações;
- Disponibilizar as informações para a equipe por meio de um AppSheet;
- Gerar documentos automaticamente quando necessário;
- Enviar notificações pelo WhatsApp;
- Informar o solicitante sobre mudanças importantes no andamento;
- Registrar o histórico das etapas realizadas;
- Permitir tratamento de erros e pendências.

---

## 3. PÚBLICO / RESPONSÁVEIS

### Empresa

Editora de Música.

### Usuários internos

Equipe responsável pelo recebimento, análise, processamento e acompanhamento das solicitações.

### Usuários externos

Autores, compositores ou demais pessoas responsáveis pelo envio das obras e informações necessárias ao processo.

---

## 4. FERRAMENTAS ENVOLVIDAS

### Tally Form

Utilizado como porta de entrada para receber as informações enviadas pelos autores.

### n8n

Utilizado como mecanismo central de automação e integração entre as ferramentas.

O n8n deverá controlar as regras do processo, validações, atualizações, notificações e movimentação das solicitações.

### AppSheet

Utilizado como painel operacional da equipe.

A equipe deverá conseguir consultar as solicitações, visualizar seus status, identificar pendências, acompanhar a fila e registrar informações necessárias ao andamento do processo.

### Google Docs

Utilizado para criação e organização dos documentos relacionados às solicitações.

### WhatsApp

Utilizado como canal de comunicação e notificações.

A implementação deverá considerar uma solução de WhatsApp/API compatível com o ambiente da empresa.

---

# 5. FLUXO PRINCIPAL

O fluxo deverá seguir inicialmente esta estrutura:

Tally Form
↓
Recebimento da solicitação
↓
Validação dos dados
↓
Identificação da solicitação
↓
Registro da solicitação
↓
Fila de atendimento
↓
AppSheet
↓
Processamento pela equipe
↓
Geração de documentos
↓
Atualização do status
↓
Notificação pelo WhatsApp
↓
Conclusão

---

# 6. ETAPA 1 — RECEBIMENTO

Utilizar o Tally Form como ponto de entrada do workflow.

Sempre que uma nova resposta for enviada, o n8n deverá iniciar automaticamente o processo.

Cada nova solicitação deverá receber um identificador único.

Exemplo:

ID_SOLICITACAO: SOL-2026-0001

O identificador deverá permitir localizar a solicitação posteriormente no AppSheet e no histórico do processo.

---

# 7. ETAPA 2 — VALIDAÇÃO

O n8n deverá verificar se todos os campos obrigatórios foram preenchidos.

Considerar inicialmente como obrigatórios:

- Nome do autor/compositor;
- Título da obra;
- Letra da obra;
- Forma de contato.

Caso algum campo obrigatório esteja ausente, a solicitação não deverá seguir automaticamente para o processamento.

Nesse caso:

1. Registrar a solicitação;
2. Alterar o status para "PENDÊNCIA";
3. Registrar qual informação está faltando;
4. Disponibilizar a pendência no AppSheet;
5. Enviar uma notificação ao responsável, quando aplicável.

A automação não deverá criar documentos definitivos para solicitações que não possuam as informações mínimas necessárias.

---

# 8. ETAPA 3 — REGISTRO DA SOLICITAÇÃO

Toda solicitação recebida deverá ser registrada com informações como:

- ID da solicitação;
- Data e hora de recebimento;
- Nome do autor;
- Título da obra;
- Contato;
- Status;
- Prioridade;
- Posição na fila;
- Responsável;
- Pendência;
- Data de início do processamento;
- Data de conclusão;
- Link do documento;
- Observações.

O registro deverá servir como fonte de acompanhamento do processo.

---

# 9. ETAPA 4 — FILA DE SOLICITAÇÕES

As solicitações válidas deverão ser organizadas em uma fila.

A fila deverá considerar inicialmente:

- Ordem de recebimento;
- Prioridade;
- Status;
- Disponibilidade da equipe.

Cada solicitação deverá possuir uma posição ou referência que permita à equipe identificar sua ordem de atendimento.

A arquitetura deverá permitir futuramente a criação de regras mais avançadas de prioridade.

Exemplos:

- Solicitação normal;
- Solicitação prioritária;
- Solicitação aguardando informação;
- Solicitação em processamento;
- Solicitação concluída.

O sistema não deverá prometer ao solicitante um prazo específico caso esse prazo não esteja definido pela empresa.

---

# 10. ETAPA 5 — APPSHEET

Criar uma estrutura para que a equipe possa acompanhar as solicitações por meio de um aplicativo AppSheet.

O AppSheet deverá funcionar como painel operacional, permitindo visualizar informações sem depender da consulta manual ao n8n.

### Informações principais

- ID da solicitação;
- Autor;
- Obra;
- Data de recebimento;
- Status;
- Prioridade;
- Posição na fila;
- Responsável;
- Pendências;
- Data de início;
- Data de conclusão;
- Link do documento;
- Observações.

### Visualizações sugeridas

Dashboard:

- Novas solicitações;
- Em análise;
- Em processamento;
- Aguardando informação;
- Com pendência;
- Concluídas.

Lista da fila:

- Posição;
- Obra;
- Autor;
- Prioridade;
- Status;
- Responsável.

Detalhes da solicitação:

- Todos os dados da solicitação;
- Histórico;
- Pendências;
- Documentos;
- Observações.

---

# 11. ETAPA 6 — PROCESSAMENTO

Quando uma solicitação chegar à etapa de processamento, a equipe deverá conseguir atualizar seu status por meio do AppSheet.

Sugestão inicial de status:

- RECEBIDA;
- VALIDANDO;
- PENDÊNCIA;
- AGUARDANDO;
- EM PROCESSAMENTO;
- DOCUMENTO GERADO;
- CONCLUÍDA;
- CANCELADA.

O n8n deverá reagir às alterações relevantes de status quando necessário.

---

# 12. ETAPA 7 — GERAÇÃO DO DOCUMENTO

Quando a solicitação estiver apta para processamento, o n8n deverá gerar um documento no Google Docs.

O documento deverá possuir estrutura padronizada.

### Modelo inicial

NOVA OBRA MUSICAL

Título:
{{título_da_obra}}

Autor/Compositor:
{{nome_do_autor}}

Data de recebimento:
{{data_de_recebimento}}

Contato:
{{contato}}

Status:
{{status}}

LETRA DA OBRA

{{letra_completa}}

O documento deverá ser identificado de maneira padronizada, preferencialmente utilizando o título da obra e o nome do autor.

O link do documento deverá ser armazenado no registro da solicitação.

---

# 13. ETAPA 8 — NOTIFICAÇÕES

O WhatsApp deverá ser utilizado principalmente para comunicação e alertas.

### Confirmação de recebimento

Exemplo:

🎵 *Solicitação recebida*

Olá, {{nome}}!

Sua solicitação referente à obra "{{titulo}}" foi recebida e registrada.

ID da solicitação:
{{id_solicitacao}}

Sua solicitação está na fila para processamento.

---

### Aviso de proximidade

Quando a solicitação estiver próxima de ser processada, enviar uma notificação.

Exemplo:

🔔 *Atualização da solicitação*

Olá, {{nome}}!

Sua solicitação "{{titulo}}" está próxima de entrar em processamento.

ID:
{{id_solicitacao}}

Nossa equipe dará continuidade ao processo em breve.

---

### Início do processamento

Exemplo:

🎵 *Solicitação em processamento*

Olá, {{nome}}!

Sua solicitação referente à obra "{{titulo}}" entrou em processamento.

---

### Pendência

Exemplo:

⚠️ *Pendência na solicitação*

Olá, {{nome}}!

Identificamos uma pendência na solicitação "{{titulo}}".

Informação necessária:
{{pendencia}}

A equipe responsável poderá dar continuidade após a regularização.

---

### Conclusão

Exemplo:

✅ *Solicitação concluída*

Olá, {{nome}}!

A solicitação referente à obra "{{titulo}}" foi concluída.

ID:
{{id_solicitacao}}

Documento:
{{link_documento}}

---

# 14. NOTIFICAÇÃO DA EQUIPE

Quando uma nova solicitação válida for recebida, a equipe deverá ser notificada.

Exemplo:

🎵 *NOVA OBRA RECEBIDA*

Título:
{{titulo}}

Autor:
{{autor}}

ID:
{{id_solicitacao}}

Data:
{{data}}

A solicitação foi adicionada à fila.

📄 Documento:
{{link_documento}}

A equipe deverá utilizar o AppSheet para acompanhar o andamento da solicitação.

---

# 15. HISTÓRICO

Sempre que uma etapa importante ocorrer, registrar:

- Data e hora;
- ID da solicitação;
- Status anterior;
- Novo status;
- Responsável;
- Observação, quando aplicável.

O histórico deverá permitir identificar como a solicitação percorreu o processo.

---

# 16. REGRAS IMPORTANTES

1. Não processar automaticamente solicitações sem título.
2. Não processar automaticamente solicitações sem letra.
3. Não alterar o conteúdo original da letra.
4. Toda solicitação deve possuir um identificador único.
5. Toda solicitação válida deve entrar na fila.
6. O AppSheet deve permitir o acompanhamento das solicitações.
7. O WhatsApp deve ser utilizado para notificações e não como principal ferramenta de controle operacional.
8. O documento deve ser criado antes da notificação que informa sua disponibilidade.
9. Não informar ao solicitante que uma etapa foi concluída caso ela tenha falhado.
10. Registrar erros importantes para facilitar o diagnóstico.
11. Solicitações com pendências devem permanecer registradas.
12. O processo deve permitir intervenção humana.
13. A automação não deve substituir decisões que dependam de análise da equipe.
14. O sistema deve evitar o envio duplicado de notificações.
15. O processo deve ser preparado para futuras alterações de regras.

---

# 17. TRATAMENTO DE ERROS

Caso ocorra falha em qualquer integração, o workflow deverá:

1. Identificar a etapa que apresentou erro;
2. Registrar o erro;
3. Manter a solicitação registrada;
4. Atualizar o status adequadamente;
5. Evitar informar conclusão ao solicitante;
6. Permitir que a equipe identifique a falha pelo AppSheet;
7. Permitir reprocessamento quando tecnicamente possível.

---

# 18. ARQUITETURA PROPOSTA

A solução deverá seguir o conceito:

Tally Form
→ Entrada de dados

n8n
→ Orquestração e automação

Base de dados / estrutura de controle
→ Registro das solicitações e histórico

AppSheet
→ Interface operacional da equipe

Google Docs
→ Documentação

WhatsApp
→ Comunicação e notificações

A implementação deverá manter as responsabilidades de cada ferramenta separadas para facilitar manutenção, escalabilidade e futuras alterações.

---

# 19. POSSÍVEIS EVOLUÇÕES FUTURAS

A arquitetura deverá permitir futuramente:

- Controle de diferentes tipos de solicitação;
- Diferentes filas;
- Regras de prioridade;
- Distribuição automática por responsável;
- Controle de prazos;
- Relatórios gerenciais;
- Indicadores de volume;
- Tempo médio de processamento;
- Histórico completo por autor;
- Integração com outros sistemas da empresa;
- Automação de outras etapas documentais;
- Agentes de IA para classificação ou conferência de informações.

---

# 20. OBJETIVO FINAL

Criar uma solução de automação no n8n organizada, confiável e preparada para uso real em uma Editora de Música.

A solução deverá automatizar o recebimento e acompanhamento das solicitações, organizar uma fila de atendimento, disponibilizar um painel operacional para a equipe, gerar documentos quando necessário e manter os envolvidos informados por meio de notificações.

O workflow deverá priorizar organização, rastreabilidade, tratamento de erros, intervenção humana quando necessária e possibilidade de expansão futura.
