# Would You Rent Out Your House or Apartment in Boston?

<p align="center">
  <img src="https://a0.muscache.com/im/pictures/20585270-53b4-4f23-907f-b371558c3e95.jpg?aki_policy=exp_xl" width="1000" title="via Airbnb">
  </p>
  
  ## Introduction
  
  It is tempting to explore ways which may help increase your income. One of these methods is becoming a host on platforms like Airbnb which allows you to
  rent out properties you're not using. Currently, [Airbnb](https://news.airbnb.com/fast-facts/) boasts more than 7 million listings worldwide, with the average number of 2 million people staying
  on Airbnb per night. But have you ever wondered how does in fare in Boston?
  
  The Boston dataset provided by Airbnb has 3585 total listings and contains various data, some of them that will prove valuable later on are:
  
  * Neighborhood
  * Property type
  * Number of beds
  * Number of bathrooms
  * Number of bedrooms
  * Listing price
  
  
  ### What is the distribution of listings for Boston's neighborhoods?
  
  First, we must sort out our data by neighboorhood and count the number of listings in each neighboorhood as follows:
  
  ```Python
  # loading the dataset
  boston = pd.read_csv('Boston\listings.csv')
  
  # Count the number of listings of each neighborhood
  boston_nb = boston.groupby('neighbourhood').count().sort_values('id',ascending= False)
  plot = boston_nb.plot(kind = 'bar', y= 'id',figsize=(12,8),legend = False)
  plt.title('Number of Listings in Each Neighborhood')
  plt.ylabel('Num of listings')
  plt.show();
  ```
  This will result in the following plot:
  
  <p align="center">
  <img src="https://github.com/Mnegh/Write-A-Data-Science-Blog-Post/blob/master/Illustrations/numlisting.PNG" width="650" title="Numer of Listings">
  </p>
  We observe that neighborhoods like Allston-Brighton, Jamaica Plain, and Back bay dominate the graph compared to neighborhoods like Harvard Square and Downtown.
  
  ### What is the average price of a listing for each neighborhood?
  We will now clean our price column by removing both the dollar sign and commas then calculate the mean of each neighborhood:
  
  ```Python
   # Obtain the average price of the listings of each neighborhood after data cleaning
  boston['price'] = boston['price'].replace('[\$,]', '', regex=True).astype(float)
  boston_nb = boston.groupby('neighbourhood')['price'].mean().sort_values(ascending=True)
  plot = boston_nb.plot(kind='barh',figsize=(14,10))
  plt.title('Average Price for Each Neighborhood')
  plt.xlabel('Average price ($)')
  plt.show();
  ```
  This will yield the following plot:
  <p align="center">
  <img src="https://github.com/Mnegh/Write-A-Data-Science-Blog-Post/blob/master/Illustrations/avgprice.png" width="850" title="Average price of a listing">
  </p>
  We first notice that Harvard Square actually has the highest average price out of all neighborhoods, this may be due to the demand compared to the number of listings in that area,
  as we have seen in the previous part that Harvard Square had the lowest number of listings.
  
  # What are the attributes that affect the price of a listing?
  We will construct a linear model with L1 regularization to pick out which features are the most important attributes that varies the price of a listing. The regularization technique
  is used as a feature selector, and will provide coefficients depending on how it affects a price.
  
  But first, we must user one-hot encoding for columns that are multi-categorical like neighborhood and property type and merge it with our feature, since the model can not handle multi-categorical data. We also fill out missing data with the mean of the feature.
   ```Python
  # dummy variable of each category
  neighbour = pd.get_dummies(boston['neighbourhood'])
  property_type = pd.get_dummies(boston['property_type'])
  features = ['beds','bathrooms','bedrooms','host_is_superhost']
  X = boston[features]
  X = pd.concat([X, property_type, neighbour],axis=1)
  # fill NaN values with the mean of the column
  X.fillna(X.mean(),inplace=True)
  ```
  We then fit our data to our machine learning model.
  
  ```Python
  # splitting the dataset
  X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.20, random_state=42)
    # initalizing pipeline
  pipeline = Pipeline([('scaler', StandardScaler()), ('lasso', linear_model.Lasso())])
  # setting up gridsearch
  param = {
      'lasso__alpha': [0,0.01,0.1,1]
  }
  pipecv = GridSearchCV(pipeline, param, n_jobs=-1, refit=True, scoring='r2')
  pipecv.fit(X_train, y_train)
  # predicting the test subset
  y_pred = pipecv.predict(X_test)
  ```
  To visualize how the features affect the model, we now take the coefficients of each feature and plot them in a graph.
  ```Python
  # setting up a df for visualization
  coefficients = pipecv.best_estimator_['lasso'].coef_
  coefs = pd.DataFrame(columns = X.columns)
  coefs.loc[0] = coefficients
  coefs.sort_values(by = 0, axis=1,inplace=True)
  fig = plt.figure(figsize=(16,10))
  plt.bar(coefs.columns, coefs.loc[0])
  plt.xticks(rotation=90)
  plt.title('Feature Coefficients')
  plt.ylabel('Coefficient')
  plt.show();
  ```
  This will result in the coefficient plot:
   <p align="center">
  <img src="https://github.com/Mnegh/Write-A-Data-Science-Blog-Post/blob/master/Illustrations/featcoef.PNG" width="850" title="Coefficients">
  </p>
  
  According to the model, the feature that increases the price the most is the number of bedrooms. While having a listing that is in Dorchester or Allston-Brighton will devalue the
  listing. This plot has a lot of information to take notes of.
  
  ## Conclusion
  In this article, we have looked into Boston's Airbnb dataset to observe the following:
  
   * Compared the number of listings in each neighborhood, where some neighborhood's numbers like **Allston-Brighton** dominate the others.
   * Observed the average price for each neighboorhood, which would help coming up with a price of your own.
   * The number of bedrooms is what postively affects the price the most, while some other attributes may decrease the value of the listing.
    
  After reading about the results of this venture, the question still stands:
  > Would You Rent Out Your House or Apartment in Boston?
  
  You can see more of my code in the following [Github](https://github.com/Mnegh/Write-A-Data-Science-Blog-Post). Thank you!
  
  
  
  
  
  
