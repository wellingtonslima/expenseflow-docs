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

### Pontos a detalhar

- Procedimento controlado de correção do CNPJ

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

