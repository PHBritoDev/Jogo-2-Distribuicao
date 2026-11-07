# Prontidão dos agentes antes de privatizar o código

**Situação em 07/11/2026:** o jogo do novo GitHub Pages já carregou no Chrome, a publicação foi aprovada pelo GitHub Actions, o token `SOURCE_READ_TOKEN` passou no teste de leitura e a automação de build foi ajustada para requerer esse token. O repositório de desenvolvimento **continua público** por decisão do criador enquanto Claude trabalha. Nenhuma migração automática de privacidade deve ocorrer.

## Como os agentes continuarão trabalhando

| Agente/serviço | Acesso exigido ao repositório-fonte privado | Como validar |
| --- | --- | --- |
| GitHub Actions deste repositório de distribuição | `SOURCE_READ_TOKEN`, selecionando **somente** `PHBritoDev/Jogo-2-2.5D` e permissão **Contents: Read-only** | Executar [Validar acesso privado antes da migracao](../../actions/workflows/verificar-acesso-fonte.yml) novamente **depois** da privatização |
| ChatGPT (conector GitHub) | GitHub App instalada na conta, seleção de repositórios incluindo o original; operações permitidas pela instalação | Abrir Settings → Applications → Installed GitHub Apps; reautorizar caso necessário. Só testar escrita em branch de trabalho, jamais direto no `main` |
| Claude | Método próprio de acesso ao GitHub utilizado no Claude; conta/integração deve ter permissão adequada no repositório privado | Revisar permissões na conta/integração do Claude e testar leitura/branch privada dentro do Claude |
| Codex | App/integração GitHub habilitada no repositório privado | Conferir repositórios selecionados nas configurações da integração e testar acesso depois da mudança |
| Agentes V5–V9, QA visual e GLB | `actions/checkout` do **próprio** repositório privado; workflows precisam estar ativados e Actions permitida para esse repositório | Executar testes de `workflow_dispatch` antes e depois da mudança; verificar permissões somente-leitura |
| Outros colaboradores humanos | Convite explícito no repositório privado, com permissão mínima | Revisar Settings → Collaborators and teams e remover acessos desnecessários |

**Limitação:** a conexão do ChatGPT e o token da distribuição já foram verificados, porém **não é possível confirmar antecipadamente que Claude e Codex terão acesso após a privatização** sem consultar suas integrações. Se algum agente perder acesso, ajustar seleção de repositórios na integração correspondente. Não inserir PAT de leitura em prompt ou chat.

## Ordem segura após o Claude encerrar a tarefa

1. Conferir PRs e merge final do Claude; guardar hash do último commit e testar o mundo 3D no navegador.
2. Executar manualmente [Preparar e publicar jogo](../../actions/workflows/publicar-jogo.yml), com `publish=true`, para atualizar o **novo** site com a versão final.
3. Confirmar o jogo em https://phbritodev.github.io/Jogo-2-Distribuicao/ e o mundo 3D no Android, com cenas, modelos, áudio, habilidades, laboratórios e progresso local.
4. Executar [Validar acesso privado antes da migracao](../../actions/workflows/verificar-acesso-fonte.yml). Confirmar sucesso e preservar o registro da execução.
5. Conferir que `SOURCE_READ_TOKEN` está salvo em **Settings → Secrets and variables → Actions** no repo de distribuição; não imprimir valor. Certificar que token não venceu e não está restrito só a repos públicos.
6. Conferir no GitHub **as permissões de cada integração** (Claude, Codex, ChatGPT e outros). Não liberar acesso amplo desnecessário a repositórios não relacionados.
7. Só após as etapas anteriores, alterar a visibilidade do **repositório-fonte** (não do repositório de distribuição), via Settings → Danger Zone → Change visibility → Private.
8. Executar imediatamente os dois workflows de distribuição de novo. Se funcionarem com repo privado, a publicação está protegida contra mudanças de visibilidade. Fazer novo deploy (`publish=true`) e confirmar URL.
9. Revalidar Claude, Codex e ChatGPT em novo branch de teste. Caso algum perca acesso, reautorizar individualmente, **sem reabrir o repo como público**.
10. Definir `AUTO_DEPLOY_PAGES=true` só quando quiser atualização automática **diária**, e se não houver trabalho simultâneo não aprovado em `main`. Enquanto Claude termina, manter a variável ausente ou `false`; deploy só manual.

## Cuidados

- O site antigo `https://phbritodev.github.io/Jogo-2-2.5D/` pode parar ao privatizar fonte. O novo endereço público continua independente.
- **GLBs, texturas, shaders e JavaScript necessários ao navegador permanecem copiáveis.** A separação protege os arquivos de desenvolvimento, não é DRM.
- Arquivos de desenvolvimento anteriormente expostos podem já ter sido copiados; privatização não elimina forks locais.
- O token de distribuição tem expiração: planejar renovação antes da data escolhida.
- Usuários com acesso de escrita ao repositório público de distribuição podem alterar workflows e potencialmente abusar dos Secrets usados em Actions. Mantenha acesso de escrita restrito aos administradores confiáveis.
- Evitar colocar o código-fonte completo no repositório de distribuição. O build publica apenas `site/` como artefato Pages e não usa commits de GLBs no branch `main` público.
- Não mudar scripts de jogo, configurações de câmera, física, VFX ou mapas para aplicar este checklist.

## Links úteis

- [Automação que publica jogo](../../actions/workflows/publicar-jogo.yml)
- [Verificação do token e build](../../actions/workflows/verificar-acesso-fonte.yml)
- [Instalações de GitHub Apps](https://github.com/settings/installations)
- [Chaves/segredos do repositório de distribuição](../../settings/secrets/actions)
