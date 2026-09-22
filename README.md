# IA Panorâmica (CCM-3269) — Proposta C: nativa da jornada

Terceira proposta de UX para o mesmo card. As outras duas continuam salvas:

| | Onde | Porta |
|---|---|---|
| A — módulo com abas | `../Gerenciador de whatsapp flow/` | 8899 |
| B — copiloto em painel | `../IA Panoramica/` | 8900 |
| **C — nativa da jornada** | **este projeto** | **8901** |

## Como rodar

```
python3 -m http.server 8901
```

## Telas

| Caminho | O que é |
|---|---|
| `index.html` | Tela de acesso |
| `jornada.html` | Versão atual — ajustada na reunião de 17/09 |
| `jornada-apresentada.html?modo=completa` | Registro do que foi apresentado (com `?modo=simplificada` também) |

## O que a reunião de 17/09 mudou

**Saiu:** relatórios pré-definidos (perguntas frequentes, argumentos dos
atendentes, métricas) — cada negócio é diferente demais e boa parte já existe no
monitoramento; coleta de informações em variáveis — sem uso claro definido;
comparativo com a execução anterior — só faz sentido com períodos iguais.

**Ficou:** sugestões de melhoria como única análise fixa e automática, lendo o
objetivo do agente; análises personalizadas em texto livre; chat sobre os dados.

**Entrou:** recorrência diária; envio do
relatório em PDF por e-mail; exportação em PDF e planilha; modal ao gerar para
desmarcar análises; botão de ver exemplo do que é gerado; e, no chat, quando o
dado não existe a IA oferece incluí-lo a partir do próximo processamento —
virando uma análise da jornada.

**Adiado:** o escopo por operação (decisão 1) saiu da tela — por ora toda
análise é de uma jornada só. A decisão continua valendo para depois; o desenho
que chegou a existir está no commit 7cf6308.

**Continua em aberto:** a cobertura ("1.284 conversas · 412 atendimentos…") ficou
como uma linha de rastreabilidade sob o resumo executivo, não como bloco de
métricas. É a leitura que fiz da decisão 4, que cortou métricas pré-definidas —
vale confirmar se era para sair por completo.

## Deploy na Vercel

A pasta é estática, sem build. Basta apontar a Vercel para ela.

O `vercel.json` liga `cleanUrls` e cria dois atalhos, para o link ficar curto na
hora de compartilhar:

- `/simplificada`
- `/completa`

Os caminhos com query string continuam funcionando nos dois ambientes, e são os
que a tela de acesso usa — assim o protótipo roda igual local e publicado.

## O conceito

A IA não é um módulo nem um painel — é uma **capacidade da jornada**, ligada ou
desligada no momento em que a jornada é criada.

**1. Configuração, na aba "Dados da jornada"**

Um cartão novo, no mesmo padrão dos existentes (Configurações, Tipo de jornada,
Canal, Fluxos de jornada), com um toggle "Usar a IA Panorâmica nesta jornada".
Ligado, revela:

- **Análise recorrente** — toggle + periodicidade
- **Coleta de informações em variáveis** — os prompts fixos de extração, que
  são configuração da jornada e não de um módulo separado

**2. Consumo, em uma aba própria da jornada**

Ligar o toggle adiciona a aba **IA Panorâmica** ao lado de Dados da jornada,
Monitoramento e Prévia de testes. Desligar remove a aba.

A aba tem o período, o botão de gerar, o histórico de análises **desta jornada**
e o relatório aberto abaixo.

**3. Atalho na listagem**

Jornadas com a IA habilitada mostram o selo "IA Panorâmica" e um ícone ✦ que
abre direto na aba de análise, com um ponto verde quando há análise nova.

## O que essa estrutura resolve sozinha

**Não existe seletor de jornada em lugar nenhum.** A jornada é o contexto, não
um filtro — aparece travada, com cadeado, na barra de escopo. O usuário escolhe
apenas o período.

Isso elimina por construção a regra mais frágil das outras propostas: o prompt
não *precisa* ser restringido ao recorte, ele já nasce restrito. Pergunte sobre
outra jornada e a resposta aponta para a aba IA Panorâmica *dela*.

**As sugestões de melhoria apontam para o fluxo que está na aba ao lado** —
clicar no nó indicado leva para "Dados da jornada". Insight e edição no mesmo
lugar.

## Arquivos

- `index.html` — protótipo completo (estilos + markup + JS)
- `app.css` — tokens do DS
- `ESPEC_CCM-3269_ia_panoramica.md` — spec de origem

## Pontos `[a definir]` prototipados com valor provisório

Os mesmos das outras: limite mínimo de volume (120 conversas), periodicidades,
desfechos dos argumentos (acordo fechado / retorno agendado / recusa) e formato
de exportação.

Decisão nova desta proposta, que precisa ser validada: **o que acontece com as
análises já geradas quando o usuário desliga o toggle.** Aqui a aba some e as
análises ficam indisponíveis, sem serem apagadas — mas isso é uma escolha do
protótipo, não do card.
# vonexaiux
