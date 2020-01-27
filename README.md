# Application To Upload/Download video To/From A Cloud Service

This project is my solution to Jules White's first assignement on
[coursera](https://www.coursera.org/learn/cloud-services-java-spring-framework).

## Running the Application

To run the application:

Right-click on the Application class in the org.magnum.dataup
package, Run As->Java Application

To stop the application:

Open the Eclipse Debug Perspective (Window->Open Perspective->Debug), right-click on
the application in the "Debug" view (if it isn't open, Window->Show View->Debug) and
select Terminate

## Overview

A popular use of cloud services is to manage media that is uploaded
from mobile devices. This assignment will create a very basic application
for uploading video to a cloud service and managing the video's metadata.
Once you are able to build this basic type of infrastructure, you will have
the core knowledge needed to create much more sophisticated cloud services.


## Instructions

This assignment tests your ability to create a web application that
allows clients to upload videos to a server. The server allows clients
to first upload a video's metadata (e.g., duration, etc.) and then to
upload the actual binary data for the video. The server should support
uploading video binary data with a multipart request.

The test that is used to grade your implementation is AutoGradingTest
in the org.magnum.dataup package in src/test/java. **_You should use the
source code in the AutoGradingTest as the ground truth for what the expected
behavior of your solution is_.** Your app should pass this test without 
any errors. The test methods are annotated with @Rubric and specify
the number of points associated with each test, the purpose of the test,
and the videos relevant to the test. 

The HTTP API that you must implement so that this test will pass is as
follows:
 
GET /video
   - Returns the list of videos that have been added to the
     server as JSON. The list of videos does not have to be
     persisted across restarts of the server. The list of
     Video objects should be able to be unmarshalled by the
     client into a Collection<Video>.
   - The return content-type should be application/json, which
     will be the default if you use @ResponseBody

     
POST /video
   - The video metadata is provided as an application/json request
     body. The JSON should generate a valid instance of the 
     Video class when deserialized by Spring's default 
     Jackson library.
   - Returns the JSON representation of the Video object that
     was stored along with any updates to that object made by the server. 
   - **_The server should generate a unique identifier for the Video
     object and assign it to the Video by calling its setId(...)
     method._** 
   - No video should have ID = 0. All IDs should be > 0.
   - The returned Video JSON should include this server-generated
     identifier so that the client can refer to it when uploading the
     binary mpeg video content for the Video.
   - The server should also generate a "data url" for the
     Video. The "data url" is the url of the binary data for a
     Video (e.g., the raw mpeg data). The URL should be the _full_ URL
     for the video and not just the path (e.g., http://localhost:8080/video/1/data would
     be a valid data url). See the Hints section for some ideas on how to
     generate this URL.
     
POST /video/{id}/data
   - The binary mpeg data for the video should be provided in a multipart
     request as a part with the key "data". The id in the path should be
     replaced with the unique identifier generated by the server for the
     Video. A client MUST *create* a Video first by sending a POST to /video
     and getting the identifier for the newly created Video object before
     sending a POST to /video/{id}/data. 
   - The endpoint should return a VideoStatus object with state=VideoState.READY
     if the request succeeds and the appropriate HTTP error status otherwise.
     VideoState.PROCESSING is not used in this assignment but is present in VideoState.
   - Rather than a PUT request, a POST is used because, by default, Spring 
     does not support a PUT with multipart data due to design decisions in the
     Commons File Upload library: https://issues.apache.org/jira/browse/FILEUPLOAD-197
     
     
GET /video/{id}/data
   - Returns the binary mpeg data (if any) for the video with the given
     identifier. If no mpeg data has been uploaded for the specified video,
     then the server should return a 404 status code.
     
      
 The AutoGradingTest should be used as the ultimate ground truth for what should be 
 implemented in the assignment. If there are any details in the description above 
 that conflict with the AutoGradingTest, use the details in the AutoGradingTest 
 as the correct behavior and report the discrepancy on the course forums. Further, 
 you should look at the AutoGradingTest to ensure that
 you understand all of the requirements. It is perfectly OK to post on the forums and
 ask what a specific section of the AutoGradingTest does. Do not, however, post any
 code from your solution or potential solution.
 
 There is a VideoSvcApi interface that is annotated with Retrofit annotations in order
 to communicate with the video service that you will be creating. Your solution controller(s)
 should not directly implement this interface in a "Java sense" (e.g., you should not have
 YourSolution implements VideoSvcApi). Your solution should support the HTTP API that
 is described by this interface, in the text above, and in the AutoGradingTest. In some
 cases it may be possible to have the Controller and the client implement the interface,
 but it is not in this 
 
 Again -- the ultimate ground truth of how the assignment will be graded, is contained
 in AutoGradingTest, which shows the specific tests that will be run to grade your
 solution. You must implement everything that is required to make all of the tests in
 this class pass. If a test case is not mentioned in this README file, you are still
 responsible for it and will be graded on whether or not it passes. __Make sure and read
 the AutoGradingTest code and look at each test__!
 
 You should not modify any of the code in Video, VideoStatus, VideoSvcApi, AutoGrading,
 or AutoGradingTest. 

## Testing Your Implementation

To test your solution, first run the application as described above. Once your application
is running, you can right-click on the AutoGradingTest->Run As->JUnit Test to launch the
test. Eclipse will report which tests pass or fail.

To get an estimated score for your solution, right-click on AutoGrading (not AutoGradingTest) and
Run As->Java Application. The AutoGrading application will run AutoGradingTest and then print a
summary of the test results and your score to the Eclipse Console (Window->Show View->Console). 
The AutoGrading application will also create a submission package that you will submit as the
solution to the assignment in Coursera.
