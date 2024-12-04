<h1>Power Outages Analysis</h1>
** by Jordyn Ives and Dan Nguyen ** 
 <p>
      Have you ever been stuck in a blackout wondering how long it will last or why it happened in the first place? 
      What if energy companies could predict outages before they happen and take action to minimize their impact? 
      How can understanding the factors behind major outages improve the reliability of energy services and customer satisfaction?
    </p>
    <section>
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
    </section>
  </div>


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
  <p>This bar chart displays the number of power outages per region based on the dataset. The regions are represented along the x-axis, while the y-axis shows the total number of outages.</p>
  <iframe
    src="assets/power_outages_by_region.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>
</section>

<section>
  <h2>Power Outages by Year</h2>
  <p>This bar chart displays the number of power outages per year based on the dataset. The years are represented along the x-axis, while the y-axis shows the total number of outages.</p>
  <iframe
    src="assets/power_outages_by_year.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>
</section>

<section>
  <h2>Total Price Per Region</h2>
  <p>This box plot visualizes the total price for each climate region in the dataset. The x-axis represents the climate regions, while the y-axis shows the total price distribution for each region. The color-coded regions help in identifying trends and differences among them.</p>
  <iframe
    src="assets/price_by_region.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>
</section>

<section>
  <h2>Residential Price vs Outage Duration</h2>
  <p>This scatter plot visualizes the relationship between residential price and outage duration, categorized by climate. The x-axis represents residential price, while the y-axis shows the outage duration in minutes. The points are color-coded by climate category for easy differentiation.</p>
  <iframe
    src="assets/res_price_vs_outage_duration.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>
</section>

<section>
  <h2>Power Outages by Year and Climate Region</h2>
  <p>This heatmap displays the distribution of power outages across years and climate regions. The x-axis represents climate regions, while the y-axis represents years. The color intensity indicates the number of outages, with brighter colors showing higher outage counts.</p>
  <iframe
    src="assets/power_outages_by_year_and_region.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>
</section>

<section>
  <h2>Number of Outages Over the Years by Region</h2>
  <p>This line plot shows the trends in the number of power outages over the years across different climate regions. Each line represents a specific region, with the x-axis indicating the years and the y-axis showing the number of outages.</p>
  <iframe
    src="assets/number_of_outages_by_region.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>
</section>

<section>
  <h2>Correlation Matrix: Residential Price, Area Percentage, and Outage Duration</h2>
  <p>This heatmap visualizes the correlation between residential price, area percentage under certain conditions, and outage duration. The color intensity indicates the strength and direction of the correlation, where 1 represents a perfect positive correlation and -1 represents a perfect negative correlation.</p>
  <iframe
    src="assets/correlation_matrix.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>
</section>
