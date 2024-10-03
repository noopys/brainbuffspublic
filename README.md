Brain Buffs is a software service that delivers adaptive SAT prep to over 1000 users and 4 enterprise customers. Our platform revolutionizes SAT Prep with targeted practice. Users just practice, and our algorithms show them exactly where they can improve their score the most giving them custom practice questions to realize those improvements. Every day, students practice hundreds of SAT questions on the platform. 

In order to deliver a commercial grade platform, the Brain Buffs Architecture relies on 20 AWS Services, 8 tables with over 20,000 records, and 90 custom React Components.
# Overview



#### Architecture Diagram 
![[BrainBuffsDiagram.png]]
#### React App
The frontend of Brain Buffs is built on React, leveraging its component-based architecture. Utilizing React provided us with the flexibility to craft a custom UI while capitalizing on the extensive ecosystem of reusable components. Tailwind CSS, a utility-first CSS framework, is employed for styling, enabling rapid and consistent design implementation and easy understanding of design choices in code. 

React's declarative state management and unidirectional data flow made it an ideal choice for architecting our application. By adhering to the principle of state localization and selectively propagating key state elements through a higher-order AuthContext component, we've maintained a streamlined state management approach for our complex application. Our React components interface with AWS Lambda microservices via RESTful API calls, facilitating efficient state management. We've also implemented the Container/Presentational pattern, a React best practice, allowing container components to fetch and manage state at the route level, then pass it down to pure presentational child components for rendering. Authentication tokens, persisted in the browser's sessionStorage, are used to authorize each request to AWS Lambda endpoints for user-specific data retrieval.

The application boasts over 30 distinct routes (pages) accessible to users and encompasses 90 custom components. Brain Buffs integrates [DaisyUI](https://www.daisyui.com), a component library built on top of Tailwind, for UI elements such as buttons, accordions, and navigation components. The majority of the application's user interface was initially prototyped in Figma, a vector graphics editor and prototyping tool, before being implemented in code.
#### Serverless AWS Lambda Backend

The Brain Buffs application offers a comprehensive suite of user interactions. As a fully-fledged adaptive learning platform, we have implemented a robust set of features enabling students to engage with practice questions, leveraging adaptive algorithms for intelligent content sequencing, persisting and retrieving previous assignments, facilitating teacher-student interactions through question assignments, allowing for customized practice sessions, and approximately 40 additional functionalities.

To efficiently deliver this feature-rich platform at scale, catering to fluctuating user loads ranging from 4 to 400 concurrent users depending on peak times, we adopted a microservices architecture. The backend infrastructure is composed of 50 discrete AWS Lambda functions, each responsible for executing specific actions. This serverless approach using AWS Lambda necessitated strict adherence to stateless application design principles and RESTful API standards.

By implementing event-driven, function-as-a-service (FaaS) architecture through AWS Lambda, we've achieved high scalability, fine-grained resource allocation, and cost-effectiveness. Each Lambda function is designed to handle a specific microservice, ensuring loose coupling and enabling independent scaling and deployment of services. This architecture allows us to maintain high performance and responsiveness across varying levels of user concurrency, while optimizing resource utilization during periods of low activity.
#### DynamoDB 

All of the application's data is persisted in Amazon DynamoDB, a fully managed NoSQL database service. We opted for DynamoDB to leverage its robust auto-scaling capabilities and on-demand pricing model, which aligns perfectly with our variable workload requirements. To effectively navigate the trade-offs inherent in NoSQL systems, we conducted thorough data access pattern analysis prior to schema design. This proactive approach ensured that we could optimize our table structures to mitigate the need for complex join operations, which are not natively supported in NoSQL databases.

Furthermore, as NoSQL paradigms often necessitate data denormalization for performance optimization, we've implemented rigorous data consistency protocols in our backend services. These protocols ensure that our Lambda functions maintain data integrity by updating all relevant denormalized copies when modifications occur. We've designed our data model to support single-table design patterns where appropriate, utilizing composite primary keys and sparse secondary indexes to enable efficient querying across multiple access patterns.

To optimize read performance and reduce costs, we've implemented strategic use of DynamoDB's read consistency models, opting for eventually consistent reads where real-time consistency isn't critical, and strongly consistent reads for operations requiring immediate data accuracy. Additionally, we've leveraged DynamoDB Streams in conjunction with AWS Lambda to build event-driven architectures, enabling real-time data processing and maintaining derived data structures.

