# faKy: Feature Extraction Library for Fake News Analysis
faKy is an advanced feature extraction library explicitly designed for analyzing and detecting fake news. It provides a comprehensive set of functions to compute various linguistic features essential for identifying fake news articles. With faKy, you can calculate readability scores, and information complexity, perform sentiment analysis using VADER, extract named entities, and apply part-of-speech tags. Additionally, faKy offers a Dunn test function for testing the significance between multiple independent variables.

Our goal with faKy is to contribute to developing more sophisticated and interpretable machine learning models and deepen our understanding of the underlying linguistic features that define fake news.

## Installation
To install the faKy library, you can use the pip package manager. The faKy library is built on top of the spaCy library, and it includes the necessary dependencies, including the standard en_core_web_md model.

Run the following command to install the faKy library:

```shell
pip install faKy==2.1.0
```

This command will download the faKy library and its dependencies, including the spaCy library, the en_core_web_md model, and NLTK. After the installation, you can use the faKy library in your Python projects.
## Getting Started with faKy
faKy is a powerful library for computing text-based features. It provides functions to extract various features from text objects within a data frame.
```python
from faKy import process_text_readability, process_text_complexity
```
Next, apply the process_text_readability function to your data frame:
```python
dummy_df['readability'] = dummy_df['text-object'].apply(process_text_readability)
```

This will compute the readability score for each text object in the 'object' column and store the results in a new 'readability' column of the data frame.

You can also similarly apply other functions, such as process_text_complexity, to compute additional features.

For functions that return multiple values, you can use the lambda function to unpack the results into separate columns, as shown in the example below:
```python
from faKy import process_text_vader
df[['vader_neg', 'vader_neu', 'vader_pos', 'vader_compound']] = df['object'].apply(
    lambda x: pd.Series(process_text_vader(x))
)
```
Make sure to import the necessary input vector functions from faKy if you plan to use them for creating input vectors based on NER labels or POS tags, as shown in the example below:
```python
from faKy import count_named_entities, count_ner_labels, create_input_vector_NER, ner_labels
df['tot_ner_count'] = df['object'].apply(count_named_entities)
df['ner_counts'] = df['object'].apply(count_ner_labels)
df['input_vector_ner'] = df['ner_counts'].apply(create_input_vector_NER)
```
Similar to the Vader sentiment scores, you can compute the different NER counts for the data frame as follows:
```python
for tag in ner_labels:
    col_name = f'NER_{tag}'
    df[col_name] = df['input_vector_ner'].apply(
                                    lambda x: x[ner_labels.index(tag)] if tag in ner_labels else 0)
```

After applying these functions to your data frame, the features will be extracted and added as a new column, as shown in the example below:

![Alt Text](./faKy_101.png)

# faKy functionality
Here is a summary of the available functions in faKy:
| Function Name             | Usage                                                                                                                                                            |
|---------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| process_text_readability  | Takes a text string as input, processes it with spaCy's NLP pipeline, and computes the Flesch-Kincaid Reading Ease score. Returns the score.                       |
| process_text_complexity   | Takes a text string as input, processes it with spaCy's custom NLP pipeline, and computes the compressed size. Returns the compressed size of the string in bits. |
| VADER_score               | Takes a text input and calculates the sentiment scores using the VADER sentiment analysis tool. Returns a dictionary of sentiment scores.                          |
| process_text_vader        | Takes a text input, applies the VADER sentiment analysis model, and returns the negative, neutral, positive, and compound sentiment scores as separate variables.  |
| count_named_entities      | Takes a text input, identifies named entities using spaCy, and returns the count of named entities in the text.                                                 |
| count_ner_labels          | Takes a text input, identifies named entities using spaCy, and returns a dictionary of named entity label counts.                                               |
| create_input_vector_NER   | Takes a dictionary of named entity recognition (NER) label counts and creates an input vector with the count for each NER label. Returns the input vector.         |
| count_pos                 | Counts the number of parts of speech (POS) in a given text. Returns a dictionary with the count of each POS.                                                    |
| create_input_vector_pos   | Takes a dictionary of POS tag counts and creates an input vector of zeros. Returns the input vector.                                                             |
| values_by_label           | Takes a DataFrame, a feature, a list of labels, and a label column name. Returns a list of lists containing the values of the feature for each label.             |
| dunn_table                | Takes a DataFrame of Dunn's test results and creates a new DataFrame with pairwise comparisons between groups. Returns the new DataFrame.                           |
    