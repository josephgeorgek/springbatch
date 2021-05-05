### An introduction for usage of JIRA Java APIs. 

I'll pick up as the example to export JIRA issues' summaries into Microsoft Excel. 
It is already provided as a standard function by Atlassian(*), but I hope this could be helpful for person who starts learning how to use JIRA Java APIs.

\* [JIRA Exporting Search Results to Microsoft Excel](https://confluence.atlassian.com/jira064/exporting-search-results-to-microsoft-excel-720416693.html)



 <dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-batch</artifactId>
		</dependency>




	
"0 0 * * * *" = the top of every hour of every day.
"*/10 * * * * *" = every ten seconds.
"0 0 8-10 * * *" = 8, 9 and 10 o'clock of every day.
"0 0 8,10 * * *" = 8 and 10 o'clock of every day.
"0 0/30 8-10 * * *" = 8:00, 8:30, 9:00, 9:30 and 10 o'clock every day.
"0 0 9-17 * * MON-FRI" = on the hour nine-to-five weekdays
"0 0 0 25 12 ?" = every Christmas Day at midnight
**/


#### Essential codes for searching JIRA issues are only two steps.;

```java
    // 1. Create JiraRestClient with using AsynchronousJiraRestClientFactory.
    JiraRestClientFactory factory = new AsynchronousJiraRestClientFactory();
    URI uri = new URI("https://your-project-name.atlassian.net");
    jiraRestClient = factory.createWithBasicHttpAuthentication(uri, userName, password);

    // 2. Get SearchRestClient and search by jql(JIRA Query Language).
    SearchRestClient searchRestClient = jiraRestClient.getSearchClient();
    final Set<String> fields = new HashSet<String>();
    String jql = "status!=done"; // Only not done issues.
    SearchResult result = searchRestClient.searchJql(jql, max, offset, fields).claim();
    Iterable<Issue> issues = result.getIssues();
    List<Issue> list = new ArrayList<Issue>();
    for (final Issue issue : issues) {
        list.add(issue);
    }
```

----

### JIRAのJava APIの使い方の紹介。

JIRAのissueの概要をMS エクセルに出力するJavaのプログラムを例にあげています。
既に標準機能として提供されている(*)のですが、JIRAのJava APIを学びたい人の役に立つと良いです。

\* [JIRA 検索結果を Microsoft Excel にエクスポートする](https://ja.confluence.atlassian.com/jira063/exporting-search-results-to-microsoft-excel-683542534.html)

このコードはただの例なので、特にExcelを出力する個所が洗練されていません。
