# Estudo de Caso de QA — Projeto Vivian

> **Objetivo deste documento:** apresentar evidências de práticas de qualidade aplicadas durante o desenvolvimento de um projeto pessoal. Este material descreve o contexto técnico e cenários de teste relevantes; não deve ser interpretado como comprovação de que todos os cenários manuais abaixo já foram executados.

## 1. Visão geral do projeto

**Vivian** é uma aplicação web de inteligência de estilo pessoal. O projeto inclui funcionalidades de perfil e descoberta de estilo, classificação de cores sazonais e gerenciamento de guarda-roupa.

O projeto foi desenvolvido em etapas, com atenção a persistência de dados, autenticação, isolamento por usuário, consistência da interface e prevenção de regressões.

### Tecnologias citadas no projeto

- React / Vinext
- TypeScript
- Cloudflare Workers
- Cloudflare D1
- Drizzle ORM
- Supabase Auth

> A lista descreve a arquitetura reportada para o projeto. Antes de publicar este portfólio, confirme no repositório quais tecnologias e componentes continuam presentes na versão atual.

## 2. Atividades de qualidade consideradas

A estratégia de QA contempla:

- **Testes funcionais:** verificar se cada funcionalidade atende ao comportamento esperado.
- **Testes de regressão:** verificar se mudanças recentes não quebram fluxos já existentes.
- **Testes de persistência:** conferir se os dados permanecem corretos após atualizar ou reabrir a aplicação.
- **Testes de autenticação e autorização:** validar sessão, expiração e acesso apenas aos dados permitidos para cada usuário.
- **Testes de interface e estados:** validar carregamento, ausência de resultados, erros, confirmação de ações e atualização visual.
- **Testes de classificação:** conferir entradas, resultados, consistência e apresentação da classificação sazonal de cores.

## 3. Exemplo de evolução: Color Engine V3

Uma etapa do projeto introduziu uma versão aprimorada do mecanismo de classificação de coloração pessoal, com classificação ponderada entre 12 estações, nível de confiança, critérios de desempate e geração de paletas. O resultado deveria permanecer consistente nas diferentes telas que o apresentam.

O registro de desenvolvimento disponível para esta etapa informou **151 testes automatizados passando**, com verificação de TypeScript e build aprovados naquele momento. Esse número é um registro histórico, não uma garantia do estado atual do repositório. Antes de utilizá-lo como evidência pública, execute novamente a suíte e registre a data, o comando e o resultado.

### Riscos de qualidade abordados

- Resultado diferente entre etapas do fluxo e perfil.
- Empate ou baixa confiança sem tratamento coerente.
- Paleta não correspondente à estação classificada.
- Regressão na apresentação do resultado.
- Duplicação de rótulos na interface.

## 4. Casos de teste propostos

Os casos a seguir são uma base de documentação. Marque-os como **executados** somente depois de realizar o teste e registrar evidência real.

| ID | Área | Cenário | Resultado esperado | Tipo |
|---|---|---|---|---|
| TC-001 | Autenticação | Entrar com credenciais válidas | Usuário autenticado e direcionado ao fluxo esperado | Funcional |
| TC-002 | Autenticação | Tentar entrar com credenciais inválidas | Acesso negado e mensagem apropriada, sem expor dados sensíveis | Negativo |
| TC-003 | Sessão | Atualizar uma página com sessão válida | Sessão preservada ou renovada conforme a implementação | Regressão / sessão |
| TC-004 | Autorização | Usuário tenta acessar dados de outro usuário | Acesso bloqueado; dados alheios não são retornados | Segurança |
| TC-005 | Perfil | Salvar preferências de estilo | Dados persistidos e exibidos corretamente ao reabrir | Funcional / persistência |
| TC-006 | Color Engine | Concluir o questionário com respostas válidas | Classificação e paleta exibidas de acordo com as regras implementadas | Funcional |
| TC-007 | Color Engine | Reabrir o resultado em outra tela do perfil | Resultado consistente entre as telas | Regressão |
| TC-008 | Color Engine | Cenário de pontuação próxima entre duas estações | Desempate e confiança tratados conforme as regras definidas | Limite |
| TC-009 | Guarda-roupa | Criar uma peça com dados válidos | Peça aparece na lista e continua disponível após recarregar | Funcional / persistência |
| TC-010 | Guarda-roupa | Pesquisar por termo sem correspondência | Estado vazio claro, sem erro de interface | Interface |
| TC-011 | Guarda-roupa | Favoritar e desfavoritar uma peça | Estado do favorito atualizado e persistido conforme esperado | Funcional |
| TC-012 | Guarda-roupa | Cancelar uma exclusão na confirmação | Peça permanece no guarda-roupa | Funcional |
| TC-013 | Guarda-roupa | Confirmar exclusão de uma peça | Peça é removida conforme as regras do produto | Funcional |
| TC-014 | Erros | Simular falha em uma operação de gravação | Mensagem compreensível e estado da interface recuperável | Resiliência |
| TC-015 | Regressão | Executar os fluxos principais após alteração no perfil ou classificação | Funcionalidades existentes continuam operando | Regressão |

## 5. Modelo de registro de execução

Use uma cópia desta tabela para cada rodada de testes. Não registre um caso como aprovado sem executar o cenário.

| Campo | Preenchimento |
|---|---|
| ID do caso | Ex.: TC-001 |
| Data e ambiente | Data, navegador, ambiente/versão |
| Pré-condições | Estado necessário antes do teste |
| Passos executados | Passos reproduzíveis |
| Resultado esperado | Comportamento definido |
| Resultado obtido | O que aconteceu de fato |
| Status | Aprovado / Reprovado / Bloqueado / Não executado |
| Evidência | Captura de tela, log ou link, sem dados sensíveis |
| Defeito relacionado | ID ou link do defeito, se houver |

## 6. Modelo de relatório de defeito

**Título:** [Área] resumo objetivo do problema

- **Ambiente:** versão, navegador e ambiente.
- **Pré-condições:** estado necessário para reproduzir.
- **Passos para reproduzir:** lista numerada e objetiva.
- **Resultado atual:** comportamento observado.
- **Resultado esperado:** comportamento correto.
- **Severidade / prioridade:** classificar conforme impacto e urgência.
- **Evidências:** captura, log ou vídeo curto, removendo dados pessoais e segredos.
- **Frequência:** sempre, intermitente ou uma única ocorrência.
- **Status:** aberto, em análise, corrigido ou retestado.

Não crie um bug fictício para preencher o portfólio. Se ainda não houver defeito reproduzido, mantenha este modelo como exemplo de documentação.

## 7. Evidências que fortalecem o portfólio

Para transformar este estudo em uma demonstração verificável:

1. Execute os comandos reais de testes, TypeScript e build no estado atual do projeto.
2. Registre data, comando, total de testes e resultado, sem misturar resultados de execuções diferentes.
3. Execute os casos manuais prioritários e preencha o resultado obtido.
4. Anexe capturas de tela sem credenciais, tokens, dados pessoais ou informações privadas.
5. Se encontrar um defeito real, documente-o com passos reproduzíveis e evidência.
6. Se possível, registre um pequeno ciclo de correção: defeito reproduzido, correção, reteste e regressão.

## 8. Limitações e transparência

- Vivian é um projeto pessoal de desenvolvimento; não representa experiência profissional de QA em uma empresa.
- Os casos da seção 4 são cenários propostos até que sua execução seja registrada.
- O total de 151 testes, TypeScript e build aprovados refere-se a um registro anterior da etapa Color Engine V3. O estado atual deve ser verificado antes de divulgar esses números como atuais.
- A existência de testes automatizados não comprova, por si só, que autenticação real, fluxos ponta a ponta, usabilidade e todos os cenários manuais foram validados.

## 9. Como apresentar o projeto em um portfólio

**Projeto pessoal — Vivian | Qualidade de Software (QA)**

Documentação de cenários funcionais, regressão, persistência, autenticação/autorização e classificação de cores em uma aplicação web. Organização de resultados esperados, riscos, critérios de validação e modelos de registro de defeitos. Os resultados de execução devem ser apresentados com evidências e data de validação.

---

**Próxima ação recomendada:** validar a suíte atual no repositório e preencher pelo menos cinco casos de teste com resultado observado e evidência real.
