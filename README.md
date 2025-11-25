Case FullStack
Contexto
O SeniorLabs é um time de P&I que busca trazer inovações das novas tecnologias e tendências do Brasil e do mundo para estudar e criar soluções aplicadas aos produtos e negócios da Senior. Uma das ferramentas desenvolvidas dentro do SeniorLabs é o Content Generator, que utiliza LLM para, como o nome sugere, gerar conteúdos. Com ela, podemos gerar textos, resumir informações ou até mesmo usá-la como um assistente inteligênte. Com base nisso, seu desafio é desenvolver uma aplicação FullStack que inclua comunicação com o Content Generator.

Funcionalidade principal: Cadastrar resumos de aula.
Funcionalidade especial: Criar resumos inteligentes a partir do texto extraído de arquivos enviados pelo usuário ou por um tópico informado.
O foco é demonstrar boas decisões de arquitetura, UX simples, código limpo e bom uso de testes automatizados.

Entregáveis
Repositório (GitHub/GitLab) com:
Frontend e Backend (monorepo ou pastas separadas).
README com setup e instruções de execução.
.env.example (sem secrets).
Teste(s) automatizado(s) mínimos (unidade e/ou integração).
Requisitos Funcionais
1) Espaço de estudo
CRUD de disciplinas, aulas e resumos (por exemplo: Matemática > Aula 03 – Funções > Resumo).
(Os campos ficam a seu critério, mas o resumo precisa ter um esquema de tags, para palavras chave do resumo).

Uma disciplina pode ter várias aulas
Uma aula pode ter um resumo
2) Upload de arquivos e extração de texto
Ao adicionar resumo, deve ter a opção de upload de arquivos, como: PDF, DOCX e TXT.
A extração das informações do texto bruto deve ser feita no backend.
Exibir pré-visualização do texto extraído antes de resumir.
Nessa tela, deve ter uma opção de utilizar o assistente de IA, que poderá resumir o texto enviado ou criar um resumo com base no tópico escolhido.
4) Assistente Inteligente (IA)
Utilizar a API Externa do Content Generator da Senior para resumir textos — O Content Generator funciona como outras LLMs, logo, você precisa especificar para ela o que deseja fazer.

Resumo inteligente
Com o texto coletado na etapa anterior, deve ter uma opção de gerar um resumo com este texto (Ex: Gere um resumo sobre: texto_a_ser_resumido).
Salvar o resumo como um Resumo de Aula vinculado à aula escolhida.
b. Geração de resumos

Além da opção de gerar um resumo com base em um texto previamente enviado pelo usuário, também deve ter a opção do usuário receber um resumo com base em um tema (Ex: Gere um resumo sobre: tema_a_ser_resumido)
Salvar o resumo como um Resumo de Aula vinculado à aula escolhida.
5) Busca e organização
Busca por texto completo (em títulos e conteúdo dos resumos).
Filtrar por disciplina, aula e tags.
Ordenação por data, relevância e tamanho.
Requisitos Não-Funcionais
Frontend: UI responsiva, acessível (labels, contraste, navegação por teclado). A Stack é da sua escolha, mas será um diferencial usar Angular.
Backend: A Stack é da sua escolha, mas será um diferencial usar Spring Boot.
Banco: O banco é da sua escolha, mas será um diferencial usar PostgreSQL.
Docs: Endpoints documentados (Swagger/OpenAPI ou coleção Rest).
Observabilidade: tratamento de erros e timeouts.
Segurança:
Nunca expor a API Key no frontend; chamadas de IA sempre via backend.
(Opcional) Rate limiting básico no endpoint de IA.
(Opcional) Performance: resposta de geração ≤ 20 s para arquivos de até 100 páginas (pode ser assíncrono com job + polling).

Internacionalização: Interface em pt-BR; resumos em pt-BR por padrão.
Fluxos Principais
Fluxo A — Cadastrar resumo manual
Usuário cria Disciplina e Aula.
Abre “Novo Resumo”, preenche campos, salva.
Item aparece na lista, com busca e filtros funcionando.
Fluxo B — Resumo inteligente via arquivo
Usuário faz upload do arquivo.
Backend extrai o texto e retorna um preview.
Backend comunica com a API do Content Generator, que resume e o backend salva o resumo retornado.
frontend mostra progresso (spinner + status).
Exibir resultado estruturado + botão “Salvar como Resumo de Aula”.
Fluxo C — Resumo inteligente via tema/tópico
Usuário define um tema.
Backend comunica com a API do Content Generator, que resume e o backend salva o resumo retornado.
frontend mostra progresso (spinner + status).
Exibir resultado estruturado + botão “Salvar como Resumo de Aula”.
UTILIZAÇÃO DO COMPLETIONS e API Key
Para acessar o endpoint do content generator, chamada completions, considere as seguintes informações:

Url de acesso
https://platform-homologx.senior.com.br/t/senior-x/platform/iassist/api/latest/completions

Parametros que o completions recebe:
{
    "prompt": "Some text to send to IA", // Texto que será enviado para a IA
    "provider": "AZURE_OPEN_AI", // Provedor de inteligencia artifical.
    "parameters": { // Parâmetros que serão utilizados pelo provedor. Os parâmetros são um mapa de String e Object
        "max_tokens": 2000, // passando um valor do tipo integer
        "temperature": 0.5, // passando um valor do tipo float
        "model": "gpt-3.5-turbo" // passando um parametro do tipo string
    }
}
Retorno do endpoint
{
    "text": "Returned text from IA", // Texto que será retornado da inteligência artificial
    "promptTokens": 1, // Quantidade de tokens da pergunta
    "replyTokens": 2  // Quantidade de tokens da resposta
}
Obter API KEY:
Para utilização do endpoint, é necessário autenticação via header Authorization. Para obter o valor desse token, é necessário executar um POST no curl a seguir:

curl --location 'https://platform-homologx.senior.com.br/t/senior.com.br/bridge/1.0/anonymous/rest/platform/authentication/actions/loginWithKey' \
--header 'Content-Type: application/json' \
--data '{
    "accessKey": "**************************(verificar chave com o responsável da vaga)",
    "secret": "**************************(verificar chave com o responsável da vaga)",
    "tenantName": "iassistinterno"
}'
Irá retornar access_token.

Para utiliza-lo basta informar: Beaer {{access_token}}
