# IA Panoramica (IA que coleta todas as informações da jornada)

ID: CCM-3269
Épico: Agente conversacional
Prioridade: 9 (Urgente)
Categoria: Dados & Métricas
Versão: v3.0
Tipo: Melhoria
Status da Task: Produto
Cliente: Diretoria
Previsão Back: 15
Responsável Produto: Lucas Rehem, Alves camila
Status Produto: ✏️ Fazendo Produto
Responsável UX/UI: suzany ribeiro
Status UX/UI: 🎨 Fazendo UX
Criado em: August 27, 2026 3:13 PM
Situação Front: ⚫ Sem Prazo
Situação Back: ⚫ Sem Prazo
Produto: 🟣 Não Iniciado
UX: 🟣 Não Iniciado
Story Points: 15
Criado por: Lucas Rehem
Falta validar: Maria Clara Coimbra,Alves camila,Lucas Rehem,suzany ribeiro,Paulo Lebtag
Atualizada (Master): No
Dias restantes para validar: Sem prazo
SLA: Sem prioridade

- Especificação
    
    # Objetivo
    
    Disponibilizar uma IA que analisa uma jornada de automação de ponta a ponta — o fluxo, seus relatórios de IA e de jornada, as conversas e os atendimentos humanos originados dela (mensagem ou voz) — e entrega um relatório com as perguntas mais frequentes dos contatos, os argumentos utilizados pelos atendentes e sua eficácia, sugestões de melhoria no fluxo e a coleta de informações definidas por prompt em variáveis reutilizáveis no sistema.
    
    **User Story:** Como **gestor da operação**, quero **gerar análises por IA de uma jornada e de suas conversas**, para **entender o que os contatos perguntam, como a operação responde e o que precisa melhorar no fluxo, sem ler conversas manualmente**.
    
    # Contexto
    
    - A jornada de automação possui relatório próprio e, vinculado a ele, o relatório do agente conversacional (IA). Esses relatórios são a camada de insights que a análise utiliza como fonte.
    - Os relatórios já exibem a transcrição automática dos áudios de atendimentos e jornadas por voz. A análise das ligações utiliza essas transcrições; não há dependência nova.
    - Hoje, uma informação dita pelo contato só é salva em variável quando um nó específico da jornada a solicita e grava a resposta. Não existe forma de extrair uma informação mencionada em qualquer ponto da conversa.
    - O Chat possui o assistente de IA do operador, com personalização de prompt em andamento (CCM-2384).
    - Demanda trazida pela Diretoria para consolidar as informações e o contexto geral da jornada com a qual o contato interagiu.
    
    **Fora do escopo (v1):** acesso externo via MCP ou qualquer IA que não seja a da Vonex; captura de variáveis em tempo real durante a conversa; análise com intervalo superior a 30 dias; análise de múltiplas jornadas na mesma execução; extração de variáveis por prompt avulso (somente prompt fixo configurado).
    
    # Regras de Negócio
    
    ## Acesso à funcionalidade
    
    - A análise é acessada a partir da jornada e integra os relatórios de jornada; ponto de entrada final **[a definir com UX]**.
    - Perfis com acesso: **[a definir — decisão pendente de permissões e privacidade]**.
    - Toda análise e toda resposta ao prompt são geradas exclusivamente pela IA da Vonex, dentro da plataforma.
    
    ## Fontes de dados da análise
    
    - A análise considera: a estrutura da jornada de automação; o relatório da jornada e o relatório do agente conversacional; as conversas da jornada; os atendimentos humanos originados pela jornada, por mensagem ou por voz.
    - Ligações são analisadas por meio da transcrição automática já existente nos relatórios.
    - Atendimento sem ligação vinculada entra somente na análise de texto.
    - Ligação sem transcrição disponível é desconsiderada e contabilizada no relatório como "não analisada".
    
    ## Recorte da análise
    
    - Cada análise contempla uma única jornada.
    - O período é selecionado livremente (data inicial e final), em qualquer data, com intervalo máximo de 30 dias.
    - Intervalo superior a 30 dias bloqueia a geração e exibe mensagem informando o limite.
    - Período com volume baixo de conversas exibe aviso de que a análise pode não ser representativa; limite mínimo **[a definir]**.
    
    ## Geração da análise
    
    - A geração é processada em segundo plano; o usuário pode continuar navegando e é notificado ao término.
    - Enquanto processa, a análise consta no histórico com status "Processando".
    - Em caso de falha, a análise consta com status "Falha" e permite nova tentativa.
    - Uma análise concluída não é alterada; dados novos exigem nova geração.
    
    ## Conteúdo do relatório gerado pela IA
    
    - Resumo executivo com os principais achados.
    - Perguntas mais frequentes dos contatos, agrupadas por tema, com volume, percentual e exemplos reais.
    - Argumentos utilizados pelos atendentes, com frequência de uso e eficácia calculada pelo desfecho dos atendimentos em que cada argumento foi utilizado; desfechos considerados **[a definir]**.
    - Validação nas ligações: cada pergunta e cada argumento indica se foi confirmado nas ligações e em quantas.
    - Sugestões de melhoria na jornada: perguntas frequentes sem resposta automática no fluxo, com indicação do ponto onde uma resposta poderia ser inserida.
    - Exportação do relatório; formatos **[a definir — seguir padrão dos demais relatórios]**.
    
    ## Campo de prompt
    
    - Toda análise concluída disponibiliza um campo de prompt para perguntas livres sobre seus dados.
    - As respostas consideram somente os dados da análise aberta (jornada e período selecionados). Perguntas fora desse recorte recebem a informação de que não há dados disponíveis.
    - Perguntas e respostas ficam salvas junto à análise.
    
    ## Histórico de análises
    
    - Todas as análises geradas ficam listadas com: jornada, período analisado, data e hora de geração, usuário solicitante, tipo (manual ou recorrente) e status.
    - Análise concluída pode ser reaberta a qualquer momento sem novo processamento.
    - Retenção e exclusão de análises: **[a definir]**.
    
    ## Análises recorrentes
    
    - Uma jornada pode ter análise recorrente configurada; periodicidades disponíveis **[a definir]**.
    - Cada execução recorrente respeita o intervalo máximo de 30 dias.
    - Análise recorrente apresenta comparativo com a execução anterior (evolução das perguntas e dos argumentos).
    
    ## Coleta de informações em variáveis (prompt fixo)
    
    - A jornada permite configurar um ou mais prompts fixos de extração, cada um vinculado a uma variável de destino (exemplo: "Capturar o motivo de cancelamento informado pelo contato" → variável de motivo de cancelamento).
    - A extração ocorre durante a geração da análise; não ocorre em tempo real durante a conversa.
    - A IA busca a informação em qualquer ponto da conversa, independentemente do nó da jornada em que foi mencionada.
    - A variável preenchida fica disponível para uso em qualquer lugar do sistema (jornadas, campanhas, chat, relatórios).
    - Quando a informação não é encontrada, a variável permanece vazia; nenhum valor é inferido.
    - Valor já existente na variável de destino não é sobrescrito pela IA **[a definir]**.
    - Todo valor preenchido pela IA carrega a origem "Preenchido por IA", visível onde a variável for exibida.
    - Variável de destino: variável existente do contato ou criada na configuração **[a definir]**.
    
    ## Insumo para o assistente de IA do chat
    
    - Os argumentos de maior eficácia identificados ficam disponíveis como sugestão para a personalização do prompt do assistente do operador (CCM-2384); forma de disponibilização **[a definir]**.
    
    ## Isolamento e segurança
    
    - A análise considera exclusivamente dados da própria conta.
    - Restrição por fila/produto e anonimização de dados pessoais no relatório e no prompt: **[a definir — P6]**.
    
    # Critérios de Aceite
    
    - [ ]  O usuário seleciona uma jornada e um período de até 30 dias, em qualquer data, e gera a análise; intervalo maior que 30 dias é bloqueado com mensagem.
    - [ ]  A geração roda em segundo plano, consta no histórico como "Processando" e notifica o usuário ao concluir.
    - [ ]  O relatório apresenta resumo executivo, perguntas mais frequentes (tema, volume, percentual, exemplos), argumentos dos atendentes com frequência e eficácia por desfecho, validação nas ligações e sugestões de melhoria na jornada.
    - [ ]  Atendimentos humanos por mensagem e por voz originados pela jornada são considerados; ligações são analisadas pela transcrição existente.
    - [ ]  O campo de prompt responde somente com base nos dados da análise aberta, e perguntas e respostas ficam salvas junto à análise.
    - [ ]  O histórico lista todas as análises com jornada, período, data, solicitante, tipo e status, e permite reabrir análises concluídas sem novo processamento.
    - [ ]  Análise recorrente configurada executa automaticamente e apresenta comparativo com a execução anterior.
    - [ ]  Prompts fixos de extração configurados na jornada preenchem as variáveis de destino durante a geração; a variável permanece vazia quando a informação não é encontrada e o valor preenchido exibe a origem "Preenchido por IA".
    - [ ]  Variáveis preenchidas pela IA ficam disponíveis para uso em jornadas, campanhas e chat.
    - [ ]  Nenhuma etapa da funcionalidade permite acesso externo ou uso de IA que não seja a da Vonex.
    
    # Cenários de Teste (Gherkin)
    
    **Cenário 1 — Geração com período válido**
    
    **Dado** que o usuário está em uma jornada de automação com conversas registradas
    
    **Quando** seleciona um período de até 30 dias, em qualquer data, e aciona a geração da análise
    
    **Então** a análise consta no histórico com status "Processando"
    
    **E** o usuário é notificado quando a análise é concluída
    
    **Cenário 2 — Período acima do limite**
    
    **Dado** que o usuário está na seleção de período da análise
    
    **Quando** informa um intervalo de 31 dias ou mais
    
    **Então** a geração é bloqueada
    
    **E** o sistema exibe mensagem informando o limite de 30 dias
    
    **Cenário 3 — Validação nas ligações**
    
    **Dado** uma análise concluída de jornada com atendimentos humanos por voz
    
    **Quando** o usuário abre o relatório
    
    **Então** cada pergunta e cada argumento indica se foi confirmado nas ligações e em quantas
    
    **E** as ligações consideradas são as que possuem transcrição disponível
    
    **Cenário 4 — Eficácia dos argumentos**
    
    **Dado** uma análise concluída com atendimentos humanos com desfecho registrado
    
    **Quando** o usuário abre o bloco de argumentos dos atendentes
    
    **Então** cada argumento apresenta frequência de uso e eficácia calculada pelo desfecho dos atendimentos em que foi utilizado
    
    **Cenário 5 — Prompt restrito ao recorte da análise**
    
    **Dado** uma análise concluída da jornada A no período X
    
    **Quando** o usuário envia um prompt sobre outra jornada ou outro período
    
    **Então** a IA responde apenas com base nos dados da análise aberta
    
    **E** informa que não há dados disponíveis fora desse recorte
    
    **Cenário 6 — Reabertura pelo histórico**
    
    **Dado** uma análise concluída listada no histórico
    
    **Quando** o usuário a abre
    
    **Então** o relatório é exibido sem novo processamento
    
    **Cenário 7 — Extração em variável com informação presente**
    
    **Dado** uma jornada com prompt fixo de extração configurado para a variável de motivo de cancelamento
    
    **E** uma conversa em que o contato informou o motivo fora do nó que o solicitaria
    
    **Quando** a análise é gerada
    
    **Então** a variável do contato é preenchida com o motivo identificado
    
    **E** o valor exibe a origem "Preenchido por IA"
    
    **Cenário 8 — Extração sem informação na conversa**
    
    **Dado** uma jornada com prompt fixo de extração configurado
    
    **E** uma conversa em que a informação não foi mencionada
    
    **Quando** a análise é gerada
    
    **Então** a variável permanece vazia
    
    **E** nenhum valor é inferido
    
    **Cenário 9 — Variável já preenchida** [a definir]
    
    **Dado** um contato cuja variável de destino já possui valor
    
    **Quando** a análise é gerada e a IA identifica um valor diferente
    
    **Então** o valor existente é mantido
    
    **Cenário 10 — Análise recorrente com comparativo**
    
    **Dado** uma jornada com análise recorrente configurada
    
    **Quando** a execução recorrente é concluída
    
    **Então** o relatório apresenta comparativo com a execução anterior
    
    **Cenário 11 — Falha na geração**
    
    **Dado** uma análise em processamento
    
    **Quando** ocorre falha no processamento
    
    **Então** a análise consta no histórico com status "Falha"
    
    **E** o usuário pode gerar novamente