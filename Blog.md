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
  
  First, we must sort out our data by neighboorhood and count the number of listings in each neighboorhood, which will result in the following plot:
  
  <p align="center">
  <img src="https://github.com/Mnegh/Write-A-Data-Science-Blog-Post/blob/master/Illustrations/numlisting.PNG" width="650" title="Numer of Listings">
  </p>
  We observe that neighborhoods like Allston-Brighton, Jamaica Plain, and Back bay dominate the graph compared to neighborhoods like Harvard Square and Downtown.
  
  ### What is the average price of a listing for each neighborhood?
  We took the average price of neighborhood, then compared them which each other as follows:
  
  <p align="center">
  <img src="https://github.com/Mnegh/Write-A-Data-Science-Blog-Post/blob/master/Illustrations/avgprice.png" width="850" title="Average price of a listing">
  </p>
  We first notice that Harvard Square actually has the highest average price out of all neighborhoods, this may be due to the demand compared to the number of listings in that area,
  as we have seen in the previous part that Harvard Square had the lowest number of listings.
  
  # What are the attributes that affect the price of a listing?
  We will construct a linear model with L1 regularization to pick out which features are the most important attributes that varies the price of a listing. The regularization technique
  is used as a feature selector, and will provide coefficients depending on how it affects a price.
  
  But first, we must user one-hot encoding for columns that are multi-categorical like neighborhood and property type and merge it with our feature, since the model can not handle multi-categorical data. We also fill out missing data with the mean of the feature. We then fit our data to our machine learning model.
 
 To visualize how the features affect the model, we now take the coefficients of each feature and plot them in a graph.
 
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
  
  
  
  
  
  
