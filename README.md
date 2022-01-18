# Team 16 Project Documentation

## Subdocuments

- [Arcitecture, styles and tactics](https://git.chalmers.se/courses/dit355/test-teams-formation/team-16/team-16-project-documentation/-/blob/main/subdocs/Architecture,%20styles%20and%20tactics.md)
- [Lessons learned & Conclusion](https://git.chalmers.se/courses/dit355/test-teams-formation/team-16/team-16-project-documentation/-/blob/main/subdocs/Lessons%20learned%20&%20Conclusion.md)
- [Management brief](https://git.chalmers.se/courses/dit355/test-teams-formation/team-16/team-16-project-documentation/-/blob/main/subdocs/Management%20brief.md)
- [Strategy & design decision](https://git.chalmers.se/courses/dit355/test-teams-formation/team-16/team-16-project-documentation/-/blob/main/subdocs/Strategy%20&%20design%20decision.md)
- [TeamContract](https://git.chalmers.se/courses/dit355/test-teams-formation/team-16/team-16-project-documentation/-/blob/main/subdocs/TeamContract.md)


## **Abstract** System Design

![Abstract conceptualization of the system](./diagrams/abstract-design.png)

## Component Diagram

![Component Diagram](./diagrams/component-diagram.png)

## Functional Decomposition Diagram

![Functional Decomposition Diagram](./diagrams/FunctionalDecomposition.png)

## Management details
- [Team Contract](./subdocs/TeamContract.md)
## Developers <a name="developers"></a>

<img src="https://avatars.githubusercontent.com/u/51012876?v=4" alt="Profile Picture DrakeAxelrod" width="20"/> [Drakeaxelrod](https://github.com/Drakeaxelrod)

<img src="https://avatars.githubusercontent.com/u/71592942?s=40&v=4" alt="Profile Picture Caisesiume" width="20"/> [Caisesiume](https://github.com/Caisesiume)

<img src="https://avatars.githubusercontent.com/u/71591829?v=4" alt="Profile Picture JohanAxell" width="20"/> [JohanAxell](https://github.com/johanaxell)

<img src="https://avatars.githubusercontent.com/u/81258179?v=4" alt="Profile Picture Jidarv" width="20"/> [Jidarv](https://github.com/Jidarv)

<img src="https://avatars.githubusercontent.com/u/81112288?v=4" alt="Profile Picture RobbanGit" width="20"/> [RobbanGit](https://github.com/RobbanGit)

<img src="https://avatars.githubusercontent.com/u/72571860?v=4" alt="Profile Picture Asiya-Ismail" width="20"/> [Asiya-Ismail](https://github.com/Asiya-Ismail)

- Klara Svensson

## Team Resources <a name="team resources"></a>

- [Trello](https://trello.com/b/Supm1hiE/dit355-group-16)
- [Discord](https://discord.gg/Xd6E9Nr2qP)
- [Gitlab]()
- [Appointment manager](https://git.chalmers.se/courses/dit355/test-teams-formation/team-16/team-16-project-booker)
- [User manager](https://git.chalmers.se/courses/dit355/test-teams-formation/team-16/team-16-project-authentication)
- [Frontend](https://git.chalmers.se/courses/dit355/test-teams-formation/team-16/frontend)
- [MQTT](https://git.chalmers.se/courses/dit355/test-teams-formation/team-16/team-16-project)
- [Gateway](https://git.chalmers.se/courses/dit355/test-teams-formation/team-16/team-16-gateway)

## Frontend

Our goal is to create an accessible and intuitive interface with functionality described below. It will support communication over the MQTT using protocol using websockets. This will enable duplex and asynchronous communication with the server while also removing the need to update the webpage in order to show an up-to-date visualisation of the current time slots available to the user.

### Requirements

1.1 Homepage: The Homepage will mainly consist of a Map showing the different clinics while also providing options to login and register as a new user.

1.2 Map: Should display the available clinics

1.3 Booking page: Showing available and unavailable time slots

1.4 User account: The system shall allow for user login, sign up and sign out. 

## Gateway and broker

- We will be using MQTT as our network protocol. This will enable usage of a publish/subscribe model in order to transfer resources between different units. This transmission is utilizing our broker as a middleman to send and receive messages between the different subsystems. These messages are not sent to a specific receiver, instead they are tagged with a topic and a message, and only the subsystems interested, subscribed, to this specific topic will be receiving them.
  Using this architecture style will facilitate our ability to be flexible, while also allowing for greater scalability. This is partly because the publish/subscribe model is asynchronous, but also because we are able to add or remove subscribers without having to do much extra programming. 

### Requirements

2.1 Should log all requests:  
A logger will be implemented to mainly provide another way to troubleshoot while also giving an extra perspective in regards to performance.

2.2 Must be able to receive data from frontend:  
As described above, the broker will be the middleman between the front- and the backend. It will track which subsystem is subscribed to what topic and so on.

2.3 Generate backend requests from frontend request:  
As soon as the front end publishes, eventually the backend should do the same to provide the requested resource back to where it was originally requested.

2.4 Must be able to send the backend request to the right subsystem

## Appointment manager

The appointment manager will handle booking requests from users. It will handle the available and booked appointments for dentist clinics, along with associated data, such as patient information. This subsystem will mostly act on-demand and will be used when a user requests to see the available appointments. The subsystem will collaborate with the user-manager in order to book appointments and verify users' that wish to book an appointment.

## User-manager

One part of the system will be handled by the user-manager. Said user-manager will handle the authentication and similar responsibilites related to user activities.
The user-manager will be used in collaboration with other subsystems to perform tasks, although this subsystem will be limited to its abilities and responsibilities.

The user manager handles the basic activities of users:
Login
Register
Logout
Authentication when requested and retrieving user information when requested

The login feature is performed by a local check from the user-manager subsystem.
The user authentication is stored in a middleware called Redux, and allows for efficient user authentication.

### Requirements

4.1 User Database:
Create MongoDB database
The user object in the database should follow the user schema
The user schema should contain the following(with the possibility to add further things): - id (autogenerated/created manually) - email/username - password

In 4.1, we create a MongoDB database through Atlas, which primarily provides simplicity and reliability, due to us being able to share database servers and only require a connection string to connect and use the database.

4.2 User Authentication Upon Request:
Check and verify user's credentials (when user is logging in): - email/username - password
If no user account exists, return message to user and deny actions that requires an account.


4.5 Create account:
User account adhere to schema

## Architectural drivers

### MQTT

One of our drivers are that we include MQTT and how we decided to design the implementation of it in our project. In our planned design, we decided to combine MQTT and the gateway broker into one unit, and thus can have them work together to fulfill their tasks. Considering that separating them would result in two smaller units, and likely also convolute the implementation of the communication between all units, including both of the respective units. As previously mentioned, we therefore decided to have them combined into one unit.
Due to the requirement to have at least four (4) independent systems, it can also be considered a constraint for us to combine them into one unit, and thus avoid having a system with few tasks and responsibilities.

Our MQTT instance runs on a QoS(Quality of Service) of 2. Due to the low risk of performance throttling, due to the design of our project, we deem it acceptable to have a QoS level of 2, without the risk of impacting our system significantly.

### Programming languages

Our choice to use TypeScript for our frontend, can be considered as a constraint of our project. TypeScript happens to be relatively similar to JavaScript, which we have experience from a previous course, which makes the development phase less stressful, and less error-prone. In order to minimize the potential risks followed by being less experienced with a programming language, we decided to use TypeScript, which shares a lot of syntax from JavaScript.

We have also decided to use React. This programming language is easy to learn, and contain plenty of functionality that we may use throughout the development phase.

### Choice of Databases

We have chosen to use MongoDB Atlas for our database, which provides a simple, yet effective alternative to hosting our own databases. We also believe that MongoDB Atlas is reliable, and have experience using their service in a previous course, which results in less confusion on how it works and how it is involved in the development phase.
This does however require an internet connection, but due to the widespread use of the internet, along with little data consumption, it happens to be a constraint we as a group are acceptable with.
