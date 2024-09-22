# Análise de Desempenho de Posts no Instagram

Este projeto coleta e analisa dados de perfis do Instagram para avaliar o desempenho das postagens, focando na relação entre as palavras usadas nas legendas e o número de likes recebidos. A análise inclui limpeza de dados, vetorização de palavras, cálculos de correlação e visualizações para identificar padrões no conteúdo que possam influenciar o engajamento.

## Instalação

Antes de executar o projeto, certifique-se de ter as seguintes bibliotecas Python instaladas:

```bash
pip install openpyxl
pip install -U scikit-learn
pip install matplotlib
pip install stop-words
pip install pandas
pip install instaloader
```

Esses pacotes são necessários para processar os dados, manipular textos, coletar informações do Instagram e gerar gráficos.

## Coleta de Dados

Os dados são coletados diretamente de um perfil específico no Instagram usando a biblioteca `Instaloader`. Essa ferramenta permite extrair informações como legendas, número de likes, comentários, usuários marcados e datas das postagens.

### Exemplo de coleta de dados:

```python
import instaloader

L = instaloader.Instaloader()

profile_name = 'nome_do_perfil'
profile = instaloader.Profile.from_username(L.context, profile_name)

posts_data = []

for post in profile.get_posts():
    post_data = {
        'Caption': post.caption,
        'Likes': post.likes,
        'CommentCount': post.comments,
        'Date': post.date_utc,
        'Is Video': post.is_video,
        'URL': 'https://www.instagram.com/p/' + post.shortcode
    }
    posts_data.append(post_data)
```

Após a coleta, os dados são exportados para um arquivo CSV para evitar a necessidade de repetir a coleta em execuções futuras.

## Processamento e Limpeza de Dados

Para preparar os dados para a análise, várias etapas são realizadas, incluindo:

1. **Filtragem de Posts:** Excluímos posts com likes desativados (3 likes) e anúncios (mais de 600 likes).
2. **Limpeza das Legendas:** Removemos hashtags, menções e caracteres especiais das legendas.
3. **Vetorização de Palavras:** Transformamos as legendas limpas em uma matriz de contagem de palavras usando o `CountVectorizer`, ignorando palavras comuns.

### Exemplo de filtragem e limpeza:

```python
df = df[(df['Likes'] > 3) & (df['Likes'] <= 600)]
df['Cleaned_Caption'] = df['Caption'].apply(replace_hashtags_and_mentions)
```

## Análise de Dados

Utilizamos duas abordagens principais para analisar a relação entre as palavras nas legendas e o número de likes:

1. **Contagem de Palavras (CountVectorizer):** Contabiliza o número de vezes que cada palavra aparece e calcula a correlação com os likes.
2. **TF-IDF (Term Frequency - Inverse Document Frequency):** Calcula a importância de cada palavra em relação às demais legendas e analisa sua correlação com os likes.

### Exemplo de cálculo de correlação:

```python
correlation = X_df.corr()['Likes'].sort_values(ascending=False)
```

## Visualização

As correlações são apresentadas em gráficos de barras horizontais, destacando as palavras mais correlacionadas com o número de likes.

### Exemplo de visualização:

```python
plt.figure(figsize=(7, 20))
correlation[1:51].sort_values(ascending=True).plot(kind='barh')
plt.title('Correlação entre palavras utilizadas e número de likes')
plt.xlabel('Correlação com likes')
plt.ylabel('Palavras')
plt.show()
```

## Conclusão

Este projeto tem como objetivo entender como o conteúdo textual das legendas influencia o engajamento em posts no Instagram. A análise das correlações entre palavras e o número de likes pode ajudar a identificar padrões úteis para otimizar o desempenho das postagens.

## Licença

Este projeto é distribuído sob a licença MIT. Para mais informações, consulte o arquivo [LICENSE](LICENSE).
