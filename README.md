[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)

Introduction
---
BeeCP,a lightweight and  fast JDBC connection pool implementation. 

<a href="http://central.maven.org/maven2/com/github/chris2018998/BeeCP/0.72/BeeCP-0.72.jar">Download beeCP_0.72.jar</a>

Configuration
---
|  Name  |   Description |   Remark |
| ------------ | ------------ | ------------ |
| poolInitSize  | connection size need create when pool initialization  |   |
| poolMaxSize |  max connnection size in pool |    |
| borrowerMaxWaitTime |request timeout for borrower(ms)  |   |
| preparedStatementCacheSize | stement cache size |   |
| connectionIdleTimeout  | max idle time,then will be close(ms)  |    |
| validationQuerySQL |  a test sql to check connection ative   |    |   |

 DataSource Demo
---
```java
String userId="root";
String password="";
String driver="com.mysql.jdbc.Driver";
String URL="jdbc:mysql://localhost/test";
BeeDataSourceConfig config = new BeeDataSourceConfig(driver,URL,userId,password);
DataSource datasource = new BeeDataSource(config);
Connection con = datasource.getConnection();
....................
```

Performace test
---
1: JMH Test with <a href="https://github.com/brettwooldridge/HikariCP-benchmark">HikariCP Benchmarks code</a> 

add the following code to HikariCP class: com.zaxxer.hikari.benchmark.BenchBase

 private void setupBeeCPWithCompete(){
		 BeeDataSourceConfig sourceInfo = new BeeDataSourceConfig(
				 "com.zaxxer.hikari.benchmark.stubs.StubDriver", 
				jdbcUrl,
				"brettw", 
				"");
	 	sourceInfo.setPoolMaxSize(maxPoolSize);
	  sourceInfo.setPoolInitSize(MIN_POOL_SIZE);
	 	sourceInfo.setBorrowerMaxWaitTime(8000);
	 	sourceInfo.setValidationQuerySQL("select 1");
		 DS = new BeeDataSource(sourceInfo);
 }
	
 private void setupBeeCPWithFair(){
	 	BeeDataSourceConfig sourceInfo = new BeeDataSourceConfig(
				"com.zaxxer.hikari.benchmark.stubs.StubDriver", 
			 	jdbcUrl,
				 "brettw", 
				 "");
	 	sourceInfo.setPoolMaxSize(maxPoolSize);
		 sourceInfo.setPoolInitSize(MIN_POOL_SIZE);
	 	sourceInfo.setBorrowerMaxWaitTime(8000);
		 sourceInfo.setValidationQuerySQL("select 1");
		 sourceInfo.setFairMode(true);
		 DS = new BeeDataSource(sourceInfo);
 }





