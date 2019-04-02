# codechallenge

Java/API Server Engineer Code Challenge
Overview
Create a simple Java rotating logging facility that provides support for periodically rotating ("rolling over") log files by renaming and uploading them to a
remote server via FTP.
The facility should support multiple loggers, each of which can be separately configured to write to distinct log files, which at rotation time are
uploaded to distinct FTP servers.
Each logger should support 3 distinct log levels (DEBUG, INFO, ERROR), and it should be possible to configure each logger to filter out log
messages below a specified level (e.g., if the level is set to INFO, only ERROR and INFO messages should be logged, etc.).
Log rotation must be triggered immediately at the end of the rotation interval, as opposed to being triggered by the first log message which lies
outside the current interval.
Empty log files should not be rotated.
The objective of this challenge is for you to demonstrate that you're able to create a simple, well-designed, requirements-satisfying framework implemented
with clean and readable code.
Logger Configuration
Logger configuration is to be read from a YAML file. Following is an example which configures two loggers:
logger1:
 interval: 5 # minutes
 host: ftp.myserver.com
 port: 21
 username: foo
 password: bar
 log-file-directory: /home/fred/logs
 log-file-name: logger1.log
 level: INFO
logger2:
 interval: 3 # minutes
 host: ftp.anotherserver.com
 port: 21
 username: foo
 password: bar
 log-file-directory: /home/fred/logs
 log-file-name: logger2.log
 level: INFO
You can feel free to add additional configuration parameters as appropriate.
Log File Name
Until a log file is rotated, its name should be as specified in the configuration file. When the file is "rotated" up to the FTP server, file should be renamed by
appending to the original name a timestamp (corresponding to the time at which the file was rotated) of the form ("_YYYYMMDD_HHMMSS"), where
YYYY = the 4-digit year, MM= the 2-digit month, etc. After a file has been successfully uploaded, it can be removed from the local log directory.
Sample Use of Logging Facility
1.
2.
3.
4.
import Logger;
public class MyApplication
{
 public static void main(String[] args)
 {
 // Logger.init() reads the configuration file and creates all Logger class instances.
 // Logger instances can be retrieved by name via calls to Logger.getLogger().
 // The first command-line argument is assumed to contain the path to the
 // configuration file.
 Logger.init(args[0])
 Logger logger = Logger.getLogger("logger1")
 logger.info("test message")
 // Do whatever it is that this application does. It is assumed to run indefinitely.
 }

 // Other application functions...
 public void Foo()
 {
 // Assumed to be running in some thread context.
 Logger logger = Logger.getLogger("logger2")
 logger.debug("test message")
 }
 public void Bar()
 {
 // Assumed to be running in some other thread context.
 Logger logger = Logger.getLogger("logger2")
 logger.debug("test message")
 }
}
Other Details
Concurrency and Thread Safety -Your logger should be thread-safe, meaning that two application threads should be able to log messages to
the same logger without interfering with one another. Also, slow FTP transfers should not hold up the rest of the logging facility. I.e., a thread
which wants to log should not be be blocked while a slow transfer is taking place.
Use of Libraries - You should feel free to use whatever freely available libraries you'd like (e.g., for YAML parsing, FTP client functionality, etc.)
to implement your logging facility. Obviously you must implement the core logging functionality yourself.
Error Handling - Include whatever error/exception handling mechanisms you think are appropriate, keeping in mind that this challenge is
intended to take only a couple hours to complete. In particular, there's no need to include retry logic for FTP upload failures.
Remote FTP Path - Files can uploaded to the default directory on the FTP server. There is no need to upload to a specific directory.
Stuff Not to Worry About
This logging facility is simple. Real logging facilities have many more features. E.g., support for message format
