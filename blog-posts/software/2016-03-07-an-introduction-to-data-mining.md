# An Introduction to Data Mining

### Extracting Meaningful Information from Data

Decision making might be one of the most common yet more difficult things to do. When doing so, we have to make sure we are being as objective as possible and confirm our hypothesis about the given situation is correct. One way to do this is by analyzing data about the desired scenarios to later on translate these into specific actionable. At [Ubiqua](http://www.ubiqua.me/), we had the opportunity to help the product development decision making process by analyzing the comments section of one of our products. We wanted to be able to:

- Find and objectively prioritize problems (or opportunities) which our application failed to solve.
- Understand how the comments section was being used to cope with functionality not provided by our application.

In this blog post I will explore how I used data mining to answer some of the questions raised by the concerns described above.

### Data Driven Decision Making
Data driven decision making refers to the analysis of data to lead decisions which enhance success. A precise analysis of data provides invaluable information for decision making, as the results of the analysis are the direct outcome of user behavior.

When doing data mining, clean and well understood data is the foundation to conduct an accurate analysis. If data needs to be manipulated or cleansed in order to create a proper dataset, make sure you do so before starting the analysis. These are a good starting point for creating a clean dataset (some might not apply to your specific needs):

- Know what type of data you will be analyzing.
  - What language is it in?
  - Do users use one or multiple words to refer to the same thing?
- Standardize letter case.
- Remove accents.
- Remove stop words.
- Do users use non-words (emoticons, etc) to represent meaning?

We can start off our analysis after the data has been preprocessed. One of the simplest yet more powerful techniques to start a data mining analysis is [tokenization](https://www.ibm.com/developerworks/community/blogs/nlp/entry/tokenization?lang=en). Tokenization refers to the process of splitting a stream of text into smaller units called tokens, usually words or phrases. Next, we can proceed to apply some common data mining techniques to the dataset:

- `Term frequency`: A term frequency analysis is simply a word count which allows us to observe what are the most commonly used terms in a dataset. This in turn can help us shape a particular algorithm we might need to create in order to answer a specific question about the data at hand.
- `N-grams`: N-grams are a sequence of `n`-terms. The most popular n-gram analysis is the sequence of two terms (bigram). Bigrams are an easy alternative to implement when term frequency analysis does not give us enough information of what the data is all about.
- `Term co-occurrences`: A term co-occurrence analysis determines what two words occur together most frequently (not necessarily side-by-side). This analysis can be extended to receive a particular word as input and then output with what other word does it co-occur the most.

### Data Visualization
When doing data mining, it is also important to properly showcase the analysis results. If you are working with Python, I recommend taking a look at [Vincent](https://github.com/wrobstory/vincent). As the description of the library says, ``The data capabilities of Python. The visualization capabilities of JavaScript.``, Vincent allows to easily exhibit the results of an analysis in a coherent and professional way.

### Conclusion
The analysis of data has enabled us to backup our decisions with direct input from user behavior, which in turn enhances our chances of a successful decision making process. I highly recommend reading [this series of articles](http://marcobonzanini.com/2015/03/02/mining-twitter-data-with-python-part-1/) about data mining with Python and Twitter, as it provides an excellent foundation for getting started with it.
