В данном проекте предлагается алгоритм, основанный на сравнении деревьев грамматик зависимостей и использовании контекстно-зависимых векторных представлений слов, полученных с помощью алгоритма машинного обучения.

### Источники:

выравнивание деревьев: https://research.utwente.nl/en/publications/normalized-alignment-of-dependency-trees-for-detecting-textual-en

нечеткое сравнение деревьев: https://www.researchgate.net/publication/236164307_Kto_vinovat_i_gde_sobaka_zaryta_Metod_validacii_otvetov_na_osnove_netocnogo_sravnenia_semanticeskih_grafov_v_voprosno-otvetnoj_sisteme_Who_is_to_blame_and_Where_the_dog_is_buried_Method_of_answers_val



### Использование

Пример исходных данных в файле `answers_udify.json`. На его основе строится дерево ответов:

```python
answer_graphs = []
for k in range(n_questions):
    answer_graphs_temp = []
    for i in range(5):
        graph = get_tree(all_vectors[k*5+i], all_parents[k*5+i])
        answer_graphs_temp.append(graph)
    answer_graphs.append(answer_graphs_temp)
```

К дереву ответов применяется `match_graphs_cosine` для вычисления косинусной меры:

```python
score_table_cosine = np.zeros((n_questions, 5))
for i in range(n_questions):
    for j in range(5):
        i_j_score = match_graphs_cosine(answer_graphs[i][0],answer_graphs[i][j])
        score_table_cosine[i][j] = i_j_score
```