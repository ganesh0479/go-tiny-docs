> # WELCOME TO GO TINY APPLICATION DESIGN

> ## GO TINY APPLICATION
## PROBLEM STATEMENT
 Our organization is going through a digital transformation phase. During this journey, we have created several digital components across various platform(like micro services, widgets, documentations, monitoring dashboards etc).
 The easiest way to have the references of all these components is through bookmarks. However, bookmarks has to be imported and exported inorder to share it with any one like a new joiner. In bookmarks, you cannot detail more about what these bookmarks are about by providing additional information through short description or an image(s) or customized icons(s).
 Bookmarks are not centralized. Even if you try to centralize it by exporting them and storing in a repository, every time a url is changed or added or removed all the people have to re-import those bookmarks from the centralized repository. It also becomes complex when you already have bookmarks and then importing could result in overlapping.
 It also becomes hard when we want to share many such lengthy urls such as kibana dashboard urls. A shorter url will become easy to share in this case.

## ARCHITECTURE
> ### Hexagonal architecture
* Allows an application to equally be driven by users, programs, automated test or batch scripts, and to be developed and tested in isolation from its eventual run-time devices and databases.
* Allows to isolate the core business of an application and automatically test its behaviour independently of everything else
* Explicitly separate User-Side, Business Logic, and Server-Side
* Dependencies are going from User-Side and Server-Side to the Business Logic
* Isolate the boundaries by using Ports and Adapters
![](/_media/hexagonal.png)

## INFRASTUCTURE
> #### AZURE

> #### DOCKER

> #### AZURE KUBERNETES CLUSTER (AKS)

> #### AZURE CONTAINER REGISTRY (ACR)

> #### POSTGRESQL DATABASE ON AZURE

## TECHNICAL STACK

BACKEND  | FRONTEND|CLOUD|CICD|
------------- | -------------|--------------|------------
Adopt OpenJDK 14 (JAVA 14)  | Node v12.18.3 | AZURE| Travis CI
Springboot 2.3.3  | Angular 10.0.5 | AZURE KUBERNETES CLUSTER | Codecov
Postgresql  | ngx-bootstrap | AZURE CONTAINER REGISTRY | Codacy
JUnit5  | bootstrap-4 | POSTGRESQL IN AZURE | Docker
Google SMTP Mail  | font-awesome |  | Kubernetes
OpenAPI|

## APPLICATION FLOW
> REGISTRATION / SIGN IN
* Application starts with registration page having options to register or Sign in the user
* After authentication success then the page navigates to Home, where all the available cards which are not assigned to any group will be displayed in the **CARDS** section

> CARDS SECTION
* In this section user can **create cards** with title, name of the card (Unique), actual url and expiration time in minutes
* Once the card get created user can click on the card to navigate to the actual url
* Once the card get created user can **update**, **delete** ,**assign to group** and **share** the card through the application
* To **update** card a modal will open where he can update title, description and modify expiry time
* To **Delete** a card a pop-over modal will come to confirm the delete action
* To **Share** a card a pop-over modal will come and asks for sender email address and receiver email address to share the card with **short url**
* Once more option available in the card to **assign to a group** with icon **G**

> GROUPS SECTION

* In this section user can **create groups** with title and description and user will become admin of that group
* All the **created groups** will be shown in this page. User can any group to see cards of particular group
* Once the Group get created user can navigate to the **cards** section and assign a card to him
* Once it got assigned to the user it will go to the **authorize** section of **Group Cards** where he can authorize card to show in the Sections
* Once user clicks on any **group** it will take the user to group cards page where it will have below three sections
    
    > GROUP CARDS
    * This section is available to all the users of the group
    * In this section we will show all the cards assigned to group
    * User can **update**, **delete** and **share** the group card and user can redirect to actual url on clicking card
    * Admin can add/update roles to another user on clicking the button **ADD USER** with the roles ADMIN/REGULAR
    * User can share the group cards on clicking **SHARE GROUP CARDS** button
    * If normal user update/delete the card in the group then the card will be moved to next section (**PENDING CARDS**)
    
    > PENDING CARDS
    * This section will be available only to the **Admin** user
    * In this section Admin will see only **pending cards** to approve
    * **Approve button** will be available in the **card footer**. If admin user clicks on it will move to next sections (**AUTHORIZE CARDS**)

    > AUTHORIZE CARDS
    * This section will be available only to the **Admin** user
    * In this section Admin will see only **cards those need authorization to display** in group cards section
    * **Authorize button** will be available in the **card footer**. If admin user clicks on it, the card will be displayed again in the **Group Cards** Section
    

## ACHIEVED FUNCTIONAL REQUIREMENTS
* User should generate short URLs which will expire after a standard default timespan. User should also be able to specify the expiration date.
  * **[URL shortner alogorithm](/design/backend/design?id=url-shortening-algorithm)**
  
* User should be able to create cards representing the url where each card has a short title, brief description and a customizable picture. Default picture would be the favicon of the serving application.

* User should be able to group cards in terms of tribes, feature teams, platforms or application.

* User should be able to share the group urls.
  * [Sharing short URLs](/design/backend/design?id=sharing-short-urls)
  
* Each card should be a short url with the re-direction to the original url. This short url will have no expiration as it belongs to a group. The generation of this short url should be dynamic and unique and could carry some contextual information too.

* The creator of the group will be the admin(role) user after which s\he will be able to add one or more admin user who would have authority to make changes. Only admin user(s) can remove a user from admin role.

* Admin user(s) of the group should be able to authorize a card to be displayed on the group, make changes like updating or deleting card(s). Unless the admin user approves the card or its changes, it will not be displayed on the group page

* A normal user i.e. not an admin user can suggest changes to an existing card. The changes are to be queued in order to be approved by the group's admin user(s).

* User should be able to share the group page enlisting all the cards of that group
  * [Sharing short URLs](/design/backend/design?id=sharing-short-urls)

## ACHIEVED NON-FUNCTIONAL REQUIREMENTS
* Deployed application in **[AZURE KUBERNETES CLUSTER]()** so that the system will be highly available on the cloud

* Created a **[LOAD BALANCER]()** in **[AZURE KUBERNETES]()** platform, so that the traffic will be shared among the **PODS** 

* Azure cloud platform is easy to use and maintain and we can quickly make changes through azure portal or its in-built **AZURE CLI**

* I have written algorithm to generate unpredictable short URLs
    **[URL shortner alogorithm]()**

* Exposed all the APIs through **[OPEN API]()**

* I have included **[CICD]()** pipeline from initial commits and able to build and test, and able to generate coverage and deploy to kubernetes for backend application 

* Azure is providing clean documentation to work with it.

* URL redirection should happen in real-time with minimal latency.

* Followed TDD approach (80%) for the backend module, created test cases first and then I have written the business logic

* Exposed the **monitoring endpoints** to see the metric in Kibana and not yet configured in Azure

* Followed Google code java style format



## URL SHORTENING ALGORITHM
> ##### An Algorithm to generate unique **6 character** short string 
   *  Take the **system time in millis** since it is unique and encode it with **base-62** 
   *  Encoded **base-62** hash can generate many combinations vary from 1 to 62 characters which will give inconsistent short URL. Like sometimes it will give 'a', sometimes it will be 'ha' etc.,
   *  The problem I observed here, there is no consistency of generating **6-character** tiny string even for the longer values of number (system time in millis)
   *  So I have written the following algorithm   
        * Take the generated base-62 encoded short string
        * Check the size of the short string
            * If the length of the base-62 encoded string is exactly 6
                * Then check this short string is already available for the **Group**, here I am considering default group as **tiny** for ungrouped cards
                * If it is not already assigned to some **Group**, then we can take this short string for tiny URL construction
                * If it is not available then swap the possible characters of the 6 Character short string, then we will get 6 factorial combinations of the string, we won't consider all the combinations at a time, instead take a chunk (For example among 6 factorial, we will take 100) of these many combinations of available(not processed before) and send it to database to check available short string.
                * Once we got the available short string to use then we can pick one   
                * If we didn't get the available short string then process repeat of taking chunk of 6 factorial combinations and query the database
                * The process repeats until we get short string of 6 characters 
                * ###### Why did I take chunk of short string among 6 factorial combinations?
                    * This is to avoid too many database calls and here database also will respond quickly, since I have created an index for corresponding columns in the table
                    * Chance of coming to this extent is very less
            * If the length of the base-62 encoded is more than 6
                * Generate a random string of 6 character and give it to the above step until we get short string of 6 character
                * Every time we track(not in database) which is short is available and which is not
            * If the length of the base-62 encoded string is lesser than 6
                * Take the base-62 encoded short string and query in the database for this particular **Group** fetch short URLs starting with base-62 encoded string
                * Generate a unique and random string of 6 characters from base-62 character set and check for existence in previously fetched short strings from database
                * If it is not available in the fetched short strings then we can take it as required 6-digit short string
                * If not generate a unique and random string of 6 characters from base-62 character and proceed to check for non-existence in the fetched string from database, until we get the 6-character short string
                * It won't take much time, since we fetched only once and check for the existence of maximum 5 factorial times
   * In this algorithm group plays a major role, by default group name will be tiny for uncategorized cards
   
## SHARING SHORT URLs
   
 User can share the short urls on clicking the **SHARE CARD** icon button on the card or **SHARE GROUP CARDS** button on the group section
    * Once th user clicks on the share card icon, it will ask for **sender** and **receiver**'s email address
    * A Mail will trigger to the user on click on share. For this I have used **Google SMTP Server**
   
   > GOOGLE SMTP SERVER
    
   * Google providing SMTP services where we can send 2000 mails per day
   * I have integrated it with my **Go Tiny** backend application with the help of spring boot and configured the credentials in **AZURE**
   * Springboot is providing a way to send mail with any SMTP Server.

