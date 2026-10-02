# Relatório Fotográfico · Belém Limpa

Ferramenta web que monta o relatório fotográfico mensal das Bases Norte e Sul a partir das fotos do WhatsApp, no mesmo padrão visual do modelo da fiscalização.

Tudo roda **no navegador de quem usa**: a leitura dos carimbos é feita por um leitor de texto (OCR) local, sem custo. As fotos só saem do navegador se você ligar, por conta própria, a sugestão de serviço pelo Gemini (opcional).

## Como usar

1. Escolha o mês e como organizar o relatório: separado por serviço ou tudo junto.
2. Arraste os `.zip` exportados do WhatsApp (Exportar conversa → Incluir mídia), fotos `.jpg` soltas ou uma pasta inteira.
3. Clique em **Ler fotos**. A ferramenta lê só as fotos necessárias, espalhadas pelos dias do mês.
4. Revise: tire fotos com ✕, marque várias para trocar serviço ou base, corrija dados clicando na foto.
5. Clique em **Gerar PDF**.

## Publicar no GitHub + Vercel

O site é o `index.html` mais as pastas `tess` e `hp`. Não precisa de build.

**Pelo navegador (sem instalar nada):**
1. No GitHub, crie um repositório vazio → **Add file → Upload files**.
2. Arraste **o `index.html`, as pastas `tess` e `hp` e o `vercel.json`** (o conteúdo desta pasta, não a pasta em si) → **Commit changes**.
3. Confira na página do repositório: na raiz devem aparecer `index.html`, `vercel.json`, a pasta `tess` (5 arquivos) e a pasta `hp` (5 arquivos). Se aparecer uma pasta com outro nome contendo tudo, o upload ficou um nível abaixo; mova os arquivos para a raiz.
4. Na Vercel: **Add New → Project** → importe o repositório → *Framework Preset*: **Other** → sem Build Command e sem Output Directory → **Deploy**.
5. Abra o site. Se ele ainda mostrar a versão antiga, recarregue com Ctrl+Shift+R (a data da versão fica no código da página: clique com o botão direito → Exibir código-fonte e procure por "versão").

**Pelo terminal:**
```bash
git init && git add . && git commit -m "Relatório fotográfico"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/relatorio-fotografico.git
git push -u origin main
```

Abrir o `index.html` com dois cliques também funciona para montar o relatório, mas o leitor de carimbos (OCR) só funciona com a página publicada ou num servidor local (`npx serve .`).

## Estrutura

| Caminho | O que é |
|---|---|
| `index.html` | A página inteira: código, logos, fontes, gerador de PDF (jsPDF, MIT), leitor de .zip (zip.js, BSD-3) |
| `tess/*` | Leitura normal: Tesseract.js (Apache-2.0) e dicionário de português |
| `hp/*` | Leitura de alta precisão: PaddleOCR PP-OCRv4 (Apache-2.0) rodando com onnxruntime-web (MIT). Baixado só quando usado |
| `vercel.json` | Faz o navegador sempre buscar a versão nova da página |

## Volume

- O .zip é lido sob demanda: dá para importar dezenas de milhares de fotos sem estourar a memória.
- A leitura (OCR) leva cerca de 1 segundo por foto. Por isso, no modo **Automático**, a ferramenta lê só o suficiente para montar o relatório (cerca de 1,6× o número de fotos que vão no PDF), escolhendo fotos de dias diferentes.
- As leituras e correções ficam guardadas no navegador. Importar o mesmo arquivo de novo não repete o trabalho.
