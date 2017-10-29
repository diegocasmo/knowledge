# Getting Started With Acceptance Testing

### A Larger Product and Team
At [Ubiqua](http://www.ubiqua.me/), as we started to grow both our product and engineering team, we realized some things had to be re-prioritized and fixed. Some of the problems we were having were:

- On-boarding for new developers took more time than we desired. This was mainly being cause by legacy code and the bad documentation of features and processes.
- Software engineers were not able to determine if a specific bug fix or feature broke older functionality.
- Slow production line. We needed to spend a lot of time manually testing if new functionality behaved as expected and did not break anything else.

As a result of this, we decided to change our development work-flow to a more collaborative and documented process, where changes to the code-base followed a specific set of actions. These actions are:

- All new features must include tests (bonus points for using TDD).
- Bug fixes must include a corresponding regression test.
- One test must be added to the existing code-base every week.

One of the tools we chose for our plan of action was [Cucumber](https://cucumber.io/). In this blogpost I will guide you through the basics and good practices of writing acceptance tests with it.

### Acceptance Tests
An acceptance test demonstrates what a feature (or system) needs to do in order for the stakeholders to find it acceptable. At Ubiqua, we like to think of testing in the following way:

- Acceptance tests: build the right thing.
- Unit tests: build the thing right.

Cucumber allows us to write acceptance tests by specifying a concrete number of executable examples (build the right thing) of how the system should behave under certain circumstances. Writing down these examples encourages a team to think about the different use-cases that a feature needs to serve which in turn assists in revealing insights about other feature use-cases that were unknown at the beginning of the iteration. Such practices help to avoid a [solution looking for a problem](http://xyproblem.info/) and consecutively boost the team's productivity.

### Overview of a Cucumber Feature
A Cucumber feature is a set of examples of how the system should behave under certain circumstances. Each feature is made of a number of examples called ``Scenarios``. Scenarios must be independent and usually follow this pattern:

- Context: Put the system in a particular state (``Given``).
- Action: Do something with the system (``When``).
- Expectation: Examine the new state is correct (``Then``).

Features in Cucumber are written in ``Gherkin``. Gherkin is a language used to facilitate communication with stakeholders to illustrate what the software should do. The greatest advantage of using Gherkin is that it is designed for human readability and thus helps developers to interact with stakeholders and create documentation at the same time.

### A Cucumber Feature Example

A simple Cucumber feature usually consists of the following components:

- ``Feature``: A convenient place to put a summary and documentation about the group of tests that follow.
- ``Scenario``: A concrete example of the desired behavior of the system under certain circumstances.
- A list of ``Steps``: Steps are plain language instructions of what the system/user will do and the desired output it should create. It allows developers to pass parameters of the values that are relevant to the scenario by using regular expressions.

Let's go ahead and write a simple Cucumber feature to verify that a user is able to send an order with some products to a specific client:

``` ruby
Feature: Create an Order

  Ubiqua allows salesmen to order products for their clients.

  Scenario: Order Some Products
    Given I am logged in as a salesman assigned to "Farmacia Moreno", with the following products:
      | product_name  | price |
      | Caja Gatorade | 1.00  |
      | Botella Agua  | 2.00  |
    When I send an order for client "Farmacia Moreno" with the following products:
      | product_name  | quantity |
      | Caja Gatorade | 2        |
      | Botella Agua  | 4        |
    Then the order should have total price $10.00
```

A scenario should be easy to read and understand. It should avoid mentioning details that have no actual relevance to the purpose of the scenario (incidental details). In a sense, it is like a joke; if you have to explain it, then it is a bad one.

Scenario steps are translated into system actions by ``step definitions``. A step definition is similar to a method definition; it can take 0 or more arguments identified by groups in the regular expression of the step. The step definition for the first step of the scenario above will look something like this:

``` ruby
Given(/^I am logged in as a salesman assigned to "(.*?)", with the following products:$/) do |client, table|
  salesman = Support::Salesman.find_or_create
  client = Support::Client.new(client)
  Support::Salesman.assign_client(client)
  Support::Product.assign_products(salesman, table.hashes)
  LoginPage.visit.login(salesman)
end
```

Some of the code has been abstracted for simplicity, but the main idea of what a step definition should do is still clear. The step definition communicates with the system and executes the necessary actions to be able to compare the system against a particular state later on.

Just as unit tests do, Cucumber will run each step in the scenario and mark it as passing if no exceptions are thrown. If an exception is thrown, it will stop executing the steps of that scenario and mark it as failing. If all the steps in the scenario pass, then it will mark that scenario as passing. It is common to use Cucumber with an assertion library such as [RSpec](http://rspec.info/) to be able to compare the result of particular action to the expected value.

### Conclusion
We have found acceptance tests have helped us to be more efficient and to have the courage to perform major surgery to our code-base. At the same time, as we are making our product more robust and reliable, we are creating documentation for it which is easy to read and explain to other people. I would highly recommend reading [The Cucumber Book](http://www.amazon.com/The-Cucumber-Book-Behaviour-Driven-Development/dp/1934356808), as it will help you to both improve your software engineering skills and have a greater understanding of acceptance testing (using Cucumber as a tool for it) and its goal.
