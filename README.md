# Titanic


So, Titanic. Let me say, that this is both great and tricky problem for beginners to learn along the way. I am a beginner ML engineer, thus can verify it. I have more than 160 saves of this work, totally changed the code 3 times, starting with basic features choice to adding more features and ending up at reducing features.

In this notebook I will show a bit of basic EDA with data visualizations, not going to deep into feature engineering (the main explanation of the whole process is below) and train the model with Random Forest Classifier. Let's dive into it!

1. PassengerID

Totally useless feature for modeling, it will be dropped by default.

2. Survived

That is our label, it will be used later for feature correlations.

3. Pclass

Categorical feature, that can be used in a model with OneHotEncoder or without it. I am writing about RF only now. In the previous versions I used this feature with get_dummies, thus had three more features in the feature list. It demonstrates class of passengers' cabin. Very important one to understand the difference of surviva rate. Well, that is too obvious.

4. Name

In the first versions I used classical beginner approach - getting titles from names to understand which men could survive. It turned out that it was helpful, basically, I got another Sex features that gets weights of dying men. The result was 79,9% accuracy (mean the public score). The second way was adding Surnames. Feature list after get_dummies was too big (over 900 positions), but it didn't help at all, the result was not better than 78% accuracy. in the last version feature "Titles" was used as well.

5. Sex

Obvious as well. "Women and children first". i will provide some code to see that men had less chances to survive, of course.

6. Age

The way of processing this feature is the next one: 1) using mean values to fill mising values 2) using median (that really improved the score) 3) adding bins (another improvement of the score) 4) creating a function to fill missing values using Sex and Pclass, using median values of these groups 5) creating a function to fill missing values using Title feature and median.

7. SibSp and Parch

The quantity of siblings, spouses, parents, children. Used it for creating features like FamilySize or IsAlone to get chances of survival depending on having a family or not. It happened that using all of them at once wasn't helpful, just a noise, no improvement to the model. I experimented with using only SibSp and Parch, adding FamilySize or IsAlone to them, it didn't help. The answer was revealed later - if you have a lot of highly correlated features between themselves, it's better to use one-two of them. In this version there is Family_Size feature.

8.Ticket and Fare

Using this feature wasn't very helpful in terms of get_dummies. A lot of feature, some of them had some values in feature_importance table (because there were big families who used one ticket for all members of the family, it also included not only family members, but also nannys or servants etc). It had more helpful use in pair with Fare feature, because it was revealed that big families in the third class have payed pretty big money that could be equal to second class prices. That is why I decided those were prices for the whole ticket and not a fare per person. Divided Fare/Ticket_Frequency and got Fare_per_one.

9. Cabin, Embarked

Spent a lot of time on dealing with them - creating Decks, grouping those decks in different combinations, but it seems, the prefiction power of these features is not very high.

About algorithms: used Random Forest, XGBoost, SVM, KNN. My personal favourite for this task is Random Forest, it did the best job, thus I lest only the code for using this particular algorithm.
