1. App.Js
    - *Adição de novas telas:* Novas telas foram adicionadas ao projeto: RegisterScreen, UserManagementScreen, EditUserScreen e RegisterUserScreen

    - *Alterações no Navegador de Perfil:* navegação para o perfil agora é feita através de um Stack Navigator dedicado (ProfileStackNavigator), contém telas relacionadas ao gerenciamento de usuários: ProfileMain, UserManagement, EditUser e RegisterUser; tendo múltiplas telas dentro do mesmo fluxo.

    - *Alterações na Função de Autenticação:*
        - A função signIn agora recebe um objeto de dados, contendo tanto o token quanto as informações do usuário. Esses dados são armazenados no AsyncStorage, sendo o token e os dados do usuário salvos separadamente.
        - A função signOut foi atualizada para remover tanto o userToken quanto o userData do AsyncStorage.
        - A função getUserData foi adicionada para permitir a recuperação dos dados do usuário armazenados no AsyncStorage.
    - *Alterações no Contexto de Autenticação:*  O AuthContext agora também fornece a função getUserData, que permite obter os dados do usuário armazenados no AsyncStorage.
    - *Mudanças na Navegação de "Perfil":* A tela de perfil agora é acessada dentro do ProfileStackNavigator, permitindo um fluxo de navegação mais complexo com várias telas.

2. package-lock.json

    - @react-navigation/bottom-tabs substitui essa dependência pela @react-native-picker/picker, que serve para criar pickers (menus suspensos) no aplicativo.
    - Agora, ao instalar as dependências com npm install ou yarn install, o @react-native-picker/picker será instalado e gerenciado corretamente, com a versão 2.11.1, e o ambiente do seu projeto estará ciente dessa versão e suas dependências; permite que possa usar componentes de "Picker" (menus suspensos de seleção) dentro do seu app, substituindo outras soluções ou apenas adicionando novas funcionalidades;
    - @react-native/babel-plugin-codegen, @react-native/babel-preset, @react-native/codegen e dependências apagadas

3. package.json
    - *Pacote adicionado:* @react-native-picker/picker na versão ^2.11.1.
        Essa modificação pode indicar a introdução de uma funcionalidade de seleção na interface do seu aplicativo, como campos de seleção de opções para o usuário.

4. AuthContext.js
    - *Autenticação Mock vs. Serviço Real:* Usa um serviço de autenticação real (authService) para realizar o login. A função signIn recebe uma resposta (response) que já contém os dados do usuário, o que implica que o login está sendo feito por meio de um serviço externo, como uma API de autenticação.
    - *Armazenamento de Dados:* Além de armazenar o user, também armazena um token de autenticação (@LogiTrack:token), utilizando AsyncStorage.multiRemove para limpar ambos quando o usuário faz logout.
    - *Estrutura do Código:* a função signIn depende de um objeto de resposta de um serviço, enquanto no primeiro código, não há essa dependência externa.

5. DashboardScreen.js
    - *Imports:* Foi adicionada a importação de Alert do react-native, que é utilizado para exibir alertas de erro quando ocorre algum problema na carga de dados.
    - *Função Alert:* usada dentro do bloco de catch para exibir um alerta quando há falha na requisição de dados
LoginScreen
    - O código começa com a importação de React, useState, e useContext, mas também inclui useEffect.

6. ProfileScreen
    - *Estrutura dos Dados do Usuário:* O estado userData é inicializado com { nomeCompleto: 'Usuário', username: '', email: '', telefone: '', tipoUsuario: 'USUARIO' }.
    - *Recuperação de Dados do Usuário:* Os dados do usuário são recuperados de AsyncStorage com a chave userData.
    - *Logout*: O logout agora envolve uma confirmação do usuário, exibindo um Alert antes de realizar o logout. O signOut() é chamado dentro da ação do alerta.
    - *Navegação*: Usa useNavigation para navegar para a tela de "UserManagement" (Gerenciamento de Usuários), caso o usuário seja um administrador (isAdmin).
    - *Exibição Condicional para Administradores*: A seção "Administração" é exibida apenas se o usuário for um administrador (isAdmin), o que é feito verificando o campo tipoUsuario.
    - *Uso do ScrollView*: O conteúdo está envolto em um ScrollView, o que permite que o conteúdo seja rolado caso seja maior que a altura da tela.
    - *Estilos e Layout*: Há uma maior padronização de margens e padding (por exemplo, paddingHorizontal: 16 para a maioria das seções), além de ajustes específicos para o layout, como marginTop: 16 no perfil.

7. authService.js
    - *Validação de Usuário:* Validação real via API, com credenciais verificadas no backend.
    - *Armazenamento do Token:*  O token JWT é armazenado no AsyncStorage para persistência.
    - *Logout:*  Remoção real do token e dados do usuário do AsyncStorage.
    - *Requisição HTTP:* Comunicação com uma API backend real via fetch.
    - *Segurança:* Autenticação baseada em JWT, com segurança implementada no backend.

8. robotService.js
    - *Autenticação com Token:* Adiciona a autenticação via token JWT. Antes de fazer a requisição, busca o token no AsyncStorage e o adiciona no cabeçalho da requisição. Se o token não estiver presente, a requisição é feita sem ele.
    - *Cabeçalhos da Requisição:* O cabeçalho da requisição é configurado dinamicamente, incluindo o token no caso de o usuário estar autenticado. Se o token for encontrado no AsyncStorage, ele será enviado no cabeçalho como um Authorization: Bearer.
    - *Tratamento de Erros:* A verificação de erro é mais detalhada. Se a resposta for 401 ou 403 (não autorizado), uma mensagem específica é retornada, indicando que o usuário não está autenticado e deve fazer login novamente. Além disso, o erro é tratado de forma mais específica, verificando se o erro está relacionado à autenticação.
    - *Método da Requisição:* Adiciona o método GET com um cabeçalho condicional para autenticação, dependendo de um token no AsyncStorage.
    - *Mensagens de Erro Específicas:*  Exibe mensagens específicas relacionadas à falha de autenticação (se o usuário não estiver autenticado) e também mantém a mensagem genérica para outros tipos de falha de conexão.
