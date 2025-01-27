---

copyright:
  years: 2019
lastupdated: "2019-06-19"

subcollection: discovery-data

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:important: .important}
{:deprecated: .deprecated}
{:codeblock: .codeblock}
{:screen: .screen}
{:download: .download}
{:hide-dashboard: .hide-dashboard}
{:apikey: data-credential-placeholder='apikey'} 
{:url: data-credential-placeholder='url'}
{:curl: #curl .ph data-hd-programlang='curl'}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:ruby: .ph data-hd-programlang='ruby'}
{:swift: .ph data-hd-programlang='swift'}
{:go: .ph data-hd-programlang='go'}

# Improving result relevance with training
{: #train}

<!-- Help topic "Train" screen Discovery ICP4D  -->

The relevance of natural language query results can be improved in {{site.data.keyword.discovery-data_long}} with training. 
{: shortdesc}

Relevancy training is optional; if the results of your queries meet your needs, no further training is necessary. For information about use cases for relevancy training, see [Improve your natural language query results from Watson Discovery](https://developer.ibm.com/blogs/improving-your-natural-language-query-results-from-watson-discovery/){: external}.

To access the **Train** screen, [create a new collection (or open an existing one)](/docs/services/discovery-data?topic=discovery-data-collections#collections), and click the **Train** tab. 

In order to train Watson, you'll need to:

  -   Identify natural language queries that are representative of the queries your users would request.
  -   Rate the results of each query as `relevant` or `not relevant`.

Once Watson has enough training input, the information you have provided about which results are relevant or not relevant for each query will be used to learn about your collection. Watson does not memorize, it learns from the specific information about individual queries and applies the patterns it has detected to all new queries. It does this with machine learning Watson techniques that find signals in your content and questions. After training is applied, {{site.data.keyword.discovery-data_short}} then reorders the query results to display the most relevant results at the top. As you add more and more training data, {{site.data.keyword.discovery-data_short}} should become more accurate in the ordering of query results.

Natural language query results will return a `confidence` score. See [Confidence scores](/docs/services/discovery-data?topic=discovery-data-confidence#confidence) for details.

Adding a custom stopwords list can also improve the relevance of results for natural language queries. See [Defining stopwords](/docs/services/discovery-data?topic=discovery-data-stopwords#stopwords) for more information.
{: tip}


## Adding queries and rating results
{: #results}

Training consists of three parts:
-   A natural language query
-   The results of that query
-   The rating you apply to each result

To train a collection:

1.  On the **Train** screen, enter a natural language query in the **Enter a question to train** field. Do not include a question mark in your query. Click the **Add+** button.
1.  Click the **Rate results** button next to the query.
1.  After the results appear, select the **Relevant** or **Not relevant** button under each one. In order to train the collection efficiently, you should select an option for each result.
1.  When you are finished, click the **Back to queries** button.
1.  Continue adding queries and rating them. As you reach relevant training thresholds, the **Watson will learn which are the best results for your queries after you've rated enough** section will indicate your status by striking out the requirements as you meet them:
    - Add more queries
    - Rate more results
    - Add more variety to your ratings
    
    You will need to train a minimum of 50 unique queries to meet all three training thresholds. 
1.  You can continue adding queries and rating results after you have reached the threshold. You should enter all queries you think your users will ask.

Write your training queries the same way your users would ask them, for example: "IBM Watson in healthcare". Training queries should be written with some term overlap between the query and the desired answer; this will improve initial results when the natural language query is run.
{: tip}

You can delete individual training queries by clicking the **Delete** icon. If you would like to delete all of the training data in your collection at one time, you must do so using the API. See [Delete all training data](https://{DomainName}/discovery-data#delete-all-training-data){: external} for more information. 

## Testing and iterating on the relevancy of results
{: #testing-results}

After you have completed rating results, and Watson has applied the training, you should test to see if your query results have improved. To do so, run test natural language queries that are related (but not identical to) your training queries. Check to see if the results of your test queries have improved.

If you would like to further improve results after testing, you could:
-   Add more documents to your collection.
-   Add more training queries.
-   Rate more results, making sure to use both the `Relevant` and `Not relevant` ratings.

For additional training guidance, see [Relevancy training tips](/docs/services/discovery-data?topic=discovery-data-relevancy-tips#relevancy-tips).

## Confidence scores
{: #confidence}

{{site.data.keyword.discovery-data_short}} returns a `confidence` score for natural language queries of trained collections.

The `confidence` score can range from `0.0` to `1.0`. The higher the number, the more relevant the result.

The `confidence` score can be found in the query results, under the `result_metadata` for each document. This number is calculated based on how relevant the result is estimated to be, compared to the trained model. 

```JSON
{
  "matching_results": 4,
  "retrieval_details": {
    "document_retrieval_strategy": "trained"
  },
  "results": [
    {
	  "id": "eea16dfd5fe6139a25324e7481a32f89_13",
	  "result_metadata": {
	    "confidence": 0.08793
	  }
    }
  ]
}
```
{: codeblock}

The `document_retrieval_strategy` can be found under the `retrieval_details`. If you query a trained collection using the {{site.data.keyword.discoveryshort}} Query Language, or the trained model is temporarily unavailable, the `document_retrieval_strategy` will be `untrained`.

For more information on querying, see the [Query overview](/docs/services/discovery-data?topic=discovery-data-query-concepts#query-concepts).


