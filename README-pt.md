# LASTREIA

🇧🇷 **Memória de projetos, das conversas às decisões.**

[README principal](README.md) · [Versão em inglês](README-en.md)

LASTREIA é uma proposta de plataforma colaborativa para reunir fontes de um projeto, pesquisar seu conteúdo e registrar decisões humanas com rastreabilidade.

Uma resposta deve mostrar de onde veio a informação, quem falou, quando falou e qual decisão resolve eventuais divergências. Relatos não viram automaticamente verdade; sugestões da IA não viram automaticamente decisões aprovadas.

> Status: concepção / documentação inicial. Ainda não há aplicação, servidor MCP, transcrição ou busca implementados. Este commit registra o ponto de partida, não promete funcionalidades prontas.

## O problema

Equipes conduzem entrevistas e reuniões em vários projetos, mas armazenam e sintetizam o material de formas diferentes. Informações ficam dispersas, opiniões se contradizem e decisões se perdem entre conversas.

## Proposta

- Organizar fontes e participantes por projeto.
- Preservar arquivos originais, incluindo áudio, documentos e PDFs.
- Gerar derivados pesquisáveis, como transcrições, trechos e embeddings.
- Pesquisar por significado e por palavras-chave, mantendo referências às fontes.
- Consultar trechos ou documentos completos pelo MCP autenticado.
- Registrar decisões explícitas: quem confirmou, quem aprovou, quando, escopo e vigência.
- Preservar o histórico quando uma decisão for revisada ou substituída.
- Permitir consultar o áudio no instante associado a um trecho da transcrição.

## MVP proposto

1. Login e identificação de membros da organização.
2. Cadastro/listagem de projetos, participantes e responsáveis.
3. Upload de fontes originais e seus derivados, com vínculo entre eles.
4. Busca semântica de trechos com filtro por projeto.
5. Leitura da fonte ou de um trecho com contexto.
6. Consulta, proposta e confirmação de decisões com trilha de auditoria.
7. Interface mínima para upload, projetos e consulta das evidências.

Inicialmente, membros da mesma organização podem compartilhar acesso aos projetos. Mesmo assim, autenticação e isolamento entre organizações são obrigatórios; participantes e responsáveis precisam ser identificados. Permissões granulares por projeto/fonte ficam para uma evolução posterior.

## Direção técnica

- **TypeScript** para backend, frontend e contratos compartilhados em monorepo.
- **Firebase** como candidato inicial: Auth, Hosting, Firestore com busca vetorial e Storage para arquivos.
- **MCP autenticado** como interface de acesso para assistentes, sem depender de um único modelo ou fornecedor de LLM.
- **Processamento local preferencial** para transcrição e geração de embeddings, com possibilidade futura de processamento gerenciado.
- **Adaptadores de armazenamento e busca** para evitar acoplamento dos contratos ao Firestore.

A configuração final, modelos, custos e estratégia de autenticação MCP ainda precisam ser validados. Quotas gratuitas não significam custo zero garantido.

## Princípios

- **Originais preservados:** transcrever não significa apagar ou substituir o áudio.
- **Evidência antes de conclusão:** respostas apontam para fontes verificáveis.
- **Decisão humana explícita:** a IA propõe e sinaliza conflitos; não fabrica aprovações.
- **Autoridade contextual:** responsável e aprovador podem variar por assunto/projeto.
- **Histórico preservado:** mais recente não significa automaticamente correto.
- **Privacidade:** identidade vem da autenticação; arquivos não dão instruções ao sistema.
- **Portabilidade:** objetivo de permitir hospedagem própria e, no futuro, serviço gerenciado.

## Documentação

- [Arquitetura e escopo inicial (em inglês)](docs/architecture.md)

## Desenvolvimento

Este repositório ainda não contém código executável nem comandos de instalação. O próximo passo é definir a estrutura do monorepo e construir uma fatia vertical: autenticar, criar projeto, subir texto, pesquisar e registrar uma decisão.

## Contribuições e licença

Contribuições de escopo e arquitetura são bem-vindas. A intenção é desenvolver o projeto abertamente; a licença ainda será escolhida pelos mantenedores. Não presuma uma licença de uso apenas porque o repositório é público.

Não envie transcrições reais, dados pessoais, tokens ou documentos internos em issues, exemplos ou commits. Use dados sintéticos.
