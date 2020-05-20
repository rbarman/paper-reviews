## Personalized, Health-Aware Recipe Recommendation:An Ensemble Topic Modeling Based Approach**

    @article{khan2019personalized,
      title={Personalized, Health-Aware Recipe Recommendation: An Ensemble Topic Modeling Based Approach},
      author={Khan, Mansura A and Rushe, Ellen and Smyth, Barry and Coyle, David},
      journal={arXiv preprint arXiv:1908.00148},
      year={2019}
    }

### Domain Overview
- Food recommendation system (FRS) to recommend recipes. 
- FRS must acknowledge factors such as demographics, health awareness, cost, cooking time, ingredient availability. 
- In the future, FRS could create personalized healthy diets that users enjoy. This could help with obesity and malnutrition problem. 
- FRS should have high `coverage` - able to make predictions for many user/food pairs
- FRS should understand `implicit user behaviors.` For example, vegetarians and vegans eat vegetables, but eggs should not be recommended to vegans. 

### Ensemble Topic Modeling
- Want to make recommendations from recipes (text corpus).
- Apply `topic modeling` to condense recipe text to a smaller set of features
  - Models have `n` `topics`. Topics can be interpreted as broader themes in the domain. For example recipe topics include ingredients, cooking process, equipment, cuisine,  nutrition, etc
  - Each `topic` has a set of closely associated word. For example the nutrition topic can have features of 'high-calcium', 'low-cholesterol', etc
- `non negative matrix factorization` (NMF) for topic modeling
  - Creates topic-term weight matrix. Columns are topics, rows, are terms, and matrix values are level of association between topic and term. 
  - Matrix filtered to top 30 topics and top 15 terms within each topic. 
  - 288 unique terms -> `food features`. Each feature has its `feature score` which is the cumulative sum of its weight over all topics. 

Ultimately each recipe is transformed to 288 features
Recipe 1 =>
| f1|f2|f3|...|f288|
|---|---|---|---|---|
| .3|.42|0|...|.42|
Where the numerical value is the TF-IDF for the feature in the recipe. 

### Data Collection
- Scraped recipes from online sites. Each recipe is a plain text document.
- Ask users to rate rate food features from -5 to 5.

### Recommendation algos

`Food feature based`
- assign preference score to a recipe for a user based on the cumulative sum of the user's ratings of all features present in the recipe.
- This approach assumes that each food feature has an equal impact on preferences

`Weighted Food feature based`
- similar to above. Scales the user's feature ratings by the `feature score`. 

`Food feature based Collaborative filtering`
- use collaborative filtering to approximate user's score on food features they did not rate. 
- With approximated user ratings for more food features, make predictions using cumulative sum in algorithm 1

### Experiment 
- 48 Participants view images representing 288 food features and rate atleast 20 features
- Participants are shown list of recommended recipes from each recommendation algorithm. Participants then rate each recipe from 0 to 5. 

### Results
- For each participant, calculate average of participant ratings for each algorithm recommendations
- Ensemble topic modeling approaches performed significantly better than generic collaborative filtering. 
- Feature based collaborative filtering had 100% coverage. However it was not able to capture the implicit user food practices - one possible explanation is the effect of neighbor preferences.

---
### Takeaways
- Possible to apply similar topic modeling to music data to make stronger music recommendations?
  - ie ignore lyrics / 'text' content and create topics on the sound content.
- Incorporate user specific data when making recommendations? Difficult for a recommender system to capture every single implicit user value. 
- How does the topic model get updated in production? 
  - Scraping recipes from new sites can lead to new topics and/or new top ranked terms per topics
- Is there a reason why participants were shown the 288 features ordered by feature weight when asked to rate each feature?
  - Maybe participants tend to avoid / not read survey options that are near the bottom and only focus their attention on the top n options. The top features have highest feature weights and thus user ratings are more impactful for the recommendation algos?

