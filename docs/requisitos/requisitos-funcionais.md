# Requisitos funcionais do ExpenseFlow

## RF01 — Autenticação

### Objetivo

Permitir que usuários cadastrados acessem o sistema por meio de e-mail e senha.

### Atores

Todos os usuários cadastrados, conforme os perfis disponíveis em cada etapa do produto.

### Etapa

Na versão inicial, a autenticação será realizada por e-mail e senha, sem segundo fator de autenticação.

A inclusão de segundo fator de autenticação e a evolução para OAuth2/OIDC ficam previstas para etapas futuras, com regras e implementação a detalhar.

### Acesso ao sistema

O sistema deve permitir que o usuário informe seu e-mail e sua senha para iniciar uma sessão.

As contas serão cadastradas pelo Administrador, conforme o RF03 — Usuários.

O e-mail informado no login deve seguir as regras de
[tratamento do e-mail](#tratamento-do-e-mail) definidas no RF03 — Usuários.

### Regras de acesso

- O acesso exige uma conta cadastrada e ativa.
- O e-mail e a senha informados devem corresponder às credenciais da conta.
- Após a autenticação, o acesso às funcionalidades deve respeitar os papéis e as permissões do usuário.

### Critérios de aceite do login

- **CA01:** dado um usuário cadastrado e ativo, quando informar e-mail e senha válidos, o sistema deve iniciar sua sessão e permitir o acesso.
- **CA02:** quando as credenciais forem inválidas, o sistema deve negar o acesso e informar que o e-mail ou a senha está incorreto, sem indicar qual deles.
- **CA03:** quando a conta estiver desativada, o sistema deve negar o acesso, mesmo que as credenciais estejam corretas.
- **CA04:** quando e-mail ou senha não forem preenchidos, o sistema deve solicitar o preenchimento dos campos obrigatórios.

### Pontos a detalhar

- Recuperação de acesso em etapa posterior.

### Primeiro acesso

Após o Administrador cadastrar uma conta, o sistema deve enviar ao e-mail cadastrado uma mensagem informando a criação da conta e contendo um link para definição da primeira senha.

O usuário deve acessar o link, informar a senha desejada e confirmá-la. Após a conclusão, poderá acessar o sistema com seu e-mail e a senha definida, desde que a conta esteja ativa.

### Critérios de aceite do primeiro acesso

- **CA05:** ao cadastrar uma conta, o sistema deve enviar o convite de primeiro acesso ao e-mail informado.
- **CA06:** ao acessar um link válido, o usuário deve conseguir informar e confirmar sua primeira senha.
- **CA07:** quando a senha e sua confirmação forem diferentes, o sistema deve impedir a conclusão e solicitar a correção.
- **CA08:** após definir a senha com sucesso, o usuário deve conseguir se autenticar com suas credenciais, desde que a conta esteja ativa.

### Decisões pendentes do primeiro acesso

- Prazo de validade do convite.
- Condições de utilização e invalidação do link.
- Procedimento para reenviar o convite.
- Regras de composição da senha.

### Regras do convite de primeiro acesso

- O link de criação da primeira senha deve ter validade de 24 horas a partir da emissão.
- O link deve permitir uma única definição de senha. Após a conclusão bem-sucedida, deve ser invalidado.
- Abrir o link sem concluir a definição da senha não deve consumir o convite.
- O Administrador deve poder reenviar o convite enquanto o usuário não tiver definido sua primeira senha.
- O reenvio deve gerar um novo link, com nova validade de 24 horas, e invalidar o convite anterior.
- Links expirados, utilizados ou invalidados não devem permitir a definição da senha.
- A conta deve estar ativa para que o usuário possa concluir a definição da primeira senha.

### Critérios de aceite do convite

- **CA09:** antes de completar 24 horas da emissão, um convite não utilizado nem invalidado deve permitir a definição da primeira senha, desde que a conta esteja ativa.
- **CA10:** ao completar 24 horas da emissão, o convite deve ser considerado expirado e não deve permitir a definição da senha.
- **CA11:** após a definição bem-sucedida da senha, uma nova tentativa de utilização do mesmo link deve ser recusada.
- **CA12:** abrir o link e sair sem concluir a definição da senha deve mantê-lo utilizável, respeitando sua validade e as demais condições.
- **CA13:** quando o Administrador reenviar o convite, o sistema deve enviar um novo link ao e-mail cadastrado e invalidar o anterior.
- **CA14:** quando o convite estiver expirado ou invalidado por reenvio, o sistema deve informar que ele não pode mais ser utilizado e orientar o usuário a solicitar um novo convite ao Administrador.
- **CA15:** quando a conta estiver desativada, o sistema deve impedir a definição da primeira senha.

### Política de senha da versão inicial

- A senha deve conter de 8 a 128 caracteres.
- Devem ser permitidos letras, inclusive acentuadas, números, símbolos e espaços.
- Não deve ser exigida uma combinação obrigatória de letras maiúsculas, minúsculas, números ou símbolos.
- Senhas presentes na lista de senhas comuns ou comprometidas devem ser recusadas.
- Não deve ser exigida troca periódica de senha. A redefinição deve ser exigida quando houver evidência de comprometimento.
- O sistema deve permitir colagem e preenchimento por gerenciadores de senhas.
- A criação e a redefinição devem exigir confirmação da senha.
- O sistema deve informar as regras não atendidas e não deve truncar senhas silenciosamente.
- A política deve ser aplicada na criação da primeira senha e na futura funcionalidade de redefinição.
- Na versão inicial, a política será fixa e não poderá ser alterada pelo Administrador.

### Critérios de aceite da política de senha

- **CA16:** senhas com menos de 8 ou mais de 128 caracteres devem ser recusadas, informando o limite aplicável.
- **CA17:** senhas entre 8 e 128 caracteres que atendam às demais regras devem ser aceitas, mesmo sem combinação de diferentes tipos de caracteres.
- **CA18:** letras acentuadas e espaços devem ser aceitos como parte da senha.
- **CA19:** senhas presentes na lista de bloqueio devem ser recusadas, com orientação para escolher outra senha.
- **CA20:** o usuário deve conseguir colar a senha e utilizar preenchimento automático por um gerenciador.
- **CA21:** a senha não deve expirar apenas pelo tempo decorrido desde sua definição.

### Evolução da política de senha

Em uma versão futura, o Administrador da empresa poderá configurar o comprimento mínimo exigido para as senhas.

Os limites permitidos para essa configuração, os demais parâmetros configuráveis e o tratamento das contas existentes serão definidos nessa etapa.

### Decisão registrada

A versão inicial adota o mínimo de 8 caracteres por decisão de escopo do projeto, mesmo sem segundo fator de autenticação. Esse mínimo fica abaixo da recomendação atual do NIST de 15 caracteres para autenticação somente por senha.

### Encerramento da sessão

O sistema deve disponibilizar a opção “Sair” na área de conta ou no menu do aplicativo.

A saída deve encerrar somente a sessão atual, preservando eventuais sessões do usuário em outros dispositivos.

### Regras de encerramento

- Quando não houver alterações não salvas, a saída deve ocorrer sem solicitar confirmação.
- Quando houver alterações não salvas, o aplicativo deve avisar que serão descartadas e permitir confirmar ou cancelar a saída.
- Após a confirmação, quando necessária, o aplicativo deve solicitar a invalidação da sessão atual no servidor.
- O encerramento confirmado pelo servidor deve impedir novos acessos e a renovação por meio das credenciais daquela sessão.
- O aplicativo deve remover as credenciais e os dados sensíveis temporários da sessão e retornar à tela de login.
- Despesas e comprovantes já salvos no servidor devem ser preservados.
- Uma sessão já expirada ou invalidada não deve impedir a limpeza local e o retorno à tela de login.
- A opção de encerrar todas as sessões não faz parte da versão inicial.

### Critérios de aceite do encerramento

- **CA22:** ao selecionar “Sair” sem alterações não salvas, o aplicativo deve iniciar o encerramento sem pedir confirmação.
- **CA23:** quando houver alterações não salvas, o aplicativo deve informar sobre o descarte e oferecer as opções de confirmar ou cancelar.
- **CA24:** ao cancelar a saída, o usuário deve permanecer na tela atual, com as alterações preservadas.
- **CA25:** ao confirmar a saída, as alterações não salvas devem ser descartadas e o encerramento deve prosseguir.
- **CA26:** após o encerramento confirmado pelo servidor, as credenciais da sessão encerrada não devem permitir acesso à API nem renovação da sessão.
- **CA27:** após a saída, voltar à tela anterior ou reabrir o aplicativo não deve permitir visualizar os dados protegidos da sessão encerrada sem nova autenticação.
- **CA28:** o encerramento da sessão atual não deve invalidar sessões do mesmo usuário em outros dispositivos.
- **CA29:** despesas e comprovantes já salvos no servidor devem permanecer disponíveis após uma nova autenticação.
- **CA30:** quando a sessão já estiver expirada ou invalidada, a opção “Sair” deve limpar os dados locais da sessão e retornar ao login.

### Pendência do encerramento

Definir o comportamento quando não houver conexão ou não for possível confirmar a invalidação da sessão no servidor.

### Encerramento sem confirmação do servidor

- Quando não houver conexão ou ocorrer uma falha que impeça confirmar a invalidação da sessão no servidor, o aplicativo deve permitir a saída local.
- A saída local deve remover as credenciais e os dados sensíveis temporários da sessão e retornar à tela de login.
- O aplicativo não deve aguardar indefinidamente uma resposta do servidor para concluir a saída local.
- O aplicativo deve informar: “Você saiu deste aparelho, mas não foi possível confirmar o encerramento da sessão no servidor.”
- A saída local não deve ser registrada como confirmação de invalidação da sessão no servidor.
- O aviso sobre alterações não salvas deve continuar sendo aplicado antes de prosseguir com a saída.

### Critérios de aceite da saída local

- **CA31:** quando estiver sem conexão, o usuário deve conseguir sair localmente, com remoção das credenciais e dos dados sensíveis temporários da sessão.
- **CA32:** quando a solicitação de encerramento falhar ou exceder o tempo limite de comunicação, o aplicativo deve concluir a saída local e retornar ao login.
- **CA33:** quando não houver confirmação do servidor, o aplicativo deve apresentar a mensagem que distingue a saída do aparelho do encerramento remoto.
- **CA34:** após a saída local, a reabertura do aplicativo deve exigir nova autenticação para acessar os dados protegidos.

### Permanência da sessão

- O aplicativo deve manter a sessão entre fechamentos e reaberturas, enquanto ela permanecer válida.
- A senha do usuário não deve ser armazenada no aparelho para realizar login automático.
- As credenciais necessárias à continuidade da sessão devem ser armazenadas de forma segura.
- Ao reabrir o aplicativo, o acesso às funcionalidades protegidas deve depender da validade da sessão, verificada pelo servidor, ou de sua renovação válida.
- Quando a sessão estiver expirada e não puder ser renovada, ou tiver sido revogada, o aplicativo deve solicitar nova autenticação.
- Uma conta desativada não deve obter acesso nem renovar sua sessão.

### Critérios de aceite da permanência

- **CA35:** ao fechar e reabrir o aplicativo com uma sessão válida e conexão disponível, o usuário deve conseguir continuar o uso sem informar novamente e-mail e senha.
- **CA36:** quando a sessão não for mais válida e não puder ser renovada, o aplicativo deve solicitar nova autenticação.
- **CA37:** após o usuário selecionar “Sair”, reabrir o aplicativo não deve restaurar a sessão encerrada localmente.
- **CA38:** quando a conta estiver desativada, tentativas de acesso às funcionalidades protegidas e de renovação da sessão devem ser recusadas pelo servidor.

### Duração da sessão

- A sessão deve expirar após 1 hora sem atividade do usuário ou ao completar 8 horas desde a autenticação, o que ocorrer primeiro.
- Os limites devem ser controlados e verificados pelo servidor.
- Uma requisição autenticada decorrente de uma ação do usuário deve atualizar o momento da última atividade, desde que a sessão ainda esteja válida.
- Chamadas automáticas em segundo plano e renovações de credenciais não devem reiniciar o prazo de inatividade.
- Fechar e reabrir o aplicativo não deve reiniciar nenhum dos prazos.
- A renovação de credenciais não deve alterar o início da sessão nem ultrapassar seus limites de duração.
- Ao atingir qualquer limite, a sessão deve ser invalidada no servidor e não poderá ser renovada. O usuário deverá se autenticar novamente.
- Ao identificar a expiração, o aplicativo deve limpar as credenciais e os dados sensíveis temporários da sessão, retornar ao login e informar: “Sua sessão expirou. Entre novamente para continuar.”
- Despesas e comprovantes já salvos no servidor devem ser preservados.

### Critérios de aceite da duração

- **CA39:** ao completar 1 hora desde a última atividade válida, a sessão deve ser considerada expirada, mesmo que ainda não tenha completado 8 horas.
- **CA40:** uma ação autenticada do usuário antes da expiração deve reiniciar a contagem de inatividade, sem alterar o limite absoluto.
- **CA41:** ao completar 8 horas desde a autenticação, a sessão deve expirar, mesmo que tenha ocorrido atividade recente.
- **CA42:** fechar e reabrir o aplicativo deve preservar os prazos originais da sessão.
- **CA43:** chamadas automáticas e renovações de credenciais não devem prolongar o limite de inatividade ou o limite absoluto.
- **CA44:** após a expiração, tentativas de acesso ou renovação com as credenciais daquela sessão devem ser recusadas pelo servidor.
- **CA45:** ao identificar a expiração, o aplicativo deve retornar ao login, informar o ocorrido e impedir o acesso aos dados protegidos até uma nova autenticação.

### Decisão registrada

A versão inicial adota 1 hora de inatividade e 8 horas de duração absoluta como escolha do projeto para equilibrar proteção e conveniência. O intervalo de inatividade é mais permissivo que os exemplos de 15 a 30 minutos citados pela OWASP para aplicações de baixo risco e deverá ser reavaliado conforme a evolução do produto.

### Renovação das credenciais de acesso

- O aplicativo deve permitir a renovação automática das credenciais de acesso, quando necessária, sem solicitar novamente a senha enquanto a sessão permanecer válida.
- Antes de autorizar a renovação, o servidor deve verificar se a conta está ativa e se a sessão não expirou nem foi revogada.
- A renovação deve preservar o limite de 1 hora de inatividade e o limite absoluto de 8 horas desde a autenticação.
- A renovação automática não deve ser considerada atividade do usuário nem reiniciar os prazos da sessão.
- Quando o servidor confirmar que a sessão expirou ou foi revogada, o aplicativo deve limpar as credenciais e os dados sensíveis temporários e solicitar nova autenticação.
- Quando o servidor informar que a conta está desativada, o aplicativo deve encerrar o acesso local e informar que o acesso está indisponível.
- Falhas de conexão ou indisponibilidade do servidor não devem ser interpretadas automaticamente como expiração da sessão.
- Quando não for possível renovar por falha de comunicação, o aplicativo deve informar o problema e permitir uma nova tentativa, sem liberar operações que dependam de uma credencial válida.

### Critérios de aceite da renovação

- **CA46:** quando a renovação for necessária e a conta e a sessão estiverem válidas, o aplicativo deve obter novas credenciais sem solicitar novamente a senha.
- **CA47:** a renovação não deve alterar o horário inicial da sessão nem o registro da última atividade do usuário.
- **CA48:** ao atingir 1 hora de inatividade ou 8 horas desde a autenticação, o servidor deve recusar a renovação e exigir nova autenticação.
- **CA49:** sessões revogadas e contas desativadas não devem obter novas credenciais.
- **CA50:** quando houver falha de comunicação durante a renovação, o aplicativo deve informar o problema sem apresentar a sessão como expirada apenas por essa falha.
- **CA51:** quando a comunicação for restabelecida, uma nova tentativa de renovação deve verificar novamente a validade da conta e da sessão, respeitando os prazos originais.

### Definições técnicas posteriores

O mecanismo de autenticação, o formato e a duração das credenciais de acesso e a eventual utilização e rotação de refresh tokens serão definidos na arquitetura, respeitando os comportamentos deste requisito.

### Consulta dos dados do próprio usuário

- O sistema deve disponibilizar uma área de conta para o usuário autenticado consultar seus próprios dados.
- Devem ser apresentados nome, e-mail, empresa e papéis atribuídos.
- Os dados devem ser apresentados somente para leitura na versão inicial.
- As alterações cadastrais e de papéis devem ser realizadas pelo Administrador, conforme o RF03 — Usuários.
- A consulta deve respeitar as regras de validade da sessão e de acesso da conta.

### Critérios de aceite da consulta dos próprios dados

- **CA52:** com uma sessão válida, o usuário deve conseguir consultar seu nome, e-mail, empresa e papéis atribuídos.
- **CA53:** a área de conta não deve disponibilizar edição desses dados na versão inicial.
- **CA54:** a consulta dos próprios dados deve retornar as informações do usuário autenticado, sem permitir consultar outra conta por alteração de parâmetros da solicitação.
- **CA55:** uma consulta atualizada deve refletir os dados e papéis vigentes no servidor, incluindo alterações realizadas pelo Administrador.
- **CA56:** consultas sem autenticação válida ou realizadas por uma conta desativada devem ser recusadas.


## RF02 — Empresa

### Objetivo

Permitir que o Administrador mantenha os dados e as configurações da empresa que utiliza o ExpenseFlow.

### Ator

Administrador.

### Etapa

A versão inicial atenderá uma única empresa. O atendimento a múltiplas empresas fica para uma evolução, com regras a detalhar.

### Regras iniciais

- O sistema deve manter um único cadastro de empresa na V1.
- Os usuários e os registros de negócio devem estar vinculados a essa empresa.
- A manutenção dos dados e das configurações da empresa deve ser restrita ao Administrador.
- A V1 não deve oferecer seleção ou alternância entre empresas.
- O sistema deve impedir a criação de uma segunda empresa.

### Configuração inicial

- O cadastro da única empresa e a conta do primeiro Administrador devem ser criados por um procedimento restrito ao responsável pela implantação.
- A V1 não deve disponibilizar cadastro público de empresas ou usuários.
- O primeiro Administrador deve estar vinculado à empresa cadastrada.
- Após a criação da conta, o sistema deve enviar o convite de definição da primeira senha, conforme as regras do RF01.
- Após definir sua senha e autenticar-se, o Administrador deve poder atualizar os dados da empresa e cadastrar os demais usuários.
- A repetição do procedimento inicial não deve gerar uma segunda empresa nem duplicar a conta do primeiro Administrador.

### Critérios de aceite da configuração inicial

- **CA01:** em um ambiente ainda não configurado, o procedimento inicial deve permitir criar a empresa e a conta do primeiro Administrador.
- **CA02:** a conta criada deve estar vinculada à empresa e possuir o papel de Administrador.
- **CA03:** o primeiro Administrador deve receber o convite para definir sua senha, respeitando a validade e as regras de utilização do RF01.
- **CA04:** executar novamente o procedimento não deve criar registros duplicados de empresa ou do primeiro Administrador.
- **CA05:** visitantes não autenticados não devem conseguir cadastrar empresas ou usuários por uma funcionalidade pública.

### Dados da empresa

| Campo | Obrigatoriedade | Descrição |
|---|---|---|
| Razão social | Obrigatório | Nome formal da empresa |
| Nome fantasia | Opcional | Nome de uso da empresa |
| CNPJ | Obrigatório | Identificador da empresa brasileira |
| Moeda padrão | Obrigatório e fixo | Real brasileiro, identificado pelo código BRL |

### Regras dos dados cadastrais

- O cadastro inicial deve conter razão social e CNPJ.
- A ausência de nome fantasia não deve impedir o cadastro.
- A moeda da empresa deve ser definida automaticamente como BRL.
- A V1 não deve permitir selecionar outra moeda nem realizar conversão cambial.
- Os valores monetários das despesas, adiantamentos e reembolsos, conforme essas funcionalidades forem disponibilizadas, devem utilizar BRL.
- A atualização dos dados da empresa deve ser restrita ao Administrador.

### Critérios de aceite dos dados cadastrais

- **CA06:** o sistema deve impedir o cadastro da empresa quando razão social ou CNPJ não forem informados.
- **CA07:** o sistema deve permitir o cadastro sem nome fantasia.
- **CA08:** a empresa deve ser cadastrada com BRL como moeda padrão, sem exigir seleção pelo Administrador.
- **CA09:** tentativas de configurar uma moeda diferente de BRL devem ser recusadas.
- **CA10:** usuários sem o papel de Administrador não devem conseguir alterar os dados da empresa.

### Pontos ainda a detalhar

- Configurações e políticas básicas de despesas.


### Alteração dos dados da empresa

- O Administrador deve poder alterar a razão social e o nome fantasia.
- A razão social deve continuar sendo obrigatória após qualquer alteração.
- O nome fantasia deve permanecer opcional, podendo ser removido.
- O CNPJ deve ser apresentado somente para leitura após o cadastro inicial.
- O servidor deve recusar tentativas de alteração do CNPJ pelas operações normais de atualização cadastral.
- A moeda deve permanecer fixa em BRL.
- Eventuais correções do CNPJ devem exigir um procedimento controlado, fora da edição cadastral comum. Esse procedimento será detalhado antes de sua disponibilização.

### Critérios de aceite da alteração cadastral

- **CA11:** um Administrador autenticado deve conseguir atualizar a razão social e o nome fantasia.
- **CA12:** uma tentativa de deixar a razão social vazia deve ser recusada, preservando o valor anterior.
- **CA13:** o Administrador deve conseguir remover o nome fantasia sem impedir a atualização do cadastro.
- **CA14:** a tela de edição não deve permitir alterar o CNPJ.
- **CA15:** uma tentativa de alterar o CNPJ diretamente pela API deve ser recusada, preservando o cadastro existente.
- **CA16:** a atualização cadastral deve preservar BRL como moeda da empresa.

### Validação do CNPJ

- O sistema deve aceitar CNPJs numéricos e alfanuméricos.
- A entrada deve aceitar o identificador sem pontuação ou com a máscara padrão `XX.XXX.XXX/XXXX-DD`.
- Letras minúsculas devem ser convertidas para maiúsculas antes da validação.
- Após a normalização, o CNPJ deve conter exatamente 14 caracteres.
- As primeiras 12 posições devem aceitar números de 0 a 9 e letras de A a Z.
- As últimas 2 posições devem conter somente números, correspondentes aos dígitos verificadores.
- O cálculo dos dígitos verificadores deve seguir a documentação técnica da Receita Federal, mantendo compatibilidade com o formato numérico.
- Caracteres não permitidos e formatações diferentes das aceitas devem ser recusados, sem remoção silenciosa de conteúdo inválido.
- O CNPJ deve ser armazenado como texto, sem pontuação, com letras maiúsculas e preservando zeros à esquerda.
- A validação deve ser executada no servidor, inclusive durante a configuração inicial.
- A V1 não deve realizar consulta cadastral automática. A aprovação na validação local não comprova existência, situação cadastral ou correspondência com a razão social.
- CNPJs numéricos existentes não devem ser convertidos para identificadores alfanuméricos.

### Critérios de aceite da validação do CNPJ

- **CA17:** um CNPJ numérico com formato e dígitos verificadores válidos deve ser aceito.
- **CA18:** um CNPJ alfanumérico com formato e dígitos verificadores válidos deve ser aceito.
- **CA19:** o mesmo CNPJ, informado com a máscara padrão ou sem pontuação, deve resultar no mesmo valor armazenado.
- **CA20:** letras minúsculas devem ser normalizadas para maiúsculas, sem alterar a identidade do CNPJ.
- **CA21:** entradas com quantidade incorreta de caracteres, caracteres não permitidos ou letras nas duas últimas posições devem ser recusadas.
- **CA22:** um CNPJ com dígitos verificadores incorretos deve ser recusado.
- **CA23:** zeros à esquerda devem ser preservados durante a entrada, o armazenamento e a consulta.
- **CA24:** uma tentativa de cadastro que não passe pela interface também deve ser validada pelo servidor.
- **CA25:** a validação local deve funcionar sem depender de um serviço externo e não deve apresentar o CNPJ como consultado ou confirmado pela Receita Federal.

### Referência técnica

[Receita Federal — Cálculo do dígito verificador do CNPJ alfanumérico](https://www.gov.br/receitafederal/pt-br/centrais-de-conteudo/publicacoes/documentos-tecnicos/cnpj)

### Política de exigência de comprovantes

- Na V1, toda despesa deve possuir pelo menos um comprovante anexado para ser enviada para aprovação.
- A exigência será fixa e não poderá ser desativada pelo Administrador.
- O colaborador deve poder criar e salvar uma despesa como rascunho sem comprovante.
- O envio para aprovação deve considerar somente anexos cujo envio e armazenamento tenham sido concluídos com sucesso.
- A existência de um anexo não representa aprovação de seu conteúdo. O Gestor deve conferir a correspondência entre o comprovante, a despesa e o valor informado.
- Exceções à obrigatoriedade de comprovantes ficam fora do escopo da V1.

### Critérios de aceite da exigência de comprovantes

- **CA26:** o colaborador deve conseguir salvar uma despesa como rascunho sem comprovante, desde que as demais regras de rascunho sejam atendidas.
- **CA27:** ao tentar enviar uma despesa sem comprovante para aprovação, o sistema deve impedir o envio, manter a despesa como rascunho e informar que é necessário anexar um comprovante.
- **CA28:** um anexo com envio em andamento ou que tenha falhado não deve atender à exigência de comprovante.
- **CA29:** uma despesa com pelo menos um comprovante armazenado deve atender à exigência documental para envio, permanecendo sujeita às demais validações.
- **CA30:** o servidor deve aplicar a exigência mesmo quando a solicitação de envio for realizada diretamente pela API.

### Requisitos relacionados

- **RF08 — Comprovante:** inclusão, visualização e remoção de anexos.
- **RF10 — Rascunho:** registro da despesa antes do envio.
- **RF11 — Enviar despesa:** validação do comprovante antes da transição para aprovação.

### Alteração de comprovantes

- O colaborador deve poder adicionar, substituir ou remover comprovantes de suas despesas enquanto estiverem no estado DRAFT.
- A remoção de todos os comprovantes deve ser permitida no rascunho, mas deve impedir o envio para aprovação até que outro comprovante seja anexado.
- Após a saída do estado DRAFT, o colaborador não deve poder adicionar, substituir ou remover comprovantes.
- Os comprovantes devem permanecer disponíveis para consulta conforme as permissões de acesso.
- O servidor deve verificar o estado atual da despesa antes de permitir qualquer alteração nos comprovantes.
- Na V1, a rejeição não deve desbloquear automaticamente a edição dos comprovantes. Um eventual fluxo de correção será definido separadamente.

### Critérios de aceite da alteração de comprovantes

- **CA31:** o colaborador deve conseguir adicionar, substituir e remover comprovantes de uma despesa própria em DRAFT.
- **CA32:** ao remover o último comprovante de um rascunho, a despesa deve permanecer salva, mas seu envio para aprovação deve ser impedido.
- **CA33:** após o envio da despesa, a interface não deve disponibilizar ações de inclusão, substituição ou remoção de comprovantes ao colaborador.
- **CA34:** tentativas de alterar comprovantes diretamente pela API devem ser recusadas quando a despesa não estiver em DRAFT.
- **CA35:** se a despesa tiver sido enviada por outra solicitação, uma tentativa posterior de alteração baseada em uma tela desatualizada deve ser recusada, preservando os comprovantes.
- **CA36:** a rejeição da despesa não deve, por si só, permitir alterações nos comprovantes.

### Requisito relacionado

Esta política deve ser referenciada no RF08 — Comprovante.

### Escopo das políticas da versão inicial

- As políticas básicas da V1 devem abranger a obrigatoriedade de comprovantes no envio para aprovação e as restrições de alteração dos comprovantes após o envio.
- Essas políticas devem ser fixas, sem opção de alteração pelo Administrador.
- A V1 não deve disponibilizar configuração de limites de gastos por categoria.
- Limites por categoria e outras políticas configuráveis ficam previstos para evolução, com regras e critérios de aceite a detalhar.
- Esta delimitação não altera os demais requisitos de aprovação previstos no planejamento.

### Critérios de aceite do escopo das políticas

- **CA37:** o sistema deve aplicar as regras de comprovantes sem depender de configuração prévia pelo Administrador.
- **CA38:** não deve ser possível desativar a obrigatoriedade de comprovantes ou liberar sua alteração após o envio pelas configurações da empresa.
- **CA39:** a V1 não deve apresentar configuração de limites de gastos por categoria.

### Responsabilidade pela correção do CNPJ

- O Administrador deve solicitar a correção quando identificar um erro no CNPJ cadastrado.
- A execução deve ser restrita ao responsável técnico pela implantação.
- O aplicativo não deve disponibilizar a alteração do CNPJ na edição cadastral comum.
- A correção deve se destinar a um erro de cadastro da mesma empresa, não à substituição por outra empresa.
- O novo valor deve atender às regras de validação de CNPJ previstas neste requisito.

### Critérios de aceite da responsabilidade

- **CA40:** o Administrador não deve conseguir alterar o CNPJ pela edição cadastral comum nem pela respectiva operação da API.
- **CA41:** o procedimento técnico de correção deve exigir autorização específica, sem considerar o papel de Administrador do aplicativo suficiente para executá-lo.
- **CA42:** o procedimento deve recusar um novo CNPJ que não atenda às regras de validação previstas no RF02.

### Pontos a detalhar

- Forma de registrar a solicitação e conferir a identidade da empresa.
- Registro do histórico da correção.
- Tratamento da correção antes e depois da existência de movimentações.

### Solicitação e conferência da correção do CNPJ

- A solicitação deve ser feita pelo Administrador e conter:
  - CNPJ atualmente cadastrado.
  - CNPJ correto.
  - Motivo da correção.
  - Comprovante de inscrição da empresa.
- O responsável técnico deve conferir manualmente a documentação antes de executar a alteração.
- A conferência deve verificar se a solicitação corresponde à correção de um erro cadastral da mesma empresa.
- A validação de formato e dígitos verificadores não deve substituir a conferência documental.
- Se as informações forem insuficientes ou apresentarem divergências, a correção não deve ser executada até o esclarecimento.
- A V1 não deve depender de consulta cadastral automática para esse procedimento.

### Histórico da correção do CNPJ

Cada correção executada deve possuir um registro contendo:

- Identificação do Administrador solicitante.
- Identificação do responsável técnico executor.
- Data e hora da alteração.
- CNPJ anterior.
- CNPJ novo.
- Motivo da correção.
- Referência à solicitação e à documentação utilizada na conferência.

### Critérios de aceite da solicitação e do histórico

- **CA43:** uma solicitação sem CNPJ atual, CNPJ correto, motivo ou comprovante de inscrição deve ser considerada incompleta e não deve autorizar a execução.
- **CA44:** a correção deve depender da conferência manual pelo responsável técnico, mesmo que o novo CNPJ passe na validação de formato e dígitos verificadores.
- **CA45:** quando houver divergências que impeçam confirmar a identidade da empresa, a alteração deve permanecer bloqueada até o esclarecimento.
- **CA46:** toda correção executada deve possuir histórico com solicitante, executor, data e hora, valores anterior e novo, motivo e referência à documentação.
- **CA47:** a correção não deve eliminar o registro do CNPJ anteriormente cadastrado no histórico.

### Pontos a detalhar

- Canal e local de armazenamento da solicitação e de sua documentação.
- Tratamento da correção antes e depois da existência de movimentações.

### Correção do CNPJ e existência de movimentações

- Antes de executar a correção, o procedimento deve verificar se existem despesas, adiantamentos ou reembolsos vinculados à empresa, conforme as funcionalidades disponíveis.
- Para essa verificação, despesas em rascunho também devem ser consideradas registros existentes.
- Na ausência dessas movimentações, a correção poderá prosseguir, desde que sejam atendidas as regras de solicitação, conferência documental, autorização e histórico.
- Na existência de movimentações, o procedimento padrão deve bloquear a alteração.
- Casos bloqueados devem ser encaminhados para análise técnica do impacto sobre os registros e o histórico.
- A análise técnica não deve, por si só, liberar a alteração. Uma eventual correção excepcional deve depender de um procedimento específico, definido e aprovado antes da execução.
- Não deve ser permitida a exclusão de movimentações como forma de contornar o bloqueio.

### Critérios de aceite da existência de movimentações

- **CA48:** quando não houver despesas, adiantamentos ou reembolsos registrados, o procedimento deve permitir prosseguir com a correção, desde que os demais requisitos sejam atendidos.
- **CA49:** quando existir qualquer uma dessas movimentações, inclusive despesa em rascunho, o procedimento padrão deve recusar a alteração e preservar o CNPJ atual.
- **CA50:** o bloqueio deve informar que a correção exige análise técnica devido à existência de movimentações.
- **CA51:** o encaminhamento para análise não deve modificar o CNPJ nem os registros existentes.

### Armazenamento da solicitação de correção

- A solicitação e os documentos de conferência devem ser armazenados em chamado ou registro privado, acessível somente aos responsáveis pela análise e execução.
- Cada solicitação deve possuir um identificador, referenciado no histórico da correção.
- A ferramenta utilizada deve permitir restringir o acesso e consultar posteriormente as informações e os documentos da solicitação.
- O repositório público do projeto deve conter somente a descrição do procedimento e, quando necessário, exemplos fictícios, sem documentos reais das empresas.
- A V1 não deve exigir a implementação de um sistema próprio de chamados no ExpenseFlow.

### Critérios de aceite do armazenamento

- **CA52:** a solicitação e sua documentação devem estar disponíveis aos responsáveis autorizados em um registro privado identificado.
- **CA53:** pessoas sem autorização não devem conseguir acessar a solicitação nem seus documentos.
- **CA54:** o histórico de uma correção executada deve referenciar o identificador da solicitação correspondente.
- **CA55:** documentos reais utilizados na correção não devem ser publicados no repositório público do projeto.

## RF03 — Usuários

### Objetivo

Permitir que o Administrador cadastre e mantenha os usuários da empresa, controle sua situação de acesso e atribua seus papéis.

### Ator

Administrador.

### Etapa

Na V1, estarão disponíveis os papéis Colaborador, Gestor e Administrador. Os papéis Financeiro e Auditor serão incluídos nas respectivas etapas de evolução.

### Dados do usuário

| Campo | Obrigatoriedade | Regra |
|---|---|---|
| Nome completo | Obrigatório | Identifica a pessoa |
| E-mail | Obrigatório | Deve ser único entre as contas cadastradas |
| Papéis | Obrigatório | Deve conter pelo menos um papel disponível |
| Situação | Obrigatório | Ativo ou inativo; ativo por padrão no cadastro |
| Empresa | Obrigatório e automático | Deve ser a única empresa cadastrada na V1 |

### Regras do cadastro

- O cadastro de usuários deve ser restrito ao Administrador.
- O sistema não deve oferecer cadastro público.
- Um usuário deve poder possuir mais de um papel.
- A empresa deve ser atribuída automaticamente, sem seleção pelo Administrador.
- A conta deve ser criada como ativa por padrão.
- Após o cadastro, o sistema deve enviar o convite de definição da primeira senha, conforme o RF01.
- A situação de acesso e a conclusão do primeiro acesso devem ser tratadas separadamente. Uma conta ativa pode estar aguardando a definição da primeira senha.
- A atribuição de papéis deve respeitar as regras de autorização definidas para cada funcionalidade.

### Critérios de aceite do cadastro

- **CA01:** um Administrador autenticado deve conseguir cadastrar um usuário com nome completo, e-mail válido e pelo menos um papel disponível.
- **CA02:** a ausência de qualquer campo obrigatório deve impedir o cadastro e gerar uma orientação de preenchimento.
- **CA03:** o sistema deve impedir o cadastro de outra conta com um e-mail já utilizado, inclusive por uma conta inativa.
- **CA04:** deve ser possível atribuir mais de um papel ao mesmo usuário.
- **CA05:** o sistema deve recusar a atribuição de papéis que não estejam disponíveis na versão.
- **CA06:** a conta deve ser vinculada automaticamente à empresa da V1 e criada como ativa por padrão.
- **CA07:** o usuário cadastrado deve receber o convite de primeiro acesso conforme as regras do RF01.
- **CA08:** usuários sem o papel de Administrador não devem conseguir cadastrar contas, inclusive por solicitações diretas à API.

### Detalhamentos relacionados

- Documentar, na preparação da implantação, o canal privado e o
  procedimento de conferência de identidade para correção do e-mail.
- Detalhar as permissões específicas nos requisitos de cada
  funcionalidade, respeitando as regras de acesso deste RF03.

### Tratamento do e-mail

- O sistema deve remover espaços em branco no início e no fim do e-mail informado.
- O sistema deve converter letras maiúsculas para minúsculas antes de armazenar ou comparar o e-mail.
- A verificação de duplicidade deve utilizar o e-mail normalizado e considerar contas ativas e inativas.
- O login previsto no RF01 deve aplicar a mesma normalização.
- Espaços no interior do endereço não devem ser removidos silenciosamente; o endereço deve ser recusado como inválido.
- A normalização não deve remover pontos nem sufixos iniciados por “+”, pois isso poderia alterar o endereço informado.
- A validação do formato não deve ser apresentada como confirmação de que a caixa de e-mail existe ou pertence ao usuário.

### Critérios de aceite do tratamento do e-mail

- **CA09:** um e-mail informado com espaços no início ou no fim deve ser processado sem esses espaços.
- **CA10:** e-mails que diferem apenas por letras maiúsculas e minúsculas devem ser considerados o mesmo endereço.
- **CA11:** o cadastro deve ser recusado quando o e-mail normalizado já estiver associado a outra conta, ativa ou inativa.
- **CA12:** o usuário deve conseguir realizar login independentemente do uso de maiúsculas e minúsculas no e-mail.
- **CA13:** um endereço com espaços internos deve ser recusado, sem alteração silenciosa para outro endereço.
- **CA14:** pontos e sufixos iniciados por “+” devem ser preservados na normalização.

### Alteração do nome e do e-mail

- O Administrador deve poder alterar o nome completo do usuário.
- O nome completo deve permanecer obrigatório.
- O e-mail deve ser apresentado somente para leitura após o cadastro.
- O bloqueio do e-mail deve ser aplicado também a contas que ainda não concluíram o primeiro acesso.
- O servidor deve recusar tentativas de alteração do e-mail pela operação comum de atualização cadastral.
- A alteração do nome deve preservar a identidade da conta, suas associações e seu histórico.
- Eventuais correções do e-mail devem ocorrer por procedimento controlado, separado da edição cadastral comum.

### Critérios de aceite da alteração do nome e do e-mail

- **CA15:** um Administrador autenticado deve conseguir atualizar o nome completo de um usuário.
- **CA16:** uma tentativa de deixar o nome completo vazio ou composto somente por espaços deve ser recusada, preservando o valor anterior.
- **CA17:** a tela de edição deve apresentar o e-mail somente para leitura, inclusive quando o primeiro acesso estiver pendente.
- **CA18:** tentativas de alterar o e-mail pela operação comum da API devem ser recusadas, preservando o endereço existente.
- **CA19:** a alteração do nome deve manter o identificador da conta, suas associações e seu histórico.


### Responsabilidade pela correção do e-mail

- A correção deve ser solicitada pelo Administrador.
- A execução deve ser restrita ao responsável técnico pela implantação.
- O papel de Administrador do aplicativo, por si só, não deve autorizar a execução do procedimento técnico.
- A correção deve preservar a identidade da conta, suas associações e seu histórico.
- O novo e-mail deve seguir as regras de normalização, validação e unicidade previstas neste requisito.
- O procedimento deve corrigir o endereço da mesma pessoa, sem permitir transferir a conta para outro usuário.

### Critérios de aceite da responsabilidade pela correção

- **CA20:** a correção deve depender de solicitação do Administrador e de execução por responsável técnico autorizado.
- **CA21:** um novo e-mail inválido ou já associado a outra conta, ativa ou inativa, deve ser recusado.
- **CA22:** a correção deve preservar o identificador da conta, suas associações e seu histórico.

### Pontos a detalhar da correção do e-mail

- Informações necessárias à solicitação e à conferência da identidade do usuário.
- Registro e armazenamento do histórico da correção.
- Confirmação do novo endereço e retomada do acesso.

### Efeito da correção do e-mail sobre o acesso

- A correção do e-mail deve invalidar todos os convites de primeiro acesso pendentes da conta.
- A correção deve revogar todas as sessões existentes da conta, em todos os dispositivos.
- As credenciais das sessões revogadas não devem permitir acesso à API nem renovação.
- O procedimento não deve ser considerado concluído com sucesso enquanto a alteração do e-mail e as invalidações necessárias não estiverem efetivadas.
- A correção deve preservar a identidade da conta, os registros de negócio e o histórico existente.
- A revogação de todas as sessões por correção de e-mail não deve alterar o comportamento da opção “Sair”, que continua restrita à sessão atual.

### Critérios de aceite dos efeitos sobre o acesso

- **CA23:** após a correção, convites de primeiro acesso emitidos anteriormente não devem permitir a definição de senha.
- **CA24:** credenciais de qualquer sessão anterior à correção devem ser recusadas nas tentativas de acesso à API.
- **CA25:** tentativas de renovação das sessões anteriores devem ser recusadas.
- **CA26:** a correção não deve excluir despesas, comprovantes ou outros registros associados ao usuário.
- **CA27:** o procedimento não deve informar sucesso se a alteração do e-mail ou alguma das invalidações obrigatórias não tiver sido concluída.

### Retomada do acesso após correção do e-mail

- Após a correção, o sistema deve enviar ao novo endereço um link para confirmação do e-mail e definição de uma nova senha.
- O link deve ser de uso único e ter validade de 24 horas a partir da emissão.
- A conta deve permanecer impedida de realizar login enquanto esse processo estiver pendente.
- A senha anterior não deve permitir autenticação após a correção.
- O usuário deve informar e confirmar uma nova senha, respeitando a política definida no RF01.
- A confirmação do novo endereço deve ocorrer na conclusão bem-sucedida da definição da senha, e não apenas na abertura do link.
- Após a conclusão, o link deve ser invalidado.
- A conclusão não deve iniciar uma sessão automaticamente. O usuário deve retornar ao login e utilizar o novo e-mail e a nova senha.
- A confirmação não deve alterar a situação administrativa da conta. Uma conta inativa deve continuar sem acesso.
- O fluxo deve integrar o procedimento controlado de correção de e-mail, sem disponibilizar recuperação pública de senha na V1.

### Critérios de aceite da retomada do acesso

- **CA28:** após a correção, a mensagem de confirmação deve ser enviada ao novo endereço cadastrado.
- **CA29:** enquanto a confirmação e a definição da nova senha estiverem pendentes, tentativas de login devem ser recusadas.
- **CA30:** um link válido deve permitir concluir o processo quando a senha e sua confirmação coincidirem e atenderem à política do RF01.
- **CA31:** abrir o link sem concluir o processo não deve confirmar o endereço nem consumir o link.
- **CA32:** links expirados ou já utilizados não devem permitir concluir o processo.
- **CA33:** após a conclusão, o usuário deve conseguir entrar com o novo e-mail e a nova senha, desde que a conta esteja ativa.
- **CA34:** a conclusão deve preservar a situação inativa de uma conta desativada e não deve permitir seu acesso.
- **CA35:** o e-mail anterior e a senha anterior não devem permitir autenticação na conta corrigida.

### Reenvio da confirmação após correção do e-mail

- O Administrador deve poder solicitar o reenvio enquanto a confirmação do novo e-mail e a definição da nova senha estiverem pendentes.
- O link deve ser enviado exclusivamente ao endereço corrigido registrado na conta.
- A operação de reenvio não deve permitir informar ou alterar o endereço de destino.
- Cada reenvio deve gerar um novo link de uso único, válido por 24 horas a partir da emissão, e invalidar o anterior.
- O reenvio não deve liberar o acesso, alterar a situação administrativa da conta ou restaurar sessões revogadas.
- Após a conclusão da confirmação e da definição da senha, o reenvio por esse fluxo não deve ser permitido.

### Critérios de aceite do reenvio

- **CA36:** enquanto o processo estiver pendente, o Administrador deve conseguir solicitar o envio de um novo link ao e-mail corrigido da conta.
- **CA37:** após a emissão do novo link, o anterior deve ser recusado, mesmo que seu prazo original ainda não tenha terminado.
- **CA38:** o novo link deve possuir validade de 24 horas contadas a partir de sua própria emissão.
- **CA39:** a solicitação de reenvio não deve permitir substituir o endereço de destino.
- **CA40:** o reenvio deve preservar o bloqueio de acesso pendente de confirmação e não deve reativar contas ou sessões.
- **CA41:** após a conclusão do processo, tentativas de reenvio por esse fluxo devem ser recusadas.
- **CA42:** usuários sem autorização de Administrador não devem conseguir solicitar o reenvio.

### Solicitação e conferência da correção do e-mail

- A solicitação deve ser feita pelo Administrador e conter:
  - Identificador da conta.
  - E-mail atualmente cadastrado.
  - E-mail correto.
  - Motivo da correção.
  - Identificação do solicitante.
- O responsável técnico deve conferir se a solicitação corresponde à correção do endereço da mesma pessoa, sem transferência da conta para outro usuário.
- A conferência deve possuir evidência registrada antes da execução.
- Informações insuficientes ou divergentes devem impedir a execução até seu esclarecimento.
- A confirmação posterior pelo link enviado ao novo e-mail não deve substituir a conferência prévia de identidade.

### Histórico e armazenamento da correção

- A solicitação e a evidência de conferência devem ser armazenadas em chamado ou registro privado, acessível somente aos responsáveis autorizados.
- Cada solicitação deve possuir um identificador.
- O histórico da correção deve conter:
  - Identificador da conta.
  - E-mail anterior.
  - E-mail novo.
  - Motivo da correção.
  - Identificação do Administrador solicitante.
  - Identificação do responsável técnico executor.
  - Data e hora da alteração.
  - Referência à solicitação e à evidência de conferência.
- O histórico não deve armazenar senhas nem links ou tokens de confirmação.
- Dados reais dos usuários e evidências de identidade não devem ser publicados no repositório público do projeto.
- A V1 não deve exigir um sistema próprio de chamados para esse procedimento.

### Critérios de aceite da solicitação e do histórico

- **CA43:** solicitações sem os dados obrigatórios devem ser consideradas incompletas e não devem autorizar a execução.
- **CA44:** a correção deve depender de conferência prévia de identidade, com evidência registrada.
- **CA45:** divergências que impeçam confirmar a identidade do usuário devem bloquear a execução.
- **CA46:** toda correção executada deve possuir histórico com os dados definidos neste requisito.
- **CA47:** a solicitação e suas evidências devem estar disponíveis apenas aos responsáveis autorizados.
- **CA48:** o histórico deve referenciar a solicitação sem registrar senhas, links ou tokens de confirmação.

### Ativação e desativação de usuários

- O Administrador deve poder ativar e desativar contas, respeitando as regras de proteção dos Administradores.
- A desativação deve impedir novos logins e a renovação de credenciais.
- A desativação deve revogar todas as sessões existentes da conta.
- A desativação deve invalidar os links pendentes de primeiro acesso e de confirmação após correção do e-mail.
- A desativação deve preservar a conta, suas associações, despesas e demais registros históricos.
- A reativação deve permitir novamente a autenticação, desde que a conta tenha concluído a definição de senha e não possua confirmação de e-mail pendente.
- A reativação não deve restaurar sessões nem links invalidados.
- Quando houver primeiro acesso ou confirmação de e-mail pendente, será necessário emitir um novo link pelo fluxo correspondente.
- A emissão ou utilização de um link não deve reativar automaticamente uma conta.
- As operações de ativação e desativação devem registrar a conta afetada, o Administrador responsável, a data e hora e a situação anterior e posterior.

### Critérios de aceite da ativação e desativação

- **CA49:** um Administrador autorizado deve conseguir desativar uma conta, respeitando as regras de proteção dos Administradores.
- **CA50:** após a desativação, tentativas de login, acesso à API e renovação de credenciais da conta devem ser recusadas.
- **CA51:** links pendentes emitidos antes da desativação devem ser recusados.
- **CA52:** a desativação deve preservar a identidade da conta e os registros associados.
- **CA53:** após a reativação, uma conta com senha definida e sem confirmação pendente deve poder realizar um novo login.
- **CA54:** a reativação não deve tornar válidas as sessões ou os links anteriormente invalidados.
- **CA55:** uma conta reativada com primeiro acesso ou confirmação pendente deve continuar sem acesso até concluir o fluxo correspondente por um novo link.
- **CA56:** a utilização de um link não deve permitir acesso enquanto a conta estiver inativa.
- **CA57:** a ativação e a desativação devem possuir registro do responsável, momento da operação e situações anterior e posterior.

### Proteção das contas de Administrador

Para aplicação destas regras, considera-se Administrador apto ao acesso uma conta que:

- Possua o papel de Administrador.
- Esteja ativa.
- Tenha concluído a definição de senha.
- Não possua confirmação de e-mail pendente.

Não é necessário que essa conta esteja com uma sessão aberta.

As seguintes proteções devem ser aplicadas:

- O Administrador não deve poder desativar a própria conta, mesmo que existam outros Administradores.
- O sistema deve impedir a desativação do último Administrador apto ao acesso.
- O sistema deve impedir a remoção do papel de Administrador quando a operação deixar a empresa sem outro Administrador apto ao acesso.
- Contas com primeiro acesso ou confirmação de e-mail pendentes não devem ser consideradas substitutas para essas operações.
- As verificações devem ser executadas no servidor com base na situação vigente das contas.
- Operações simultâneas não devem permitir contornar essas proteções.

### Critérios de aceite da proteção dos Administradores

- **CA58:** uma tentativa de desativar a própria conta deve ser recusada, preservando sua situação.
- **CA59:** uma tentativa de desativar o último Administrador apto ao acesso deve ser recusada.
- **CA60:** uma tentativa de remover o papel do último Administrador apto ao acesso deve ser recusada.
- **CA61:** a existência de outro Administrador com primeiro acesso ou confirmação de e-mail pendente não deve liberar uma operação que deixe a empresa sem Administrador apto ao acesso.
- **CA62:** a existência de outro Administrador apto ao acesso deve permitir prosseguir com a operação, desde que as demais regras sejam atendidas.
- **CA63:** duas operações simultâneas não devem conseguir desativar ou remover os papéis dos últimos Administradores aptos ao acesso.
- **CA64:** uma operação bloqueada deve informar que é necessário manter outro Administrador ativo com a configuração de acesso concluída.

### Proteção do Administrador na correção de e-mail

- A correção do e-mail de uma conta com papel de Administrador deve verificar se outro Administrador permanecerá apto ao acesso após a operação.
- Quando a correção deixar a empresa sem Administrador apto ao acesso, o procedimento deve ser bloqueado.
- Antes de prosseguir, outro Administrador deve estar ativo, com senha definida e sem confirmação de e-mail pendente.
- A verificação deve ocorrer no momento da execução, inclusive no procedimento técnico.
- Quando a operação for bloqueada por essa proteção, o e-mail, as credenciais, as sessões e os links da conta devem permanecer inalterados.
- Operações simultâneas de correção de e-mail, desativação ou remoção de papel não devem contornar a proteção.

### Critérios de aceite da proteção na correção de e-mail

- **CA65:** a correção do e-mail do último Administrador apto ao acesso deve ser recusada.
- **CA66:** outro Administrador com primeiro acesso ou confirmação de e-mail pendente não deve ser considerado substituto apto.
- **CA67:** quando outro Administrador permanecer apto ao acesso, a correção deve poder prosseguir, respeitando as demais regras do procedimento.
- **CA68:** o bloqueio deve ocorrer antes de alterar o e-mail ou invalidar credenciais, sessões e links.
- **CA69:** operações simultâneas não devem deixar a empresa sem Administrador apto ao acesso.

### Papéis e permissões

- A V1 deve disponibilizar os papéis Colaborador, Gestor e Administrador, com permissões predefinidas.
- O Administrador deve poder atribuir um ou mais papéis disponíveis a cada usuário.
- Cada usuário deve manter pelo menos um papel.
- A V1 não deve permitir criar papéis personalizados nem conceder ou retirar permissões individualmente.
- Quando houver múltiplos papéis, o usuário deve possuir as permissões correspondentes à sua combinação, respeitando as restrições de cada funcionalidade.
- O papel Colaborador deve limitar suas operações às próprias despesas, conforme as regras dos requisitos relacionados.
- O papel Gestor deve limitar as operações de gestão e aprovação à equipe sob sua responsabilidade.
- O papel Administrador não deve, por si só, conceder permissão para aprovar despesas.
- A autorização deve ser verificada no servidor, considerando tanto a ação solicitada quanto os registros envolvidos.
- Alterações de papéis devem respeitar as regras de proteção dos Administradores.

### Critérios de aceite dos papéis e permissões

- **CA70:** o Administrador deve conseguir atribuir um ou mais dos papéis disponíveis a um usuário.
- **CA71:** uma tentativa de deixar o usuário sem nenhum papel deve ser recusada.
- **CA72:** tentativas de criar papéis personalizados ou atribuir permissões individuais não devem ser aceitas na V1.
- **CA73:** um usuário com múltiplos papéis deve conseguir executar as ações correspondentes, respeitando os limites de acesso aos registros.
- **CA74:** um Colaborador sem outra autorização aplicável não deve conseguir acessar despesas de outro usuário.
- **CA75:** um Gestor não deve conseguir aprovar despesas de colaboradores fora da equipe sob sua responsabilidade.
- **CA76:** possuir somente o papel Administrador não deve permitir aprovar despesas.
- **CA77:** solicitações diretas à API devem respeitar as mesmas regras de autorização aplicadas na interface.

### Vínculo entre Colaborador e Gestor

- Cada Colaborador deve possuir, no máximo, um Gestor responsável.
- O vínculo pode permanecer pendente no cadastro e durante a elaboração de rascunhos.
- O envio de despesas para aprovação exige um Gestor responsável ativo e apto ao acesso.
- Um Gestor deve poder ser responsável por vários colaboradores.
- A atribuição e a alteração do Gestor responsável devem ser restritas ao Administrador.
- A conta indicada como responsável deve possuir o papel Gestor.
- O vínculo deve identificar a equipe sob responsabilidade do Gestor para aplicação das regras de acesso e aprovação.
- Pertencer ao mesmo departamento, por si só, não deve conceder responsabilidade sobre um Colaborador.
- Alterar o Gestor responsável não deve modificar os registros históricos de decisões já realizadas.

### Critérios de aceite do vínculo

- **CA78:** o Administrador deve conseguir atribuir um Gestor responsável a um Colaborador.
- **CA79:** o sistema não deve permitir mais de um Gestor responsável simultaneamente para o mesmo Colaborador.
- **CA80:** o sistema deve permitir vincular vários colaboradores ao mesmo Gestor.
- **CA81:** uma conta sem o papel Gestor não deve ser aceita como responsável.
- **CA82:** usuários sem autorização de Administrador não devem conseguir alterar o vínculo.
- **CA83:** a alteração do responsável deve preservar a identificação de quem realizou aprovações e rejeições anteriores.

### Obrigatoriedade do Gestor no envio para aprovação

Considera-se Gestor apto ao acesso uma conta que possua o papel Gestor, esteja ativa, tenha concluído a definição de senha e não possua confirmação de e-mail pendente. Não é necessário que esteja com uma sessão aberta.

- A ausência de Gestor responsável não deve impedir o cadastro do Colaborador nem a criação e edição de rascunhos.
- Antes de permitir o envio de uma despesa para aprovação, o servidor deve verificar se há um Gestor responsável ativo e apto ao acesso.
- Quando essa condição não for atendida, o envio deve ser bloqueado, preservando o rascunho e seus comprovantes.
- O aplicativo deve informar que é necessário solicitar ao Administrador a regularização do Gestor responsável.
- A verificação deve utilizar a situação vigente do vínculo e da conta do Gestor.

### Critérios de aceite da obrigatoriedade do Gestor

- **CA88:** o Administrador deve conseguir cadastrar um Colaborador sem Gestor responsável.
- **CA89:** um Colaborador sem Gestor responsável deve conseguir criar e editar rascunhos, respeitando as demais regras.
- **CA90:** o envio para aprovação deve ser recusado quando não houver Gestor responsável ou quando ele não estiver ativo e apto ao acesso.
- **CA91:** o bloqueio deve preservar o rascunho e seus comprovantes e orientar o Colaborador a procurar o Administrador.
- **CA92:** após a regularização do vínculo, uma nova tentativa deve poder prosseguir, desde que as demais condições de envio sejam atendidas.

### Requisito relacionado

O RF11 — Enviar despesa deve aplicar a verificação do Gestor responsável definida nesta seção.


### Troca de Gestor e aprovações pendentes

- A alteração do Gestor responsável deve ser realizada pelo Administrador.
- Quando houver aprovações pendentes, o novo Gestor deve estar ativo e apto ao acesso para receber essas solicitações.
- A troca deve encaminhar ao novo Gestor as aprovações pendentes do Colaborador.
- O encaminhamento não deve aprovar, rejeitar ou exigir novo envio das despesas pelo Colaborador.
- O Gestor anterior deve deixar de poder decidir sobre as solicitações transferidas.
- Decisões concluídas antes da troca devem permanecer inalteradas, preservando responsável, data, decisão e comentário.
- A mudança deve registrar o Colaborador, os Gestores anterior e novo, o Administrador executor, a data e hora e as solicitações transferidas.
- O servidor deve verificar o responsável vigente antes de aceitar uma decisão de aprovação ou rejeição.

### Critérios de aceite da troca de Gestor

- **CA93:** quando o Administrador substituir o Gestor por outro ativo e apto ao acesso, as aprovações pendentes devem ficar disponíveis para decisão do novo responsável.
- **CA94:** solicitações transferidas devem continuar pendentes, sem exigir reenvio pelo Colaborador.
- **CA95:** tentativas do Gestor anterior de decidir sobre uma solicitação já transferida devem ser recusadas, inclusive a partir de uma tela desatualizada.
- **CA96:** aprovações e rejeições concluídas antes da troca devem preservar seus dados históricos.
- **CA97:** a transferência deve possuir registro dos envolvidos, do momento da alteração e das solicitações afetadas.
- **CA98:** quando houver aprovações pendentes, a troca para um Gestor que não esteja ativo e apto ao acesso deve ser recusada.
- **CA99:** uma decisão concluída antes da efetivação da troca não deve ser transferida como pendente; uma decisão posterior deve respeitar o novo responsável.

### Requisito relacionado

O RF12 — Aprovação simples deve respeitar a responsabilidade vigente e as transferências definidas nesta seção.


### Desativação de Gestor sem substituto

- A existência de colaboradores vinculados ou de aprovações pendentes não deve impedir a desativação de um Gestor.
- A desativação deve respeitar as demais proteções de contas, inclusive quando o usuário também possuir o papel Administrador.
- Os vínculos dos colaboradores com o Gestor desativado devem ser preservados até sua substituição.
- As aprovações existentes devem permanecer pendentes, sem aprovação, rejeição ou cancelamento automático.
- O Gestor desativado não deve conseguir acessar ou decidir sobre as solicitações.
- Novos envios dos colaboradores vinculados devem ser bloqueados enquanto não houver Gestor responsável ativo e apto ao acesso.
- O Administrador deve conseguir identificar os colaboradores vinculados ao Gestor inativo e as aprovações que precisam de regularização.
- Ao atribuir um substituto ativo e apto ao acesso, o sistema deve aplicar as regras de transferência das aprovações pendentes.
- A desativação e a posterior substituição devem preservar o histórico das despesas e das decisões já concluídas.

### Critérios de aceite da desativação sem substituto

- **CA100:** o Administrador deve conseguir desativar um Gestor com colaboradores vinculados e aprovações pendentes, desde que as demais proteções sejam atendidas.
- **CA101:** após a desativação, as solicitações existentes devem permanecer pendentes e conservar seus dados e comprovantes.
- **CA102:** o Gestor desativado não deve conseguir aprovar ou rejeitar solicitações.
- **CA103:** novos envios dos colaboradores vinculados devem ser recusados, preservando seus rascunhos e orientando a regularização do responsável.
- **CA104:** o Administrador deve conseguir identificar os vínculos e as aprovações afetados pela desativação.
- **CA105:** após a atribuição de um substituto apto, as solicitações pendentes devem ser transferidas sem exigir reenvio pelo Colaborador.

### Vínculo do Colaborador com departamento

- O Colaborador deve poder estar vinculado a, no máximo, um departamento por vez.
- O vínculo com departamento deve ser opcional.
- A atribuição, alteração e remoção do vínculo devem ser restritas ao Administrador.
- O departamento selecionado deve pertencer à empresa cadastrada.
- A ausência de departamento não deve impedir o cadastro do Colaborador, a elaboração de rascunhos ou o envio de despesas, desde que as demais regras sejam atendidas.
- O vínculo com departamento não deve conceder permissões de acesso ou aprovação.
- A alteração ou remoção do departamento não deve modificar automaticamente o Gestor responsável nem transferir aprovações pendentes.
- O cadastro e a manutenção dos departamentos devem seguir o RF04 — Departamentos.

### Critérios de aceite do vínculo com departamento

- **CA106:** o Administrador deve conseguir vincular um Colaborador a um departamento cadastrado da empresa.
- **CA107:** uma tentativa de manter mais de um departamento simultaneamente para o mesmo Colaborador deve ser recusada.
- **CA108:** o Administrador deve conseguir cadastrar e manter um Colaborador sem departamento.
- **CA109:** a ausência de departamento não deve, por si só, impedir o envio de uma despesa para aprovação.
- **CA110:** usuários sem autorização de Administrador não devem conseguir alterar o vínculo.
- **CA111:** a atribuição, alteração ou remoção do departamento deve preservar o Gestor responsável e a distribuição das aprovações pendentes.
- **CA112:** pertencer ao mesmo departamento não deve conceder acesso às despesas de outros colaboradores.

### Associação entre conta e cadastro de colaborador

- Toda conta que possuir o papel Colaborador deve ter um cadastro de colaborador associado.
- O cadastro de colaborador deve ser criado quando o papel Colaborador for atribuído à conta pela primeira vez.
- Cada conta deve possuir, no máximo, um cadastro de colaborador, e cada cadastro deve estar associado a uma única conta.
- A atribuição do papel e a criação da associação devem ser concluídas de forma consistente, sem deixar uma conta com papel Colaborador e sem o cadastro correspondente.
- O cadastro de colaborador deve pertencer à mesma empresa da conta.
- Os vínculos com departamento e Gestor responsável devem ser associados ao cadastro de colaborador.
- Nome e e-mail devem ser consultados a partir da conta associada, sem cadastro independente desses dados no colaborador.
- Contas que nunca receberam o papel Colaborador não devem exigir um cadastro de colaborador.
- A V1 não deve exigir matrícula, CPF ou dados trabalhistas para essa associação.

### Critérios de aceite da associação

- **CA113:** ao atribuir o papel Colaborador pela primeira vez, o sistema deve criar e associar o cadastro de colaborador.
- **CA114:** uma falha na criação da associação não deve deixar a conta com o papel Colaborador sem o cadastro correspondente.
- **CA115:** o sistema deve impedir a associação de mais de um cadastro de colaborador à mesma conta ou do mesmo cadastro a contas diferentes.
- **CA116:** a conta e o cadastro de colaborador devem pertencer à mesma empresa.
- **CA117:** uma conta exclusivamente administrativa deve poder existir sem cadastro de colaborador.
- **CA118:** o cadastro de colaborador deve utilizar o nome e o e-mail da conta associada, sem exigir preenchimento duplicado.
- **CA119:** a associação não deve exigir matrícula, CPF ou dados trabalhistas na V1.

### Remoção e reatribuição do papel Colaborador

- A remoção do papel Colaborador deve retirar as permissões concedidas por esse papel.
- A operação deve preservar o cadastro de colaborador, sua associação com a conta e seus registros históricos.
- Despesas, comprovantes, reembolsos e decisões anteriores não devem ser excluídos pela remoção do papel.
- Os demais papéis da conta devem ser preservados, com suas respectivas permissões e restrições.
- A remoção deve respeitar a regra de que toda conta precisa manter pelo menos um papel.
- A reatribuição do papel Colaborador deve reutilizar o cadastro existente, sem criar outro.
- A reatribuição não deve reativar uma conta inativa nem dispensar as verificações de Gestor responsável e demais condições necessárias para enviar despesas.

### Critérios de aceite da remoção e reatribuição

- **CA120:** após a remoção, o usuário não deve conseguir executar ações cuja autorização dependia exclusivamente do papel Colaborador.
- **CA121:** a remoção deve preservar o cadastro de colaborador, sua associação com a conta e os registros existentes.
- **CA122:** a operação deve preservar os demais papéis atribuídos à conta.
- **CA123:** uma tentativa de remover o papel Colaborador quando ele for o único papel da conta deve ser recusada.
- **CA124:** ao reatribuir o papel, o sistema deve reutilizar o mesmo cadastro de colaborador.
- **CA125:** a reatribuição a uma conta inativa não deve liberar seu acesso.
- **CA126:** novos envios após a reatribuição devem continuar sujeitos à verificação do Gestor responsável e às demais regras aplicáveis.

### Ponto a detalhar

- Tratamento das despesas em rascunho ou em processamento quando o papel Colaborador for removido.

### Despesas existentes após a remoção do papel Colaborador

- A remoção do papel Colaborador deve preservar as despesas em rascunho e seus comprovantes.
- Enquanto estiver sem esse papel, o usuário não deve poder editar ou enviar seus rascunhos utilizando permissões anteriormente concedidas pelo papel Colaborador.
- Despesas já enviadas devem continuar no fluxo de aprovação e reembolso, respeitando as demais regras do sistema.
- A remoção do papel não deve aprovar, rejeitar, cancelar ou marcar despesas como pagas.
- Gestores e demais responsáveis autorizados devem poder continuar o tratamento das solicitações existentes.
- Após a reatribuição do papel, o usuário deve poder retomar seus rascunhos, desde que sua conta esteja ativa e as demais condições de acesso sejam atendidas.
- O envio de um rascunho retomado deve utilizar as regras e os vínculos vigentes no momento da solicitação.

### Critérios de aceite das despesas existentes

- **CA127:** a remoção do papel deve preservar os rascunhos e os comprovantes associados.
- **CA128:** enquanto o papel estiver removido, tentativas de editar ou enviar rascunhos com base nas permissões desse papel devem ser recusadas pelo servidor.
- **CA129:** despesas já enviadas devem manter seu estado e continuar disponíveis aos responsáveis autorizados pelo processamento.
- **CA130:** a remoção do papel não deve produzir uma decisão de aprovação, rejeição, cancelamento ou pagamento.
- **CA131:** após a reatribuição do papel, os rascunhos anteriores devem poder ser retomados, respeitando as condições de acesso.
- **CA132:** o envio de um rascunho retomado deve validar o Gestor responsável e as demais condições vigentes, sem reutilizar autorizações antigas.

### Efeito da alteração de papéis sobre as sessões

- A inclusão ou remoção de qualquer papel deve revogar todas as sessões existentes da conta afetada.
- As credenciais dessas sessões devem deixar de permitir acesso à API e renovação.
- O usuário deve realizar nova autenticação para utilizar as permissões resultantes da alteração.
- A nova autenticação deve respeitar a situação da conta e eventuais pendências de primeiro acesso ou confirmação de e-mail.
- Salvar os mesmos papéis, sem alteração efetiva, não deve provocar revogação de sessões.
- A alteração de papéis e a revogação das sessões devem ser efetivadas de forma consistente. A operação não deve informar sucesso se alguma dessas etapas falhar.
- Ao identificar a revogação, o aplicativo deve limpar as credenciais e os dados sensíveis temporários da sessão e solicitar novo login.
- A operação deve registrar a conta afetada, o Administrador responsável, a data e hora e os papéis anteriores e posteriores.
- Todas as alterações devem respeitar as proteções do último Administrador apto ao acesso e a exigência de pelo menos um papel por conta.

### Critérios de aceite da alteração de papéis

- **CA133:** incluir ou remover um papel deve revogar todas as sessões existentes da conta afetada.
- **CA134:** credenciais anteriores à alteração devem ser recusadas nas tentativas de acesso à API e de renovação.
- **CA135:** após nova autenticação válida, o usuário deve possuir as permissões correspondentes aos papéis atuais, respeitando os limites de acesso aos registros.
- **CA136:** salvar uma atribuição de papéis sem mudanças não deve encerrar sessões.
- **CA137:** falhas na alteração ou na revogação devem impedir a confirmação de sucesso e não devem deixar sessões operando com permissões retiradas.
- **CA138:** a alteração deve possuir histórico com responsável, momento da operação e papéis anteriores e posteriores.
- **CA139:** alterar papéis não deve reativar contas nem dispensar pendências de acesso.

### Remoção do papel Gestor

- A remoção do papel Gestor deve ser bloqueada enquanto houver colaboradores sob sua responsabilidade ou aprovações pendentes atribuídas à conta.
- Antes da remoção, o Administrador deve transferir essas responsabilidades para outro Gestor ativo e apto ao acesso.
- A transferência deve respeitar as regras de vínculo, encaminhamento de aprovações pendentes e impedimento de autoaprovação.
- Decisões já concluídas devem permanecer associadas ao responsável original, sem impedir a remoção do papel.
- Após a regularização das responsabilidades, a remoção deve respeitar a exigência de pelo menos um papel por conta.
- A remoção efetivada deve revogar todas as sessões da conta, conforme as regras de alteração de papéis.
- Quando for necessário bloquear imediatamente o acesso, a conta poderá ser desativada conforme as regras de desativação, sem depender da transferência prévia.

### Critérios de aceite da remoção do papel Gestor

- **CA140:** a tentativa de remover o papel Gestor de uma conta com colaboradores vinculados ou aprovações pendentes deve ser recusada.
- **CA141:** o bloqueio deve informar quais responsabilidades precisam ser regularizadas antes da remoção.
- **CA142:** após a transferência dos colaboradores e das aprovações pendentes, a remoção deve ser permitida, desde que as demais proteções sejam atendidas.
- **CA143:** a existência de decisões históricas concluídas não deve impedir a remoção nem alterar sua autoria.
- **CA144:** se Gestor for o único papel da conta, sua remoção deve ser recusada até que outro papel seja atribuído.
- **CA145:** a remoção concluída deve revogar as sessões da conta.
- **CA146:** uma tentativa de remoção bloqueada não deve alterar os papéis nem revogar sessões.
- **CA147:** a verificação e a remoção devem impedir que operações simultâneas deixem colaboradores ou aprovações pendentes atribuídos a uma conta sem o papel Gestor.

## RF04 — Departamentos

### Objetivo

Permitir que o Administrador mantenha os departamentos da empresa para organizar os colaboradores.

### Ator

Administrador.

### Etapa

A V1 deve disponibilizar um cadastro simples, sem hierarquia entre departamentos.

### Dados do departamento

| Campo | Obrigatoriedade | Regra |
|---|---|---|
| Nome | Obrigatório | Deve ser único na empresa |
| Descrição | Opcional | Explica a finalidade da área |
| Situação | Obrigatório | Ativo ou inativo; ativo por padrão |
| Empresa | Obrigatório e automático | Deve ser a única empresa cadastrada na V1 |

### Regras do cadastro

- O cadastro deve ser restrito ao Administrador.
- O nome deve ser preenchido e não pode ser composto somente por espaços.
- A ausência de descrição não deve impedir o cadastro.
- O departamento deve ser criado como ativo por padrão.
- A empresa deve ser atribuída automaticamente.
- A V1 não deve permitir definir departamentos superiores ou subdepartamentos.
- O departamento deve servir à organização dos colaboradores, sem conceder permissões ou definir automaticamente o Gestor responsável.
- A associação dos colaboradores deve seguir as regras do RF03 — Usuários.

### Critérios de aceite do cadastro

- **CA01:** um Administrador autenticado deve conseguir cadastrar um departamento com nome válido e único.
- **CA02:** a ausência de nome ou o preenchimento somente com espaços deve impedir o cadastro.
- **CA03:** o cadastro sem descrição deve ser permitido.
- **CA04:** o departamento deve ser criado como ativo e vinculado automaticamente à empresa.
- **CA05:** o sistema deve recusar o cadastro de um nome já utilizado na empresa, conforme a regra de comparação de nomes.
- **CA06:** usuários sem autorização de Administrador não devem conseguir cadastrar departamentos, inclusive pela API.
- **CA07:** a V1 não deve disponibilizar associação hierárquica entre departamentos.

### Normalização e unicidade do nome

- O sistema deve remover espaços em branco no início e no fim do nome.
- Sequências de espaços entre palavras devem ser reduzidas a um único espaço.
- O nome normalizado deve preservar as letras maiúsculas e minúsculas informadas para exibição.
- A comparação de nomes deve desconsiderar diferenças entre letras maiúsculas e minúsculas.
- A verificação de duplicidade deve considerar todos os departamentos da empresa, ativos e inativos.
- As regras devem ser aplicadas tanto no cadastro quanto na alteração do nome.
- Na edição, o próprio departamento deve ser desconsiderado na busca por outro registro com o mesmo nome.
- O servidor deve garantir a unicidade, inclusive diante de solicitações simultâneas.

### Critérios de aceite da normalização e unicidade

- **CA08:** espaços no início e no fim devem ser removidos antes da validação e do armazenamento.
- **CA09:** espaços consecutivos entre palavras devem ser reduzidos a um único espaço.
- **CA10:** nomes que diferem apenas por maiúsculas, minúsculas ou espaços normalizados devem ser considerados duplicados.
- **CA11:** um nome utilizado por departamento inativo deve impedir a criação de outro departamento com o mesmo nome.
- **CA12:** a edição deve permitir manter o próprio nome ou ajustar apenas sua capitalização, desde que não haja conflito com outro departamento.
- **CA13:** uma tentativa de renomear um departamento para o nome de outro deve ser recusada, preservando o valor anterior.
- **CA14:** duas solicitações simultâneas com nomes equivalentes não devem gerar departamentos duplicados.

### Alteração dos dados do departamento

- O Administrador deve poder alterar o nome e a descrição do departamento.
- O nome deve permanecer obrigatório e único na empresa, respeitando as regras de normalização e comparação.
- A descrição deve permanecer opcional, podendo ser removida.
- A alteração deve preservar o identificador do departamento e os vínculos dos colaboradores.
- A edição do nome ou da descrição não deve modificar a situação ativa ou inativa.
- A alteração não deve modificar os Gestores responsáveis, as permissões dos usuários ou a distribuição das aprovações pendentes.

### Critérios de aceite da alteração

- **CA15:** um Administrador autenticado deve conseguir alterar o nome e a descrição do departamento.
- **CA16:** uma tentativa de salvar um nome vazio ou duplicado deve ser recusada, preservando os dados anteriores.
- **CA17:** o Administrador deve conseguir remover a descrição.
- **CA18:** após a alteração, o departamento deve manter seu identificador e os mesmos colaboradores vinculados.
- **CA19:** editar nome ou descrição não deve ativar nem desativar o departamento.
- **CA20:** a alteração deve preservar os vínculos com Gestores, as permissões e as aprovações pendentes.
- **CA21:** tentativas de edição por usuários sem autorização de Administrador devem ser recusadas, inclusive pela API.

### Desativação de departamentos

- O Administrador deve poder desativar um departamento, mesmo que existam colaboradores vinculados.
- A desativação deve preservar o cadastro e os vínculos existentes.
- Departamentos inativos não devem aceitar novas associações de colaboradores.
- Os vínculos existentes devem continuar visíveis ao Administrador, com indicação de que o departamento está inativo.
- O Administrador deve poder transferir os colaboradores para outro departamento ativo ou remover seu vínculo com departamento.
- A desativação não deve bloquear o acesso dos colaboradores nem impedir, por si só, o registro ou envio de despesas.
- A desativação e as transferências de departamento não devem modificar o Gestor responsável, as permissões ou a distribuição das aprovações pendentes.
- A restrição de novas associações deve ser verificada no servidor.

### Critérios de aceite da desativação

- **CA22:** um Administrador deve conseguir desativar um departamento com ou sem colaboradores vinculados.
- **CA23:** após a desativação, os vínculos existentes devem permanecer registrados e disponíveis para consulta administrativa.
- **CA24:** uma tentativa de associar um Colaborador a um departamento inativo deve ser recusada, inclusive pela API.
- **CA25:** o Administrador deve conseguir transferir um Colaborador de um departamento inativo para outro ativo ou remover o vínculo.
- **CA26:** a desativação do departamento não deve desativar as contas dos colaboradores nem impedir suas operações por esse motivo.
- **CA27:** a desativação deve preservar os Gestores responsáveis, as permissões e as aprovações pendentes.
- **CA28:** ao consultar um vínculo existente com departamento inativo, o Administrador deve conseguir identificar essa situação.
- **CA29:** salvar outros dados de um Colaborador sem alterar seu vínculo existente não deve ser impedido apenas porque o departamento está inativo.

### Reativação de departamentos

- O Administrador deve poder reativar um departamento inativo.
- A reativação deve preservar o identificador, o nome, a descrição e os vínculos existentes no momento da operação.
- Após a reativação, o departamento deve voltar a aceitar associações de colaboradores.
- Colaboradores transferidos ou desvinculados durante a inatividade não devem retornar automaticamente.
- A reativação não deve alterar a situação das contas, os Gestores responsáveis, as permissões ou as aprovações pendentes.
- A operação deve respeitar as regras de unicidade do nome já definidas.

### Critérios de aceite da reativação

- **CA30:** um Administrador autenticado deve conseguir reativar um departamento inativo.
- **CA31:** a reativação deve manter o mesmo identificador e os dados cadastrais vigentes.
- **CA32:** os colaboradores ainda vinculados devem permanecer associados após a reativação.
- **CA33:** colaboradores transferidos ou desvinculados não devem ser reassociados automaticamente.
- **CA34:** após a reativação, novas associações devem ser permitidas, respeitando as demais regras de vínculo.
- **CA35:** a reativação não deve modificar contas, Gestores responsáveis, permissões ou aprovações pendentes.
- **CA36:** tentativas de reativação por usuários sem autorização de Administrador devem ser recusadas, inclusive pela API.

