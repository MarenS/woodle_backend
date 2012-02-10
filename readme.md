woodle.backend
========================


REST URLs (/rest)
-----------

Create a new member (Registration)
-----------


    Path:               /members
    Method:             PUT (old POST)
    RequestHeader:      content-type:application/json
    RequestBody:        {"email":"$email","password":"$password","phonenumber":"$phonenumber"}
    Response:           Status 200, if User is created
    Implemented:        partly, see MemberResourceRESTService -> addMember()
    Details:            Needs to be a PUT, because it creates a new resource

Details: Member.java must contain: E-Mail (ok), Password (missing), PhoneNumber (ok). Delete ID and Username.

Caution E-Mail is the key!

Login as a member (Authorisation)
-----------

    Path:               /members/authorisation
    Method:             POST
    RequestHeader:      content-type:application/json
    RequestBody:        {"email":"$email","password":"$password"}
    Response:           Status 200, if E-Mail and Password are correct
    Implemented:        NO
    Details:            Login with E-Mail and Password.
    
Modify a member (myself)
-----------


    Path:               /members/$email
    Method:             POST, (old PUT)
    RequestHeader:      content-type:application/json
    RequestBody:        {"username":"$username","password":"$password","email":"$email","phonenumber":"$phonenumber"}
    Response:           Status 200: OK
    Implemented:        partly, see MemberResourceRESTService -> modifyMember()
    Details:            Login with E-Mail and Password
   

Create a new appointment (invite someone to an appointment)
-----------


    Path:               /appointments
    Method:             PUT
    RequestHeader:      content-type:application/json
    RequestBody:        {"id":$id","title":"$title","location":"$location","description":"$description","startDate":"$startDate","endDate":"$endDate,"attendance":["$useremail-eventId","$attendance2"],"maybeAttendance":["$maybeAttendance1","$maybeAttendance2"],"user":$user,"maxNumber":$maxNumber}
    Implemented:        partly, see AppointmentsRessource -> create()
    Caution:            ID is the key, which consists of title-startDate, e.g. "id":"title-2012-21-03-16.20.00". Date: YYYY-MM-DD-HH-MinutesM-SecondsS
    Caution2:           for "attendance":["$useremail-eventId","$attendance2"]: useremail-eventId can be "info@info.com-343a4h4". Need E-Mail and EventID together, so that the Client knows in which calender delete which event
    Caution3:           for maybeAttendance: consists of useremail.eventId (same as attendance), e.g. "info@info.com-343a4h4"
    Details:            maxNumber: How many people are allowed to add this appointment?
                        user: E-Mail Adress of the user, who created this appointment
                        attendance: E-Mail Adresses of users, who wants to use this appointment
                        maybeAttendance: if maxNumber > size of attendance : the user will fill in maybeAttendance

Details: Appointment.java needs description (String), startDate (old start) (String), endDate (String), attendance (ArrayList<String>), maybeAttendance (ArrayList<String>), user (String), maxNumber (int)

    
Show all appointments
-----------


    Path:               /appointments
    Method:             GET
    RequestHeader:      -
    RequestBody:        -
    Response:           {"appointments":[{appointment1},{appointment2}]}
    Response Status:    200: OK
    Example Response:   {\"appointments\":[{\"id\":\"$id\",\"title\":\"Joggen am Mittag\",\"location\":\"location\",\"description\":\"$description\",\"startDate\":\"2012-05-21-16.20.00\",\"endDate\":\"$endDate\",\"attendance\":\"$useremail-eventId"\",\"maybeAttendance\":\"$maybeAttendance1\",\"user\":\"$user\",\"maxNumber\":\"$maxNumber\"},{\"id\":\"$id2\",\"title\":\"Jobbörse\",\"location2\":\"location\",\"description\":\"$description2\",\"startDate\":\"2012-05-21-16.20.00\",\"endDate\":\"$endDate2\",\"attendance\":\""$useremail-eventId"\",\"maybeAttendance\":\"$maybeAttendance3\",\"user\":\"$user1\",\"maxNumber\":\"$maxNumber1\"}]}
    Caution:            for info with attendance and maybeAttendance, see Create a new appointment! (Caution2&3)
    Implemented:        partly, see AppointmentsRessource -> serviceGetAppointments()
    
Show a specific appointment
-----------

    Path:               /appointments/$id
    Method:             GET
    RequestHeader:      -
    RequestBody:        -
    Implemented:        NO

Show all appointments, which I have created
-----------

    Path:               /appointments/$email
    Method:             GET
    RequestHeader:      -
    RequestBody:        -
    Implemented:        partly, see AppointmentsRessource -> serviceGetAppointments()
    Details:            $email is the E-Mail Adress of $user in all appointments
 
Show all appointments, which I belong to (user is in attendance OR maybeAttendance with $user-eventId)
-----------

    Path:               /appointments/myappointments/$email
    Method:             GET
    RequestHeader:      -
    RequestBody:        -
    Implemented:        partly, see AppointmentsRessource -> serviceGetAppointments()
    Details:            $email is the E-Mail Adress of $user in all appointments
    
Show a specific appointment, which I have created
-----------

     see Show a specific appointment (Android Client will filter the results; nothing to do!)

Add me to an appointment (attendance)
-----------

    Path:               /appointments/$id
    Method:             POST
    RequestHeader:      content-type:application/json
    RequestBody:        {"id":$id","title":"title","location":"location","description":"$description","startDate":"$startDate","endDate":"$endDate,"attendance":["$attendance1","$attendance2"],"maybeAttendance":["$maybeAttendance1","$maybeAttendance2"],"user":$user,"maxNumber":$maxNumber}
    Implemented:        NO
    Details:            see Create a new appointment
    
Delete me from an appointment, where I belong to
-----------

    see Add me to an appointment (Android Client will filter the results; nothing to do!)


Delete an appointment
-----------

    Path:               /appointments/$id
    Method:             DELETE
    RequestHeader:      -
    RequestBody:        -
    Implemented:        NO
    
Not necassary
-----------
Delete my user profile. 
Edit an appointment. Groups.


What is it?
-----------

This is your project! It's a sample, deployable Maven 3 project to help you
get your foot in the door developing with Java EE 6 on JBoss AS 7 or EAP 6. This 
project is setup to allow you to create a compliant Java EE 6 application 
using JSF 2.0, CDI 1.0, EJB 3.1, JPA 2.0 and Bean Validation 1.0. It includes
a persistence unit and some sample persistence and transaction code to help 
you get your feet wet with database access in enterprise Java. 

System requirements
-------------------

All you need to build this project is Java 6.0 (Java SDK 1.6) or better, Maven
3.0 or better.

The application this project produces is designed to be run on a JBoss AS 7 or EAP 6. 
The following instructions target JBoss AS 7, but they also apply to JBoss EAP 6.
 
With the prerequisites out of the way, you're ready to build and deploy.

Deploying the application
-------------------------
 
First you need to start JBoss AS 7 (or EAP 6). To do this, run
  
    $JBOSS_HOME/bin/standalone.sh
  
or if you are using windows
 
    $JBOSS_HOME/bin/standalone.bat

To deploy the application, you first need to produce the archive to deploy using
the following Maven goal:

    mvn package

You can now deploy the artifact to JBoss AS by executing the following command:

    mvn jboss-as:deploy

This will deploy `target/woodle.backend.war`.
 
The application will be running at the following URL <http://localhost:8080/woodle.backend/>.

To undeploy from JBoss AS, run this command:

    mvn jboss-as:undeploy

You can also start JBoss AS 7 and deploy the project using Eclipse. See the JBoss AS 7
Getting Started Guide for Developers for more information.
 
Running the Arquillian tests
============================

By default, tests are configured to be skipped. The reason is that the sample
test is an Arquillian test, which requires the use of a container. You can
activate this test by selecting one of the container configuration provided 
for JBoss AS 7 (remote).

To run the test in JBoss AS 7, first start a JBoss AS 7 instance. Then, run the
test goal with the following profile activated:

    mvn clean test -Parq-jbossas-remote

Importing the project into an IDE
=================================

If you created the project using the Maven archetype wizard in your IDE
(Eclipse, NetBeans or IntelliJ IDEA), then there is nothing to do. You should
already have an IDE project.

Detailed instructions for using Eclipse with JBoss AS 7 are provided in the 
JBoss AS 7 Getting Started Guide for Developers.

If you created the project from the commandline using archetype:generate, then
you need to import the project into your IDE. If you are using NetBeans 6.8 or
IntelliJ IDEA 9, then all you have to do is open the project as an existing
project. Both of these IDEs recognize Maven projects natively.

Downloading the sources and Javadocs
====================================

If you want to be able to debug into the source code or look at the Javadocs
of any library in the project, you can run either of the following two
commands to pull them into your local repository. The IDE should then detect
them.

    mvn dependency:sources
    mvn dependency:resolve -Dclassifier=javadoc
