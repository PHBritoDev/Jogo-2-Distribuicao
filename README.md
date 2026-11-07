# A Batalha Contra os 7 Pecados Capitais — distribuição pública

Este repositório é destinado **exclusivamente à publicação** do jogo para navegador.

- **Projeto original de desenvolvimento:** `PHBritoDev/Jogo-2-2.5D` (migrará para privado após a nova URL ser testada).
- **Conteúdo distribuído:** apenas os arquivos preparados pelo empacotador de publicação; os documentos, os scripts de criação de modelos, os testes internos e o histórico de desenvolvimento ficam no projeto original.
- **Aviso importante:** qualquer modelo 3D, textura e JavaScript carregado pelo Chrome continuará tecnicamente acessível ao usuário.
- **Direitos autorais:** conteúdo original © 2026 PHBritoDev. Direitos reservados; materiais de terceiros seguem as próprias licenças.

## Como ativar a publicação

1. Abra **Settings → Pages** neste repositório e selecione **Source: GitHub Actions**.
2. Abra **Actions → Preparar e publicar jogo (codigo-fonte separado) → Run workflow** e marque **publish: true**.
3. Confira no workflow que a build, os testes e a publicação foram concluídos. Teste **na prática** no Android:
   - https://phbritodev.github.io/Jogo-2-Distribuicao/
   - https://phbritodev.github.io/Jogo-2-Distribuicao/mundo-3d-prototype.html
4. **Só depois** de confirmar que o novo link funciona, configure um GitHub **fine-grained personal access token (PAT)** com permissão de **Contents: read-only** somente no repositório de desenvolvimento. Adicione-o em **Settings → Secrets and variables → Actions → New repository secret** sob o nome `SOURCE_READ_TOKEN`. Isso permite que a publicação continue quando o original ficar privado. **Nunca publique ou envie o token no chat**.
5. Antes de tornar o projeto original privado, faça novamente **Run workflow** para confirmar que o token consegue acessar o código de origem; reexecute após privatizá-lo.
6. Para publicar atualizações automaticamente, em **Settings → Secrets and variables → Actions → Variables**, crie `AUTO_DEPLOY_PAGES` com valor `true`. A rotina verifica `main` do projeto original diariamente. Para publicação imediata, use **Run workflow → publish: true**.
7. A mudança para repositório privado pode interromper o **link antigo** `Jogo-2-2.5D`. Avise os jogadores sobre o novo endereço. **Não altere a privacidade sem verificar o novo site.**

## Segurança do fluxo

A Action faz checkout do projeto de desenvolvimento em um runner efêmero, empacota a versão estática com `tools/public-release/build.cjs`, executa testes de conteúdo e hashes, e envia **apenas** o diretório `site/` ao GitHub Pages.

As credenciais ficam no armazenamento de **Secrets** do GitHub. O token de leitura não deve ter permissão de escrita nem acesso a outros repositórios.

O GitHub Pages publica uma versão para navegador, **não proteção anticópia**: código JS e arquivos 3D entregues ao cliente podem ser recuperados por DevTools/requisições de rede.
