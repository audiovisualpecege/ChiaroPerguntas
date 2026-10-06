# Chiaro Perguntas

Extensão do Chrome que leva a pergunta do aluno (detectada no chat ou capturada
na tela) para a planilha do Google Sheets do local e, dali, para o title do vMix.

Instalação sem loja. Este repositório guarda os **releases**: cada um traz o
instalador e o pacote assinado que o atualizador automático baixa.

## Instalar

1. Baixe `Chiaro-Perguntas-V<versão>.zip` do [último release](https://github.com/audiovisualpecege/ChiaroPerguntas/releases/latest) e extraia.
2. Dê dois cliques em `INSTALAR.bat`.
3. No Chrome: Modo do desenvolvedor → **Carregar sem compactação** →
   `%LOCALAPPDATA%\Chiaro Perguntas\extensao` (o caminho já fica copiado).

## Atualização automática

O `INSTALAR.bat` registra a tarefa do Windows **Chiaro Perguntas - Atualizador**
(do próprio usuário, sem administrador), que roda a cada 30 minutos:

1. pergunta ao GitHub qual é o release marcado como *latest*;
2. se for mais novo que o instalado, baixa `ChiaroPerguntas-<versão>.zip` e `.zip.sig`;
3. confere a assinatura RSA (SHA-256) com a chave pública embutida no
   atualizador — pacote adulterado é recusado antes de mexer em qualquer arquivo;
4. guarda a versão atual em `_atualizacao\anterior`, troca os arquivos e
   deixa o `manifest.json` por último. Se a troca falhar, volta ao que era.

O painel da extensão percebe a versão nova no disco e mostra a faixa
**Versão X pronta · Atualizar**. Nada recarrega sozinho, para não fechar o
painel no meio de uma transmissão. Reiniciar o Chrome também aplica.

Registro: `%LOCALAPPDATA%\Chiaro Perguntas\atualizador.log`.

## Lançar uma versão

No PC de quem publica (com `chave-atualizacao.xml` e o GitHub CLI logado):

1. Suba o `"version"` em `extensao_chrome/manifest.json`.
2. Rode:

   ```powershell
   powershell -ExecutionPolicy Bypass -File publicar_versao.ps1 -Notas "O que mudou"
   ```

   Ele gera os pacotes, assina e cria o release `v<versão>` com os três anexos:
   `Chiaro-Perguntas-V<versão>.zip` (instalação), `ChiaroPerguntas-<versão>.zip`
   e `ChiaroPerguntas-<versão>.zip.sig` (atualizador).

O atualizador só enxerga o release *latest* (não marque como pre-release) e
procura os anexos exatamente com esses nomes.

**A chave.** `chave-atualizacao.xml` assina os pacotes e **nunca** vai para o
GitHub. Quem tiver ela consegue mandar código para todos os PCs; sem ela, a
próxima versão só chega pelo `INSTALAR.bat`. Guarde um backup fora deste PC.
