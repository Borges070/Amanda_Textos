# Explicação do código (ex1.py)

Este arquivo explica passo a passo cada função do `ex1.py`, indicando o que acontece em cada etapa e o que é retornado.

## Imports iniciais
- `re`: expressões regulares, usadas para limpar espaços repetidos.
- `unicodedata`: para identificar categorias Unicode dos caracteres (letras, números, sinais etc.).
- `collections.Counter`: estrutura para contar itens (usada em algumas partes).
- `pathlib.Path`: manipulação de caminhos de arquivos.
- `nltk` e corpora: usados para carregar textos de exemplo (Gutenberg, Machado e stopwords).

---

## `garantir_nltk_corpus(nome: str)`
O que faz:
1. Tenta localizar o corpus `nome` na instalação local do NLTK usando `nltk.data.find`.
2. Se não encontrar (produz `LookupError`), chama `nltk.download(nome, quiet=True)` para baixar o corpus.

Por que usar:
- Evita erro quando o script tenta usar `gutenberg.raw(...)` ou `machado.raw(...)` sem ter os dados baixados.

Retorno:
- Não retorna nada (None). A função faz efeito colateral: garante que o corpus exista localmente.

---

## `limpar(texto: str) -> str`
O que faz (etapa a etapa):
1. `texto = texto.lower()` — converte tudo para minúsculas para uniformizar.
2. Percorre cada caractere `ch` em `texto`.
3. Usa `unicodedata.category(ch).startswith('L')` para verificar se `ch` é uma letra (qualquer tipo: minúscula, maiúscula, letra de outro alfabeto, acentuada etc.).
4. Se for letra, adiciona `ch` à lista `letras`.
5. No final, `return ''.join(letras)` junta todas as letras em uma string contínua, sem espaços, números ou pontuação.

Observação sobre `startswith('L')`:
- `unicodedata.category(ch)` retorna códigos como `Ll`, `Lu`, `Lo` (tipos de letra). `startswith('L')` pega qualquer categoria que represente letra.

Retorno:
- String contendo apenas as letras, todas em minúsculas, sem espaços nem pontuação.

---

## `limpar_texto_para_palavras(texto: str) -> str`
O que faz (etapa a etapa):
1. `texto.lower()` para minúsculas.
2. Para cada caractere `ch`:
   - se for letra, adiciona `ch` ao resultado;
   - caso contrário, adiciona um espaço (`' '`) — assim números e pontuação viram separadores.
3. `texto = ''.join(resultado)` — junta a lista em uma string.
4. `texto = re.sub(r'\s+', ' ', texto).strip()` — reduz sequências de espaços múltiplos para um espaço único e remove espaços nas extremidades.

Retorno:
- String onde as palavras estão separadas por um único espaço e pronta para `split()`.

---

## `ocorrencias(texto: str)`
O que faz (etapa a etapa):
1. Chama `limpar(texto)` e recebe `texto_limpo` — só letras, minúsculas e sem espaços.
2. Cria um dicionário `contagem = {}`.
3. Para cada `letra` em `texto_limpo`, incrementa `contagem[letra]` (soma 1) ou cria a entrada com valor 1 se não existir.
4. `lista_ordenada = list(contagem.items())` cria uma lista de pares `(letra, frequencia)`.
5. `lista_ordenada.sort(key=lambda item: (-item[1], item[0]))` — ordena por frequência decrescente; em caso de empate, ordena alfabeticamente.

Retorno:
- `contagem`: dicionário `{letra: frequencia}`
- `lista_ordenada`: lista ordenada de tuplas `[(letra, frequencia), ...]` com as letras mais frequentes primeiro

Uso recomendado:
- Útil para o Exercício 1: a função devolve tanto o mapa de frequências quanto a lista ordenada.

---

## `contar_palavras(texto: str)`
O que faz (etapa a etapa):
1. Chama `limpar_texto_para_palavras(texto)` para obter uma string onde as palavras estão separadas por espaços.
2. `palavras = texto_limpo.split()` quebra em lista de palavras.
3. Conta cada palavra usando um dicionário `contagem` (incrementando ou inicializando em 1).
4. Finalmente retorna `Counter(contagem)` — um `Counter` com as frequências.

Retorno:
- `Counter` com `{'palavra': frequencia, ...}`

---

## `riqueza_lexical(contador_palavras: Counter) -> float`
O que faz:
1. `total_palavras = sum(contador_palavras.values())` — total de tokens no texto.
2. `tipos = len(contador_palavras)` — número de palavras distintas (tipos).
3. Se `total_palavras == 0`, retorna `0.0` para evitar divisão por zero.
4. Caso contrário retorna `tipos / total_palavras` — medida da riqueza lexical.

Retorno:
- Float entre 0 e 1 representando a proporção de tipos por token.

---

## `hapax_legomena(contador_palavras: Counter)`
O que faz:
1. Itera sobre `contador_palavras.items()` e seleciona palavras cuja frequência é exatamente 1.
2. Ordena essa lista e retorna.

Retorno:
- Lista ordenada de palavras que aparecem uma única vez no texto (hápax legômena).

---

## `top_n(counter: Counter, n=20)` e `bottom_n(counter: Counter, n=20)`
- `top_n`: usa `counter.most_common(n)` para retornar os `n` itens mais frequentes.
- `bottom_n`: transforma em lista de itens, ordena por frequência crescente e retorna os `n` primeiros.

Retorno:
- Lista de tuplas `(item, frequencia)`.

---

## `imprimir_top_20_palavras(contador: Counter, titulo: str)`
O que faz:
- Imprime um cabeçalho e as 20 palavras mais frequentes usando `top_n`.

Retorno:
- None (efeito colateral: impressão no console).

---

## `alfabeto_do_texto(texto: str)` e `letras_unicas(alfabeto1, alfabeto2)`
- `alfabeto_do_texto`: chama `limpar(texto)` para obter apenas letras e então faz `set(...)` para obter letras únicas.
- `letras_unicas`: faz `alfabeto1 - alfabeto2` e retorna ordenado.

Retorno:
- conjuntos/listas de letras únicas.

---

## `carregar_texto_arquivo(caminho: Path) -> str`
O que faz:
- Lê todo o conteúdo de um arquivo com `encoding='utf-8'` e `errors='ignore'` (ignora possíveis bytes inválidos).

Retorno:
- String com o conteúdo do arquivo.

---

## `imprimir_contagem_letras(nome: str, texto: str, n=10)`
O que faz:
1. Chama `ocorrencias(texto)` para obter `lista_ordenada`.
2. Imprime as `n` letras mais frequentes conforme a lista ordenada.

Retorno:
- None (impressão).

---

## `main()` — fluxo geral
1. Garante corpora NLTK necessários: `gutenberg`, `machado`, `stopwords`.
2. Exercício 1: usa `ocorrencias` em um texto pequeno de exemplo.
3. Exercício 2: carrega dois livros do Gutenberg e imprime as letras mais frequentes.
4. Exercício 3: compara inglês e português usando um livro de cada (Emma e Memórias Póstumas).
5. Exercício 4: analisa `marm05.txt` (Memórias Póstumas): número de caracteres, palavras, tipos e riqueza lexical.
6. Exercício 5/6: se existir `ubirajara.txt` no diretório, compara top 20, retira stopwords e calcula hápax comuns e quantidades.

Retorno:
- O `main()` não retorna nada; apresenta saídas impressas que respondem aos exercícios.

---

## Observações finais
- O código já está organizado em funções pequenas; para um estudante iniciante a versão atualiza0 (com loops explícitos) facilita a leitura.
- Para obter resultados reais, rode `ex1.py` após executar `install_and_setup.py` ou instalar `nltk` e baixar os corpora necessários.


