<h1>Power Outages Analysis</h1>
by <em>Jordyn Ives</em> and <em>Dan Nguyen</em>

 <p>
      Have you ever been stuck in a power outage wondering how long it will last or why it happened in the first place? 
      What if energy companies could predict outages before they happen and take action to minimize their impact? 
      How can understanding the factors behind major outages improve the reliability of energy services and customer satisfaction?
  </p>
  <h2>Introduction</h2>
  <p>
        This analysis makes use of the Power Outage dataset, which tracks power outages in the US between the years 2000-2016. Though at first glance this dataset seems rather technical, it is important for understanding power outage trends across the US and the qualities that have remained constant and changed over the years. There are 1,533 rows, each corresponding to a different power outage. There are 55 columns in this dataset, most of which are categorical and correspond to specific characteristics relating to power outages.
    </p>
   <p>
        The purpose of this analysis is to explore trends in power outage severity over time, centered around the analytical question:
   </p>
   <blockquote>
        <strong>What are the characteristics of high-impact power outages? How have these factors and characteristics changed over time, and based on this, how can energy companies best allocate their resources to helping customers?</strong>
   </blockquote>
   <p>
        The column <code>OUTAGE.DURATION</code> is important here because it measures how severe a power outage is. The longer the power outage, the more impactful it is to residents, and the more revenue is lost for the company. It is interesting to explore the factors that contribute to longer power outages and whether it is possible to predict the length of a power outage given other data. <code>OUTAGE.DURATION</code> is a quantitative continuous variable that is measured in minutes, with a minimum of 1 minute and a mean of 108,653 minutes.
   </p>
   <p>
        We also explore <code>OUTAGE.DURATION</code>’s change over time using the <code>YEAR</code> column. With the acceleration of climate change in recent years, simple graphs showing the number of outages per year and the average severity per year are useful. Additionally, understanding how a region comes into play adds another layer of complexity: the categorical <code>CLIMATE.REGION</code> column breaks down outages into one of 12 US regions. For our model, we used a few other quantitative variables, but our main analytical question and exploration focused on these variables and analyzing trends over time.
 </p>

  <h2>Data Cleaning</h2>
  <ol>
    <li>
      <strong>Removing Unnecessary Columns:</strong>
      <p>We removed the first column from the dataset, labeled “variables.” This column was the result of misreads in the original Excel file and did not provide any meaningful information for analysis.</p>
    </li>
    <li>
      <strong>Handling Missing Values in Outage Start Date and Time:</strong>
      <p>Missing values in the <code>OUTAGE.START.DATE</code> and <code>OUTAGE.START.TIME</code> columns were dropped. Since this analysis makes use of time series data, missing dates are problematic and can affect the accuracy of our results.</p>
    </li>
    <li>
      <strong>Creating Datetime Columns:</strong>
      <p>We combined the outage start date and time into a new column, <code>OUTAGE.START</code>, as a datetime object. Similarly, we combined the outage restoration date and time into a new column, <code>OUTAGE.RESTORATION</code>, ensuring that the time series data was fully captured and ready for analysis.</p>
    </li>
    <li>
      <strong>Extracting Relevant Information:</strong>
      <p>Once we had the datetime columns, we extracted useful information such as the year, day of the week, and month for each outage. This information is valuable for identifying patterns and trends in power outages over time.</p>
    </li>
    <li>
      <strong>Handling Missing Values in Model Features:</strong>
      <p>For the features used in our model, we decided to drop all rows with <code>NA</code> values. The dataset was large enough that this did not result in a significant reduction in data. While imputing missing values can be useful, the small number of missing values in this dataset meant that imputing would not significantly improve the analysis. Removing these rows ensured compatibility with Scikit-learn and streamlined the modeling process.</p>
    </li>
  </ol>
  <h3>Why These Steps Are Important</h3>
  <p>Dropping irrelevant columns and rows with missing values simplifies the dataset and ensures compatibility with modeling tools. Extracting date-related features helps uncover temporal trends in power outages, which are critical for predictive analysis.</p>

<section>
  <h2>Power Outages by Region</h2>
  <p>
    This bar chart shows the number of power outages across different regions in the United States from 2000 to 2016. To generate this graph, we used the <code>CLIMATE.REGION</code>, which categorizes each outage into one of 12 US climate regions. By counting the occurrences of outages for each region (<code>value_counts()</code>), we created a summary of outage frequencies.
  </p>
  <p>
    The x-axis represents the climate regions, and the y-axis represents the total number of outages. Each bar shows the number of recorded outages in a specific region, with the Northeast experiencing the highest number of outages, followed by the South and West.
  </p>
  <p>
    This graph helps us identify regions with the most frequent outages. In addition, it also provides context for analyzing how different climate conditions may influence outage patterns, which closely relates to our goal of understanding high-impact outages and their characteristics.
  </p>
  <iframe
    src="assets/power_outages_by_region.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>
</section>

<section>
  <h2>Power Outages by Year</h2>
  <p>
    This bar chart shows the number of power outages recorded each year in the United States from 2000 to 2012. To generate this graph, we used the <code>OUTAGE.START.YEAR</code> column from the dataset, which specifies the year when each outage began. By counting the occurrences of outages for each year (<code>value_counts()</code>), we created a summary of annual outage frequencies and plotted them on the chart.
  </p>
  <p>
    The x-axis represents the years and the y-axis represents the total number of outages in each year. Each bar indicates the frequency of outages, with 2012 showing the highest number of recorded outages in this dataset.
  </p>
  <p>
    This graph is important because it identifies the temporal patterns in power outages. Specifically, it shows how outage occurrences have varied across the years, which indicates whether certain years experienced more extreme weather or other contributing factors. This graph helps us understand the context of outages, identify spikes in occurrence, and predict future patterns.
  </p>
  <iframe
    src="assets/power_outages_by_year.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>
</section>

<section>
  <h2>Total Price Per Region</h2>
  <p>
    This box plot shows the distribution of total electricity prices across different climate regions in the United States. To generate this graph, we used the <code>TOTAL.PRICE</code> column, which represents the total price of electricity during outages, grouped by the <code>CLIMATE.REGION</code> column. Each box-and-whisker plot represents the price distribution within a specific climate region.
  </p>
  <p>
    The x-axis displays the climate regions, and the y-axis shows the total electricity price. The box represents the interquartile range (IQR), the line within the box indicates the median price, and the whiskers extend to the minimum and maximum prices, excluding outliers. Outliers are displayed as individual points.
  </p>
  <p>
    This graph helps identify regional differences in electricity prices during outages. For example, the Northeast exhibits the highest variability and median price, suggesting it faces greater economic impacts from power outages. By contrast, regions like the Southeast and Northwest have lower median prices and less variability. These insights are crucial for understanding the economic dimensions of power outages and identifying regions where residents may face higher costs.
  </p>
  <iframe
    src="assets/price_by_region.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>
</section>

<section>
  <h2>Residential Price vs Outage Duration</h2>
  <p>
    This scatter plot shows the relationship between residential electricity price (<code>RES.PRICE</code>) and outage duration (<code>OUTAGE.DURATION</code>), with points color-coded by climate category. To generate this graph, we plotted <code>RES.PRICE</code> on the x-axis and <code>OUTAGE.DURATION</code> on the y-axis, while grouping data by the <code>CLIMATE.CATEGORY</code> column to distinguish between different climate conditions (normal, cold, warm).
  </p>
  <p>
    The x-axis represents the residential price, and the y-axis shows the outage duration in minutes. Each point corresponds to a specific power outage, and its color indicates the associated climate category.
  </p>
  <p>
    This graph helps identify whether there is a significant correlation between residential price and outage duration, as well as how different climate categories affect this relationship. While most outages are clustered at shorter durations and lower prices, a few outliers represent prolonged outages at varying price levels.
  </p>
  <iframe
    src="assets/res_price_vs_outage_duration.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>
</section>

<section>
  <h2>Power Outages by Year and Climate Region</h2>
  <p>
    The provided aggregation is a cross-tabulation of the dataset's YEAR and CLIMATE.REGION columns. It summarizes the number of occurrences for each climate region across different years, with rows representing years and columns representing climate regions. This allows for easy identification of trends or patterns in the data across regions over time.
  </p>
  <iframe
    src="assets/yearly_distribution_climate_regions.png"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>
</section>

<section>
  <h2>Number of Outages Over the Years by Region</h2>
  <p>
    This line graph shows the yearly number of power outages across different climate regions in the United States. To create this graph, we used a pivot table that aggregates outage counts by year (<code>YEAR</code>) and climate region (<code>CLIMATE.REGION</code>).
  </p>
  <p>
    The x-axis represents the years, while the y-axis shows the number of outages for each region. Each line corresponds to a specific climate region, and the legend identifies the regions by color.
  </p>
  <p>
    The graph reveals trends in outage occurrences over time, highlighting spikes and declines across various regions. For example, the Northeast shows a peak in 2012, while other regions like the Central and Southeast remain relatively consistent over the years. This visualization provides insights into temporal patterns and helps identify regions prone to frequent outages during specific periods.
  </p>
  <iframe
    src="assets/number_of_outages_by_region.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>
</section>

<section>
  <h2>Correlation Matrix: Residential Price, Area Percentage, and Outage Duration</h2>
  <p>
    This heatmap shows the relationships between three key variables: Residential Price, Area Percentage, and Outage Duration. The graph uses a color gradient from red to blue, where red indicates a strong positive correlation (closer to 1), blue represents a strong negative correlation (closer to -1), and lighter shades indicate weaker correlations closer to zero.
  </p>
  <p>
    <strong>Residential Price</strong> refers to the cost of electricity for residential customers. <strong>Area Percentage</strong> denotes the percentage of the area affected during an outage, and <strong>Outage Duration</strong> measures the total time (in minutes) of an outage. The matrix reveals the following relationships:
  </p>
  <ul>
    <li>
      Residential Price has a weak positive correlation with Area Percentage (0.15), indicating that higher residential prices may slightly correlate with a larger area affected during outages.
    </li>
    <li>
      There is almost no correlation between Residential Price and Outage Duration (0.00), suggesting that the cost of electricity is not related to how long outages last.
    </li>
    <li>
      Area Percentage and Outage Duration show a very weak negative correlation (-0.03), implying that as the area affected increases, the duration of the outage slightly decreases, though this relationship is negligible.
    </li>
  </ul>
  <p>
    This graph helps us understand whether these variables are interrelated. The weak correlations suggest that Residential Price, Area Percentage, and Outage Duration are largely independent factors. This insight is crucial for identifying other potential drivers of outage impact and understanding how these variables might contribute to power outage trends.
  </p>
  <iframe
    src="assets/correlation_matrix.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>
</section>

<div>
<h2>Prediction Problem</h2>
    <p>
        Our prediction problem is can we predict the length of a power outage given factors like region, state and cause category. 
        Since <code>OUTAGE.DURATION</code> is a quantitative variable, this prediction problem is best explored using linear regression. 
        At the time of the outage, we know the state and region of the outage. We also know the cause category, though there is the 
        slight chance more information can be uncovered as time goes on. We chose this response variable because it has valuable 
        implications for power companies - if we can predict the outage duration, it can help customers understand what is on the 
        horizon and deploy adequate resources.
    </p>
    <p>
        To evaluate this initial model, and other iterations, we are using Mean Squared Error (MSE). MSE is less robust to outliers, 
        but in this case, an abnormally large difference between the predicted and actual power outage duration can have drastic results 
        for customers and the power company. If the model doesn’t take into account the longest and smallest outages, predictions may be misleading. 
        Thus, it is important that this model is as accurate as possible, especially in a stressful outage during a storm or disaster.
    </p>
    <p>
        Initially, we are evaluating the model's MSE on the test vs training set to determine if it is underfit or overfit, since an 
        abnormally low MSE on the training set than a much higher one on the test set represents overfitting. And from there, depending 
        on the degree of overfitting or underfitting, we will take steps to regularize our model and adapt it.
    </p>
 </div>

<section>
  <h2>Initial Model</h2>
  <p>
    This initial model uses two categorical nominal variables, <code>CLIMATE.CATEGORY</code> and <code>CAUSE.CATEGORY</code>. These variables are categorical because they represent belonging to different groups, and are nominal because there is no inherent order. 
    We did some initial exploration using a heat map for quantitative variables, and plots showing the breakdown of <code>OUTAGE.DURATION</code> across different climate regions and cause categories. The latter of these relationships was more intriguing initially, so we decided to head in that direction.
  </p>
  <p>
    Since these variables are categorical, we used one-hot encoding in our initial pipeline. This was a simple pipeline, as we initially performed no other transformations to our model. 
    At first glance, there is a large difference between the train Mean Squared Error (MSE) of <strong>20,589,306.28</strong> and the test MSE of <strong>58,370,110.28</strong>. 
    The test MSE being double that of the training MSE indicates that this model is overfit to the training data and should be improved.
  </p>
  <p>
    It is also useful to add more information to this model. While state and <code>CAUSE.CATEGORY</code> are useful predictors, incorporating new information like <code>AREAPCT_UC</code>, 
    the percent of the outage’s state that is urban, can reveal interesting patterns. For example, it can help identify if outages in more rural areas last longer.
  </p>
  <h3>Supporting Visualizations</h3>
  <p>
    The following graphs illustrate patterns observed in our data, which supported our initial model development:
  </p>
  <h4>1. Boxplot of Outage Duration by Climate Category</h4>
  <p>
    This boxplot visualizes the distribution of outage durations across different climate categories. It highlights significant variations in outage durations within different categories, such as "normal," "cold," and "warm."
  </p>
  <iframe
    src="assets/climate_category_boxplot.png"
    width="600"
    height="400"
    frameborder="0"
  ></iframe>
  <h4>2. Bar Plot of Average Outage Duration by Cause Category</h4>
  <p>
    This bar plot presents the average outage duration for each cause category. It highlights substantial differences in average durations across categories like "fuel supply emergencies" and "severe weather," which are key drivers in our model.
  </p>
  <iframe
    src="assets/cause_category_boxplot.png"
    width="600"
    height="400"
    frameborder="0"
  ></iframe>
</section>
<section>
  <h2>Final Model</h2>
  <p>
    To improve our final model, we decided to add one quantitative variable. Though randomly adding variables is not the most efficient way to produce a final model, the variable <code>RES.PRICE</code> had a high correlation (0.93) with <code>OUTAGE.DURATION</code>. Since this variable represents the monthly electric price for the area where that outage occurs, it adds a new layer of data and relationships not reflected in our other two variables. It is also interesting to explore if power companies are quicker to resolve areas that have higher vs lower rates. A graph of <code>OUTAGE.DURATION</code> vs <code>RES.PRICE</code> demonstrates that strong correlation. To best represent the relationship, we explored adding polynomial features to <code>RES.PRICE</code>, using cross-validation to find the best polynomial features to add. We also decided to apply a <code>StandardScaler</code> transformation.
  </p>
  <p>
    Another variable we decided to add was <code>AREAPCT_UC</code>, which is a quantitative variable that describes the percent of the outage’s state that is urban. The reason this variable is important is because it accounts for the difference in response times for rural vs urban areas. And if a state has a higher percentage of rural areas, perhaps the response time will differ. For this variable, we decided to apply a <code>StandardScaler</code> transformation.
  </p>
  <p>
    The reason we decided to use <code>StandardScaler</code> is for our final model, which also uses <code>LASSO</code> regression. LASSO is important when we are trying to identify the best parameters in the model, with the goal of generalizing unseen data. When using LASSO, it is important to use the <code>StandardScaler</code> transformer to ensure that all coefficients are penalized equally.
  </p>
  <pre>
    <code>
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import Lasso
from sklearn.preprocessing import FunctionTransformer, PolynomialFeatures, OneHotEncoder
from sklearn.pipeline import Pipeline, make_pipeline
from sklearn.compose import make_column_transformer
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import GridSearchCV
from sklearn.linear_model import Ridge

X = df.loc[:, df.columns != 'OUTAGE.DURATION']
y = df['OUTAGE.DURATION']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25, random_state=42)

categorical_transforms = make_pipeline(OneHotEncoder
                                       (drop='first', handle_unknown='ignore'), SimpleImputer(strategy = 'most_frequent'))
numerical_cols = make_pipeline(SimpleImputer(fill_value=0), StandardScaler(),PolynomialFeatures(2,include_bias=False))

preprocessing = make_column_transformer((categorical_transforms, ['CLIMATE.CATEGORY', 'CAUSE.CATEGORY']),
                                        (numerical_cols, list(X_train.select_dtypes('number').columns)))

hyperparams = {
'ridge__alpha' : 2.0 ** np.arange(-5, 20)}
#'columntransformernumerical_cols__pipeline__polynomialfeatures__degree': range(1, 6)}

searcher = GridSearchCV(
estimator = make_pipeline(preprocessing, Ridge()),
param_grid=hyperparams,
cv=5, # k = 5.
scoring='neg_mean_squared_error'
)

searcher.fit(X_train, y_train)

#model = make_pipeline(preprocessing, Lasso())
#model.fit(X_train, y_train)

y_pred_train = searcher.predict(X_train)
y_pred_test = searcher.predict(X_test)

train_mse = mean_squared_error(y_train, y_pred_train)
test_mse = mean_squared_error(y_test, y_pred_test)

print(f"Train MSE: {train_mse}")
print(f"Test MSE: {test_mse}")

# Output:
# Train MSE: 17413483.47043481
# Test MSE: 60549018.451451585
   </code>
  </pre>
</section>
