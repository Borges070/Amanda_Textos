# Respostas da lista de exercícios (capítulo 2)

Este arquivo contém explicações e respostas em língua natural para a lista de exercícios pedida no enunciado. As implementações estão em `ex1.py`; respostas discursivas devem constar como comentário no script (conforme instrução do trabalho).

---

## Instruções gerais para submissão
- Envie o arquivo `.py` com as resoluções (por exemplo `fulane_LP_final.py`).
- Insira respostas discursivas como comentários (`# comentário`) dentro do script ao lado ou acima do código que gera o resultado.
- Anexe também o arquivo do corpus usado (por exemplo `ubirajara.txt`) se você o usou na comparação.

---

## Exercício 1
(a) Recebe uma string e elimina dela os caracteres não-alfabéticos:
- Em `ex1.py` a função que resolve isso é `limpar(texto)`.
- Passos: converte para minúsculas (`texto.lower()`), percorre caractere a caractere e mantém apenas aqueles cuja categoria Unicode começa com `L` (letras). Retorna uma string contendo só letras.

(b) Cria um dicionário que tem como chaves as letras e como valores as contagens:
- A função `ocorrencias(texto)` chama `limpar(texto)`, percorre as letras e monta um dicionário `contagem` onde `contagem[letra]` é incrementado a cada ocorrência.

(c) Retorna uma lista ordenada das letras por número de ocorrências:
- `ocorrencias` também constrói `lista_ordenada` com tuplas `(letra, frequencia)` ordenadas por frequência decrescente e, em empate, por letra.

**Observação (como entregar no script):** inclua comentários no arquivo `.py` explicando cada parte (por exemplo: `# Esta função retorna o dicionário e a lista ordenada conforme o item (b) e (c)`).

---

## Exercício 2
- Procedimento: aplique a função do Exercício 1 em dois livros inteiros (por exemplo, `gutenberg.raw('austen-emma.txt')` e `gutenberg.raw('melville-moby_dick.txt')`).
- Em `ex1.py` isso já é feito na seção do `main()` que carrega `textos_ingles` e chama `imprimir_contagem_letras`.

Comparação/Interpretação (resposta discursiva):
- Pode haver diferenças nas letras mais frequentes por causa do gênero, estilo e vocabulário do autor. Porém, em inglês, letras como `e`, `t`, `a`, `o`, `i`, `n` costumam aparecer com alta frequência em textos longos.
- Diferenças sutis entre romances (diferente uso de diálogo, nomes próprios, termos técnicos) vão refletir-se nas frequências relativas.

**Como apresentar no script:** imprima ou grave as duas `lista_ordenada` e comente as observações.

---

## Exercício 3
Procedimento já implementado em `ex1.py` (com `Emma` e `Memórias Póstumas`):
- Carregar dois livros (uma obra em inglês e outra em português), aplicar `ocorrencias` e `alfabeto_do_texto`.

Respostas esperadas (discussão):

(a) Alfabetos usados nas duas línguas:
- Inglês: alfabeto latino básico (a–z), normalmente sem acentuação.
- Português: alfabeto latino mais letras acentuadas usadas no texto (á, é, í, ó, ú, ã, õ, ç etc.), desde que o texto contenha essas formas.

(b) Letras presentes em um alfabeto e não no outro:
- Letras acentuadas (á, ç, ã, õ, ê, ô, etc.) tendem a aparecer em português e não em inglês.
- Letras como `k`, `w` e `y` historicamente aparecem raramente no português tradicional (dependendo do corpus), mas isso depende do texto.

(c) Letras mais frequentes em cada língua (tendência geral):
- Inglês (típico): `e, t, a, o, i, n, s, r`.
- Português (típico): `a, o, e, s, r, d` (varia por corpus e limpeza).

(d) Letras menos frequentes e hápax legômena:
- Letras menos frequentes: `q, z, x, w, k` (depende da língua e do corpus).
- Hápax legômena (letras que aparecem uma única vez) ocorrem menos nas letras do que nas palavras; em palavras, hápax indicam vocabulário raro, nomes próprios, formas arcaicas ou erros de OCR.

---

## Exercício 4 (Memórias Póstumas: procedimentos)
Procedimentos a realizar (o `ex1.py` já implementa estas etapas):
- `numero_caracteres = len(texto_machado)` — conta chars.
- `contador_palavras_machado = contar_palavras(texto_machado)` — limpa e conta palavras.
- `riqueza = riqueza_lexical(contador_palavras_machado)` — tipos / tokens.

Perguntas e respostas (orientação para comentário no script):

- As vinte palavras mais frequentes do romance Memórias Póstumas são semelhantes às de Ubirajara?
  - Provavelmente não idênticas; antes da remoção de stopwords, a lista conterá muitas palavras vazias (por exemplo: `a`, `o`, `de`, `e`, `que`). Essas listas são fortemente influenciadas por frequências de functors/gramaticais.
  - Quando se compara o uso concreto da língua, se ambas as obras mostram as mesmas palavras gramaticais no topo, isso indica que as diferenças lexicais aparecem mais quando retiramos as stopwords.

- O que isso representa em termos linguístico-discursivos?
  - Palavras funcionais (stopwords) dominam as listas de frequência; para análise de conteúdo (temas, tópicos), é preciso remover stopwords e analisar as palavras de conteúdo (substantivos, verbos significativos).

---

## Exercício 5 (remover stopwords)
Procedimento:
- `stopwords_portugues = set(stopwords.words('portuguese'))` e filtrar as top 100 palavras retirando essas stopwords (feito em `ex1.py`).

Resposta/Interpretação:
- Após remover stopwords, as listas mostram termos temáticos — nomes, substantivos, verbos relevantes ao conteúdo das obras.
- Comparar as duas listas sem stopwords revela diferenças de estilo, tema e vocabulário entre as obras.

---

## Exercício 6 (hápax legômena)
Procedimento:
- `hapaxes_machado = hapax_legomena(contador_palavras_machado)` usa `contar_palavras` para obter o `Counter` e filtrar palavras com frequência 1.

Perguntas e orientação de resposta:
- Comparando a extensão da lista de hápax entre Machado e Ubirajara, o que inferir?
  - Em geral, o número de hápax tende a crescer com o tamanho do corpus. Um texto muito grande terá muito vocabulário e, portanto, muitos hápax (palavras raras, nomes, termos específicos). Se Ubirajara for menor, pode ter menos hápax; se for maior, pode ter mais.
  - Interpretação: muitos hápax indicam vocabulário mais heterogêneo ou presença de muitos nomes próprios, termos raros e variações.

- Hápax comuns entre os dois textos:
  - Palavras que aparecem exatamente uma vez em cada corpus e coincidem (por exemplo, nomes muito raros ou formas arcaicas comuns a ambos). Em `ex1.py` isso é calculado com `sorted(set(hapaxes_machado) & set(hapaxes_ubirajara))`.

---

## Como obter resultados exatos (comandos rápidos)
1. Crie `venv` e instale dependências (ou rode `install_and_setup.py`).

2. Rodar o script principal:

Windows:
```powershell
venv\Scripts\python ex1.py
```

Unix/Mac:
```bash
venv/bin/python ex1.py
```

3. Para obter listas diretamente em um REPL Python, você pode importar as funções do `ex1.py` (modifique o arquivo para permitir import, por exemplo movendo a chamada de `main()` para `if __name__ == '__main__'` — já está assim).

Exemplo (Python REPL):
```python
from ex1 import contar_palavras, hapax_legomena
texto = open('ubirajara.txt', encoding='utf-8').read()
contador = contar_palavras(texto)
print(contador.most_common(20))
print(len(hapax_legomena(contador)))
```

---

## Observações finais e entrega
- Coloque respostas discursivas diretamente no script como comentários `#` junto ao bloco que implementa cada exercício; isso atende ao requisito do enunciado.
- Anexe o(s) arquivo(s) de corpus (por exemplo `ubirajara.txt`) na submissão.
- Se quiser, eu posso gerar um arquivo `fulane_LP_final.py` já com comentários de resposta embutidos seguindo o padrão exigido — quer que eu gere esse arquivo automaticamente a partir de `ex1.py` e das respostas que aqui escrevi?


