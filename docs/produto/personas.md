# Personas do ExpenseFlow

## Objetivo do documento

Descrever os principais perfis de usuários do ExpenseFlow, seus contextos, objetivos, dificuldades e necessidades, orientando a definição dos requisitos e da experiência de uso.

As personas são provisórias e baseadas nas hipóteses do planejamento. Suas características deverão ser revisadas conforme houver contato com usuários reais.

## Perfis e etapas

| Perfil | Etapa prevista |
|---|---|
| Colaborador | Inicial |
| Gestor | Inicial |
| Administrador | Inicial |
| Financeiro | Evolução |
| Auditor | Evolução |

Uma mesma pessoa pode exercer mais de um papel. Por exemplo, um profissional do financeiro pode assumir a administração do sistema. As permissões de cada papel serão detalhadas nos requisitos de acesso.

A classificação como perfil inicial não significa que todas as suas necessidades serão atendidas na primeira versão. O atendimento seguirá a evolução das funcionalidades prevista no projeto.

## Colaborador

**Etapa:** perfil inicial do produto.

**Contexto:** realiza gastos a trabalho utilizando recursos próprios ou um adiantamento fornecido pela empresa. Acumula notas fiscais e, ao retornar, registra as despesas em formulário de papel ou planilha para solicitar reembolso ou prestar contas.

**Objetivo:** registrar e comprovar os gastos realizados a trabalho, recebendo o reembolso quando utilizar recursos próprios ou prestando contas quando utilizar um adiantamento.

**Dificuldades:** precisa registrar várias despesas de uma vez, consultando os comprovantes acumulados.

**Necessidades:**

- Registrar despesas ao longo do trabalho.
- Manter os comprovantes organizados.
- Acompanhar a análise das despesas.
- Acompanhar o reembolso quando utilizar recursos próprios.
- Acompanhar a prestação de contas quando utilizar um adiantamento.

**Atividades principais previstas no ExpenseFlow:**

- Cadastrar despesas.
- Anexar comprovantes.
- Enviar despesas para aprovação.
- Consultar o andamento das despesas.
- Acompanhar reembolsos, quando utilizar recursos próprios.
- Prestar contas de adiantamentos, quando essa funcionalidade estiver disponível.

**Hipóteses a validar:** confirmar o uso de recursos próprios e adiantamentos, o acúmulo de comprovantes, as dificuldades de registro e as necessidades de acompanhamento.

## Gestor

**Etapa:** perfil inicial do produto.

**Contexto:** recebe planilhas e comprovantes de despesas de diversos colaboradores para conferir os gastos e decidir sobre sua aprovação.

**Objetivo:** decidir sobre a aprovação das despesas, verificando a correspondência entre os registros e seus comprovantes.

**Dificuldades:** precisa conferir um grande volume de planilhas e comprovantes de diferentes colaboradores. A quantidade de informações torna a análise trabalhosa e pode ocasionar erros na comparação dos valores e na associação entre despesas e comprovantes.

**Necessidades:**

- Consultar as despesas de forma organizada por colaborador.
- Visualizar cada registro junto ao respectivo comprovante.
- Identificar quais despesas ainda aguardam análise.

**Atividades principais no ExpenseFlow:**

- Consultar despesas aguardando aprovação.
- Conferir os comprovantes de gastos.
- Comparar os valores dos comprovantes com os valores informados.
- Aprovar ou rejeitar despesas, informando o motivo da rejeição.
- Consultar o histórico das decisões.

**Hipóteses a validar:** confirmar o volume de despesas recebido, as dificuldades de conferência e a forma como o gestor organiza sua análise.

## Administrador

**Etapa:** perfil inicial do produto.

**Contexto:** profissional da área financeira ou gestor que conhece os processos de adiantamento e reembolso da empresa e assume a responsabilidade pela configuração do ExpenseFlow.

**Objetivo:** manter as configurações e os acessos do sistema adequados à organização e ao processo de gestão de despesas da empresa.

**Dificuldades:**

- Manter os acessos atualizados conforme admissões, desligamentos e mudanças de área ou responsabilidade dos colaboradores.
- Atribuir as permissões corretas, evitando que usuários tenham acesso indevido ou fiquem sem acesso às funções necessárias ao seu trabalho.

**Necessidades:**

- Consultar e atualizar a situação de acesso dos usuários.
- Identificar e ajustar os papéis e as permissões atribuídos a cada usuário.
- Manter os dados da empresa e os cadastros utilizados no registro das despesas.
- Configurar as políticas básicas de despesas da empresa.

**Atividades principais no ExpenseFlow:**

- Cadastrar e atualizar os dados e as configurações da empresa.
- Cadastrar, editar, ativar e desativar usuários.
- Atribuir papéis e permissões.
- Manter departamentos, centros de custo e categorias de despesas.
- Configurar políticas básicas de despesas, conforme as funcionalidades disponíveis.

**Hipóteses a validar:** confirmar quem assumirá essa responsabilidade e se a atualização dos acessos e a atribuição de permissões representam dificuldades reais em sua rotina.

## Financeiro

**Etapa:** perfil de evolução do produto.

**Contexto:** profissional da área financeira responsável por acompanhar as despesas aprovadas e processar os reembolsos aos colaboradores.

**Objetivo:** organizar os reembolsos e manter o acompanhamento dos pagamentos atualizado.

**Necessidades principais:**

- Consultar despesas aprovadas que aguardam pagamento.
- Agrupar despesas em um reembolso.
- Consultar os valores e os comprovantes relacionados.
- Registrar o pagamento e acompanhar o histórico dos reembolsos.

**Hipóteses a validar:** confirmar como os reembolsos são organizados, quem registra os pagamentos e quais informações são necessárias para executar essas atividades. A participação do Financeiro no processo de adiantamentos será detalhada junto à evolução dessa funcionalidade.

## Auditor

**Etapa:** perfil de evolução do produto.

**Contexto:** profissional responsável por consultar o histórico das operações e verificar a rastreabilidade do processo de despesas, aprovações e reembolsos.

**Objetivo:** verificar o que ocorreu em cada operação, quem realizou as ações e quando elas aconteceram.

**Necessidades principais:**

- Consultar os registros das despesas e seus comprovantes.
- Consultar o histórico de aprovações e rejeições, incluindo responsáveis, datas e comentários.
- Consultar o histórico dos reembolsos.
- Acessar a trilha de auditoria das operações relevantes.

**Hipóteses a validar:** confirmar quais informações serão consultadas, o alcance do acesso desse perfil e como os registros serão utilizados nas verificações.

## Revisão das personas

As personas deverão ser revisadas quando novas informações sobre os usuários estiverem disponíveis ou quando as funcionalidades relacionadas forem detalhadas.

Os perfis Financeiro e Auditor serão aprofundados nas respectivas etapas de evolução. As regras de adiantamento e prestação de contas também serão detalhadas posteriormente.