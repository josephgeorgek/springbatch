

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




@Configuration
public class JiraJobConfig {

  @Bean
  public Job jiraJob(JobBuilderFactory jobBuilders,
      StepBuilderFactory stepBuilders) {

        System.out.println("helloWorlJob Spring XD!");

    return jobBuilders.get("jiraJob")
        .start(helloWorldStep(stepBuilders)).build();
  }

  @Bean
  public Step helloWorldStep(StepBuilderFactory stepBuilders) {
      return stepBuilders.get("step1").tasklet(new Tasklet() {
  
          @Override
          public RepeatStatus execute(StepContribution contribution, ChunkContext chunkContext) throws Exception {
              System.out.println("Hello jiraJob XD!3232323");

              Test test = new Test();
              test.getTest();
              return RepeatStatus.FINISHED;
          }
      }).build();
    }

}



package sample.jira.export;

import javax.sql.DataSource;
import org.springframework.batch.core.configuration.annotation.DefaultBatchConfigurer;
import org.springframework.batch.core.configuration.annotation.EnableBatchProcessing;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableBatchProcessing
public class BatchConfig extends DefaultBatchConfigurer {

  @Override
  public void setDataSource(DataSource dataSource) {
    // initialize a Map based JobRepository by default
  }
}






import org.springframework.batch.core.Job;
import org.springframework.batch.core.JobParameters;
import org.springframework.batch.core.JobParametersBuilder;
import org.springframework.batch.core.launch.JobLauncher;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration;
import org.springframework.scheduling.annotation.EnableScheduling;
import org.springframework.scheduling.annotation.Scheduled;


@SpringBootApplication(
    exclude = {DataSourceAutoConfiguration.class})
@EnableScheduling
public class App {

    @Autowired
    JobLauncher jobLauncher;
     
    @Autowired
    Job job;	

    private static final String JIRAEXPORT_PROPERTIES = "jiraexport.properties";


    @Scheduled(cron = "*/10 * * * * *?")
  public void perform() throws Exception 
  {
	  System.out.println("-------Job is starting ----------");
      JobParameters params = new JobParametersBuilder()
              .addString("JobID", String.valueOf(System.currentTimeMillis()))
              .toJobParameters();
      jobLauncher.run(job, params);
      System.out.println("-------Job ends ----------");
  }  


