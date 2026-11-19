# Marketplace

Um marketplace de plugins para o [Claude Code](https://code.claude.com/docs/pt/overview) e o [Codex](https://developers.openai.com/codex).

[Click here for the English version](README.en.md)

## Plugins disponíveis

| Plugin | Descrição |
|--------|-----------|
| [cofounder](skills/) | Um cofundador movido por IA que guia você da ideia ao app publicado na internet. |

## Instalando como plugin do Claude Code

Dentro do Claude Code, rode:

```
/plugin marketplace add locaweb/cofounder-plugin
/plugin install cofounder@locaweb
```

Para começar, abra o Claude Code na pasta do projeto e rode `/cofounder:iniciar` (ou peça algo como "quero criar um app"). Na primeira vez, o cofounder grava um `AGENTS.md` e um `CLAUDE.md` no projeto: a partir da sessão seguinte, ele inicia sozinho naquela pasta.

Para receber as atualizações automaticamente, ative o auto-update em `/plugin` → **Marketplaces** → **locaweb** → **Enable auto-update**. Para atualizar manualmente, rode `claude plugin update cofounder@locaweb` no terminal.

## Instalando como plugin do Codex

No terminal, adicione o marketplace:

```
codex plugin marketplace add locaweb/cofounder-plugin
```

Depois abra o `codex`, rode `/plugins`, instale o **cofounder** do marketplace **locaweb** e inicie uma nova sessão. Na primeira sessão em cada projeto, peça para usar o cofounder (por exemplo, "quero criar um app com o cofounder"); ele grava um `AGENTS.md` no projeto e, a partir da sessão seguinte, inicia sozinho naquela pasta. Também na primeira sessão, o cofounder pede sua aprovação uma vez para configurar o projeto (`.codex/config.toml` com acesso total, e a pasta marcada como confiável no Codex), para poder instalar ferramentas e rodar comandos sem pedir permissão a cada passo; isso vale a partir da sessão seguinte. Para atualizar, rode `codex plugin marketplace upgrade locaweb`.

## Como o cofounder é iniciado

O plugin não roda nada sozinho quando uma sessão começa: o cofounder só age depois que você o chama.

1. **Primeira vez em um projeto:** abra o agente em uma pasta vazia (para um projeto novo) ou em um projeto já criado pelo cofounder e rode `/cofounder:iniciar` no Claude Code. No Codex, peça para usar o cofounder. Em geral, basta pedir algo como "quero criar um app".
2. **Ativação no projeto:** nessa primeira sessão, o cofounder grava um bloco de instruções no `AGENTS.md` do projeto e um `CLAUDE.md` que o importa. Se esses arquivos já existirem, o conteúdo que você escreveu é mantido.
3. **Próximas sessões:** o agente lê esses arquivos ao abrir a pasta e inicia o cofounder sozinho, sem precisar do `/cofounder:iniciar`.

Para desativar em um projeto, apague o bloco entre `<!-- cofounder:begin -->` e `<!-- cofounder:end -->` no `AGENTS.md` e no `CLAUDE.md`.

## Verificando a instalação

Cada versão do plugin é publicada como um [GitHub Release](https://github.com/locaweb/cofounder-plugin/releases) com um tarball do plugin e um `SHA256SUMS`. O tarball tem attestation de proveniência que prova que foi gerado pelo workflow `release.yml` a partir de um commit específico deste repositório. Para verificar, use o [GitHub CLI](https://cli.github.com) autenticado:

```bash
VERSION=v0.29.1
gh release download "$VERSION" --repo locaweb/cofounder-plugin --dir "cofounder-plugin-$VERSION"
cd "cofounder-plugin-$VERSION"
sha256sum -c SHA256SUMS   # no macOS: shasum -a 256 -c SHA256SUMS
gh attestation verify "cofounder-plugin-$VERSION.tar.gz" --repo locaweb/cofounder-plugin
```

O `install.sh` e o tarball das skills são publicados nas [Releases do locaweb/cofounder](https://github.com/locaweb/cofounder/releases), também com `SHA256SUMS` e attestation. Para verificá-los, repita os comandos acima com `--repo locaweb/cofounder` e rode `gh attestation verify install.sh --repo locaweb/cofounder`.

## Desenvolvimento

As skills são desenvolvidas no [locaweb/cofounder](https://github.com/locaweb/cofounder), que também atende quem instala via `npx skills`. A pasta `skills/` deste repositório é uma cópia de uma tag de lá, feita pelo workflow `sync-skills.yml`: não edite essa pasta aqui, porque a próxima cópia sobrescreve as mudanças. Aqui ficam só os manifestos do plugin, o comando `/cofounder:iniciar` e os workflows de cópia e de release.

## Licença

Functional Source License (FSL-1.1-ALv2) — veja [LICENSE](LICENSE).
