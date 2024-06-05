Data Mining Project
Working with and finding patterns and information within Eurovision data from the years 2016–2023
Amina Hukić


I. Introduction

The Eurovision Song Contest, often known simply as Eurovision, is an international song competition organised annually by the European Broadcasting Union. Each participating country submits an original song to be performed live, with competing countries then casting votes for the other countries' songs to determine a winner.

The contest was inspired by and based on Italy's national Sanremo Music Festival. Eurovision has been held annually since 1956 (apart from 2020 due to the COVID-19 pandemic), making it the longest-running international music competition on television and one of the world's longest-running television programmes. As of 2023, 52 countries have participated in the contest at least once.

The first contest, held in 1956, had seven participating countries. Each country was represented by two songs of their choice, making this the only Eurovision contest with more than one entry per participant. The voting was held secretly, and the winner was announced at the end on stage. 

The tradition of a scoreboard showing the current points was introduced in 1957, and the tradition of the previous contest winner holding the next one was introduced in 1958.

Nowadays, the format of the contest is different due to the larger amount of contestants, The event is held over several days, usually three days with two semi-finals and one final. In the two semi-finals, all countries compete, with the ten highest-scoring countries of each semi-final being qualified for the grand final.

There is an exception, with the hosting / previously winning country and the five most financially contributing countries, known as the “big five” (France, Germany, Italy, Spain and the United Kingdom) being automatically qualified for the finale. They usually do not participate in the semi-finals due to that, but sometimes they still perform in them without being voted for.

Throughout the show, there are always several special musical performances. Often they are previous winners and important well-known artists of the hosting country. The opening performing act of each show is used to set up a unique theme of the Eurovision Contest, which will dictate the looks and organisation of the show.

The voting system used to determine the results works based on positional voting. Each country awards 1-8, 10 and 12 points to their ten favourite songs, given by the country's chosen general public or assembled jury. The most popular song receives 12 points. In the semi-finals, each country is awarded one set of points by public voting, while in the final each country is awarded two sets of points. One is given by a five-person jury panel of musical experts chosen by the voting country, and the other is awarded by the viewers. Since 2023, non-participating countries can also vote via an online platform, and those points are considered an “extra country” and given to the overall public favourite. 

Should there be a tie between two or more countries, a procedure is employed to determine the final placings. As of 2016, a combined national televoting and jury result is calculated for each country, and the country which has obtained more points from the public voting following this calculation is deemed to have placed higher.

The sources used were mostly Wikipedia articles [1].


II. Background research

We can find several already done projects on the same topic, looking into patterns and information. One of them is the following project [2]; made by Artem Mateush, Oleksandr Cherednychenko, Gerson Noboa and Vasiliy Skydanienko, for the Data Mining course at the University of Tartu.

It goes into detail about voting biases between countries, finding that countries in the Balkans (“ex-yu” countries) have a bias for each other, which due to their political affiliation can be perfectly explained.

Nordic, Baltic, and Iberian regions also cluster together. There are also certain country pairs (ex. Cyprus and Greece) which have a strong voting bias for each other.

Another interesting article found is one made by The Alan Turing Institute on who is most likely to win Eurovision 2023 [3].

They developed three different models; a simple baseline model, a Bayesian regression model, and a machine learning model. All three models shared the same countries as their top three predictions – Italy, Ukraine and Sweden. The baseline and the machine learning model both predicted that Italy would come first, Ukraine second and Sweden third. However, the Bayesian model predicted that Ukraine would come first, Sweden second and Italy third.

It is interesting to note that the actual winner in Eurovision 2023 was Sweden, with Finland a very close second (and overall public vote winner), Israel third, Italy fourth, and Ukraine sixth. The models therefore came extremely close to correctly guessing all three first places, but unpredictable factors (like public vote, an unusual genre) affected the actual results, thus showing the limitation of prediction models.

So, we can see that several models already exist, mostly focusing on trying to guess the winner of Eurovision and to find biases in voting patterns. Both of those will be taken into account in this project.


III. Methodology

A. Finding the appropriate dataset
We will be using a Eurovision dataset from Kaggle [4]. The dataset contains detailed information on every song that entered the Eurovision contest since 2016. There is data about the country, the contest, the competing song, and final scores which is a folder made up of several smaller datasets on each year’s different voting type.

Several parts of the dataset will not be used as it’s a huge amount of information and makes organising and processing data extremely difficult. 


B. Data cleaning and preprocessing
To explain the process of cleaning the data, we first must look at how the data is given to us.

There are files labelled “contest_data”, “country_data”, and “song_data”.
“Contest_data” holds information about each year's contest, where it was held, when it was held, how many people watched it, etc. This data was discarded as it was difficult to work with and it does not largely impact the final results.

“Country_data” holds information about each participating country, like geographical region, average temperature, capital city, etc. This data was also discarded for the same reason as the previous data.

There very likely could have been interesting patterns that would emerge from those two data sets. An example would be how jury voting is often largely impacted by whether or not two countries neighbour each other, whether or not they have had a good relationship historically and/or if they are building a good relationship in the present. In Eurovision, there is a disparity between Western and Eastern European countries. There have always been politics in play when it comes to Eurovision voting, but in this project, we will focus purely on voting points without any further context. Such can always be added afterwards to make sense of any emerging point patterns.

(A deeply interesting source further analysing the political impact on Eurovision is this video by Verity Ritchie [5])

The other data given is Final votes; there are jury votes, televotes and polls. Since polls are in no way affecting the final results, this data will also be ignored. 

That leaves “song_data”, which holds information about a song performance and will be the main focus, and then seven data files each for jury votes and televotes; one for every contest year from 2016-2023 (2020 is excluded due to the contest being cancelled because of Covid).

The data was turned into three datasets: jury votes, televotes and song data. After cleaning and preprocessing each dataset, they were combined and then cleaned one more time to fix any mistakes.

The process was fairly straightforward, the files were read with the “pd.read_cvs()” function, the encoding used was “latin1” as that one worked. The columns were printed and then checked for duplicates and percentage of empty data points. Duplicates were removed, but a trend emerged in empty data points. Certain columns which should not have been empty, had 50% of their data missing. Specifically, televoting and jury data were an anomaly. But, after looking into those columns it can be concluded that the large percentage of missing data is from the fact that several countries did not get to vote due to not being in the finals or not being in Eurovision at all that year. 

After a closer look, countries that were voting less than 75% of the time were fully dropped. For the others, every empty data point was replaced with a 0.0, the reason being that that fills up empty data points but does not affect or change the final points. So, we can safely include that data and work with it. Some empty columns from “song_data” were also dropped, and all three datasets were combined.


C. Performing data analysis
We looked through the data and tested it with a few simple analyses. Some information searched for was: Amount of wins per country, Frequency of winner per country, Most points won by winners, Male to Female ratio of performance, Plotting votes over time, Plotting points over time, etc.

Some immediately noticeable patterns were:
Due to a relatively small dataset of only 12 years (Eurovision happened 67 times by 2023) Sweden had a frequency of winning of 25%, as three of their wins happened since 2009. The total amount of Sweden’s wins is 7, which means four wins happened in the span of fifty-three years (Sweden first joined in 1958), and three happened in the span of only twelve years.
Screenshot of win frequency per country taken from the project code
There is no data on the year 2013.
Years 2009-2013 do not have separate televote and jury vote data.
It was also shown that certain graphs were excessively difficult to read due to the large amount of data present on the x-axis. In such cases, we can only draw general conclusions.

D. Applying machine learning approaches
We have attempted several different algorithms and approaches. 

The first one is Decision trees. A decision tree is a supervised learning algorithm, with a hierarchical tree structure consisting of a root node, branches, and leaf nodes [6].

It is a fairly simple algorithm, and we used it for several different things. We attempted to predict the final place, the language, style, key, and the vote for Germany, France and Serbia respectively. 

The decision tree was coded by giving it every variable that could affect the searched variable, and then it was training data and test data and ran using a classifier object. Lastly, the accuracy was printed as well as a graph of the tree. 

The next is Naive Bayes. Naive Bayes is a supervised machine learning algorithm using principles of probability to perform classification tasks [7].

There are several different types of Naive Bayes classifiers. 

Gaussian Naive Bayes, which is used with Gaussian distributions, where we use the mean and standard deviation of each class. This model is fitted by finding the mean and standard deviation of each class. 

Multinomial Naive Bayes, which assumes that the features are from multinomial distributions. It is typically applied within natural language processing.
 
Bernoulli Naive Bayes, which is used with Boolean variables.

We will use Gaussian and Bernoulli for our project to try out different ways of predicting certain data. 


IV. Results and discussions

A. Results
	The results for the Decision trees have not been satisfactory. When trying to predict the final place, the key, or the language the accuracy was extremely low and the graph of the Decision tree was much too complex and unreadable.

Results:

Decision tree predicting the language
Accuracy: cca. 0.75

Decision tree predicting the style
Accuracy: cca. 0.43

Decision tree predicting the Germany vote
Accuracy: cca. 0.89


B. Evaluating results
	The decision tree results were not satisfactory, especially for trying to predict data with a large number of possible endpoints. The graph would be extremely complex and illegible. The average accuracy for decision trees was cca. 0.63.

	Prediction simpler, numerical data gave better results. The average accuracy for predicting the vote a country gave to another is 0.86, which is an acceptable result. 

	Still, we can see that decision trees are not a favourable way to attempt to predict anything for our dataset.

	The results for using the Naive Bayes algorithm were not much more favourable. The average accuracy was 0.49, but with one outlier with an accuracy of 1.0. Since that result came from the algorithm only predicting three results, without counting it the average accuracy is 0.45.

	We tried to predict the likelihood of winning for certain styles, to narrow down the amount of data to be taken into account. The average accuracy was very low, at around 0.33.

	We also used Bernoulli Naive Bayes to find the gender (male/female) and key (major/minor). The average accuracy was 0.52, which shows that for this dataset Naive Bayes is not particularly useful.


V. Conclusion

	While going through this dataset is interesting, it is essential to accept its limitations and draw conclusions from this reality.

	We can easily see that both Decision Trees and Naive Bayes did not give favourable results. The accuracy was low, the graphs were illegible and the overall ways we can apply them are sparse.

	Higher accuracy results have been found when working with voting data from certain countries, showing that predicting the amount of points a country gives out is relatively easy. That can partly be attributed to the fact that certain countries are often not attending the finale or don’t vote for certain other countries, leading to zero points given.

	Something that was completely unpredictable is key and gender. Even though both values in this set are considered binary (major and minor, male and female), the accuracy for either is very low both for decision trees and for Naive Bayes (Bernoulli).

	From that, we can easily draw the conclusion that there is no pattern, and gender and key do not affect overall results. 

	We find that attempted data mining regarding winners alone is essentially impossible, due to only twelve winners being present in this dataset. 

	But, we can deduce certain facts from the lack of patterns in several parts of the dataset.

	Gender is not a defining characteristic in any way, it does not affect any other categories such as style, BPM, or final place; therefore we can assume that by the time this data was taken (2016), the contest was very inclusive and not affected by discrimination.

	That fact can also be confirmed further through genderqueer artists joining Eurovision in more recent years [8].

Furthermore, we can see that the key does not impact anything either. The category key was simplified into major and minor. The key is not connected to the final place, gender, country, or any other category. 

Even though the major key is considered “happy” and “upbeat”, and the minor key is considered “sad”, we can see there are no noticeable patterns in the way they are used by artists. They are not affected by the style (rock, ballad, etc), or the gender of the artist. 

The overall conclusion we can come to is that this dataset is not favourable for in-depth analysis. Only twelve years of data do not allow patterns to be found for winners, we can not find political data for Western European and Eastern European countries, and we can not find data on the big five and how they are affected by automatically entering the finale.

The accuracy for most patterns found was low, and even though we might sometimes try to make sense of those numbers, oftentimes the conclusion we would draw from those is misleading and false. The small number of contests we are working with can skew our data in one direction, and it is important to remember that it is only presented in that way because we have such a small amount of contests to draw knowledge.

At the end of this project, the main lesson to take is that what matters more is the dataset itself, than what one does with it.



VI. References and sources

[1]	Wikipedia,  (2024, May 28th), Eurovision Song Contest [Online], available: 
https://en.wikipedia.org/wiki/Eurovision_Song_Contest

[2]	A. Mateush, O. Cherednychenko, G. Noboa and V. Skydanienko, (2017), eurovision_mining [Online], 
available:
https://github.com/Similacrest/eurovision_mining

[3]	 Dr K. Goldmann, Dr R. Jersakova, Dr M. Stoffel, E. Chapman, Dr J. Yong, Dr C. Rangel Smith, D. 
Llewellyn-Jones, Dr J. Palmer, (2023, May 11th), Can data science help us predict the winner of Eurovision 2023? [Online], available: 
https://www.turing.ac.uk/blog/can-data-science-help-us-predict-winner-eurovision-2023

[4]	Anon., (2024), Eurovision song contest data [Online], available:
https://www.kaggle.com/datasets/diamondsnake/eurovision-song-contest-data/data

[5]	Verity Ritchie, (2023), The [Queer] Politics of Eurovision [Online], available: 
https://youtu.be/Wnjtzn7ZkCs?si=TgmbBLa7qsd9bqEc

[6]	IBM, What is a decision tree? [Online], available: https://www.ibm.com/topics/decision-trees
 
[7]	IBM, What are Naive Bayes Classifiers? [Online], available: https://www.ibm.com/topics/naive-bayes

[8]	Wikipedia, (2024, May 27th), List of LGBT participants in the Eurovision Song Contest [Online], available:
	https://en.wikipedia.org/wiki/List_of_LGBT_participants_in_the_Eurovision_Song_Contest

