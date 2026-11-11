# Notas Mapão

Mostra a nota e as faltas de cada aluno a partir da planilha do mapão (`.xlsx`), com abas por bimestre, gráficos de evolução e um cartão em imagem para o responsável. Publicado em `https://notas-mapao.onrender.com` e em `/mapao/` do domínio principal.

## Arquivos
- `index.html`: o app inteiro (CSS, HTML e JavaScript). Há um `<script>` enorme com a biblioteca SheetJS embutida (linhas muito longas). **Não edite, não reformate e não leia esse trecho inteiro**: ele fica entre `<script>` e o segundo `<script>`. A lógica do app está no segundo `<script>`.
- Sem build e sem dependências.
- Contador de visitas: GoatCounter (`notasmapao.goatcounter.com`), script no fim do arquivo.

## Como o app lê a planilha (`parse()`)
- Procura a linha com a célula `ALUNO` e lê o cabeçalho em dois níveis: nome da disciplina (célula mesclada) e, embaixo, os rótulos `M` (nota/menção), `F` (faltas) e `AC` (ausência compensada). As colunas são descobertas pelos rótulos, nunca por posição fixa. Uma versão antiga errava por uma coluna por isso.
- Bloco `TOTAL` com `TF`, `Fre(%)`, `FT An`, `Fre An(%)`. Linhas "Aulas Dadas: N" dão o total de aulas por disciplina. A legenda no fim da planilha explica menções não numéricas (ES, EP, ET).
- Nomes de disciplinas vêm sem acento: o app restaura acentos com um dicionário (`ACENTOS`) e capitalização (`bonito()`).
- O bimestre vem de "Tipo Fechamento" (`periodoInfo()`). Vários arquivos da mesma turma viram abas; planilhas de turmas diferentes viram um seletor de turma.

## Recursos
- Evolução por bimestre: gráficos em SVG gerados à mão (`linhas()` e `barras()`), sem biblioteca.
- Cartão do aluno ("Cartão para foto"): imagem PNG desenhada em `<canvas>` (`desenharCartao()`), com salvar e compartilhar. Mostra só o aluno em foco, **sem média da turma e sem nomes de outros alunos**.
- Nota mínima configurável na lateral (padrão 5). Frequência mínima fixa em 75%.

## Cuidados
- Tudo é lido no navegador: nada da planilha é enviado a servidor. Mantenha assim.
- Para testar, gere planilhas inventadas com a mesma estrutura (openpyxl) ou use a planilha que o Alexandre anexar na conversa. Nunca a salve no repositório.

## Idioma e público
- Tudo em português do Brasil (interface, textos, commits e respostas).
- Público: professores da rede estadual de SP (usam no computador e no celular, em redes de escola). Mantenha a interface simples e legível.

## Identidade visual (igual em todas as Ferramentas do Professor)
- Paleta índigo: botão principal `#4f46e5` (hover `#4338ca`), fundos suaves `#eef2ff`, texto `#0f172a` e `#475569`, bordas `#e2e8f0`.
- Fonte Plus Jakarta Sans (Google Fonts), fundo em degradê `#eef2ff` → `#fff` → `#faf5ff` com manchas de cor desfocadas.
- Cartões com cantos bem arredondados (24 a 28 px), botões e abas em formato pill (totalmente arredondados), ícones SVG no estilo Lucide.
- Só tema claro (sem modo escuro).
- Barra de topo com o logo do site e o botão "Ferramentas do Professor" que volta para `https://www.ferramentasdoprofessor.com.br/`.
- Rodapé: "Desenvolvido por Prof. Alexandre Santos" + link "Contato" para `https://www.ferramentasdoprofessor.com.br/contato/`. Não escreva "Especialista" no nome.
- Cores semânticas das etiquetas: âmbar = notas e faltas, azul-céu = AVA/fechamento, roxo = orientação, esmeralda = estágio.

## Privacidade
- Planilhas e relatórios têm dados de alunos (menores). Nunca coloque dados reais de alunos no repositório, em exemplos, testes ou capturas de tela. Use dados inventados.
- Nunca grave telefone, e-mail ou chaves de API em arquivos do repositório (os repositórios, exceto o vagas-ciee, são públicos).

## Fluxo de trabalho com Git
- Faça as mudanças numa branch `claude/<assunto>` a partir da `main` atualizada (`git fetch origin main`).
- Mudanças de visual, lógica ou novas funções: envie a branch e peça aprovação ao Alexandre antes do merge na `main`.
- Ajustes de texto pedidos de forma explícita podem ir direto para a `main`.
- O merge é um avanço direto (fast-forward): `git push origin <branch>:main`. Só faça se `main` for ancestral da branch.
- A `main` publica sozinha no Render. Não crie pull request se não for pedido.
- Não faça commit de `package-lock.json`, `node_modules/` nem `dist/`.
- Mensagens de commit em português, no imperativo, com o corpo explicando o porquê.
- O aviso do hook sobre "commit sem branch remota" pode aparecer atrasado: confirme com `git ls-remote --heads origin <branch>` antes de reenviar.

## Como testar
- Use o Playwright (já instalado: `require('playwright')`) e confira sempre em 1280 px e em 390 px de largura. Verifique que não há rolagem horizontal (`scrollWidth <= innerWidth`) e que não há erros de página.
- A rede da sessão bloqueia vários endereços (CDNs, Google Fonts, GoatCounter, onrender.com). Isso não é defeito do código: as fontes caem para a fonte do sistema e o contador não carrega nos testes locais.
- Capturas de tela com `fullPage` mostram a barra fixa no meio da página: é efeito da captura, não bug.
