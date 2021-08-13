# Designing Cloud Native Microservices on AWS  (via DDD/EventStormingWorkshop)

![](docs/img/coffee.jpg)
_Picture license-free from [Pexels](https://www.pexels.com/photo/background-beverage-breakfast-brown-414645/)_

Building software is hard. Understanding the business needs of the software is even harder. In almost every software development project, there will always be some form of gap between the requirements of the business users and the actual implementation.

Tạo ra phần mềm là 1 việc khó. Hiểu business cần gì ở phần mềm còn khó hơn. Trong hầu hết các dự án phần mềm, luôn luôn có những phần là GAP giữa yêu cầu của business users và thực tế triển khai.

As a developer, knowing how to narrow this gap can help you go a long way to building applications that are relevant for the users. Using a Domain Driven Design approach, delivered via Event Storming, it can help to reduce the time it takes for everyone in the project team to understand a business domain model.

Khi bạn là developer, làm thế nào để biết cách thu hẹp khoảng cách này có thể giúp bạn đi một chặng đường dài để xây dựng các ứng dùng phù hợp cho người dùng. Sử dụng phương pháp Domain Driven Design, được cung cấp thông qua Event Storming, có thể giúp bạn giảm thời gian, điều đó sẽ làm cho mọi thành veien trong project team hiểu được một model của business domain


> Theory and Practice: Learning in the Real world cases - Lý thuyết và thực hành : Học từ case study thực tế.

**Go through all of the learning journey, develop-->build-->deploy artifacts on AWS** (Đi hết từng bước của hành trình, develop --> build --> deployment trên AWS)

![](docs/img/Coffeeshop-architecture.png)





## Table of Contents
- [00 - Event Storming](#eventstorming)
  - [What is Event Storming?](#what-is-event-storming)
  - [Whom is it for?](#whom-is-it-for)
  - [Event Storming Terms](#event-storming-terms)
  - [Event Storming Benefits](#event-storming-benefits)
  - [Event Storming Applications](#event-storming-applications)
- [01 - Hands-on: Events exploring](docs/01-hands-on-events-exploring/README.md)
- [02 - Cafe business scenario](docs/02-coffee-shop-scenario/README.md)
- [03 - Roles, Commands, and Events Mapping](docs/03-roles-commands-events-mapping/README.md)
  - [Key Business events in the coffeeshop](docs/03-roles-commands-events-mapping/README.md#key-business-events-in-the-coffeeshop)
  - [Commands and Events mapping](docs/03-roles-commands-events-mapping/README.md#commands-and-events-mapping)
  - [Roles](docs/03-roles-commands-events-mapping/README.md#roles)
  - [Exceptions or risky events](docs/03-roles-commands-events-mapping/README.md#exceptions-or-risky-events)
  - [Re-think solutions to serve risky events](docs/03-roles-commands-events-mapping/README.md#re-think-solutions-to-serve-risky-events)
  - [Aggregate](docs/03-roles-commands-events-mapping/README.md#aggregate)
  - [Bounded Context forming up](docs/03-roles-commands-events-mapping/README.md#bounded-context-forming-up)
- [04 - Modeling and Development](docs/04-modeling-and-development/README.md)
  - [Specification by Example](docs/04-modeling-and-development/README.md#specification-by-example)
  - [TDD within Unit Test environment](docs/04-modeling-and-development/README.md#tdd-within-unit-test-environment)
  - [Generate unit test code skeleton](docs/04-modeling-and-development/README.md#generate-unit-test-code-skeleton)
  - [Implement Domain Model from code Skeleton](docs/04-modeling-and-development/README.md#implement-domain-model-from-code-skeleton)
  - [Design each Microservices in Port-adapter concept](docs/04-modeling-and-development/README.md#design-each-microservices-in-port-adapter-concept)
- [05 - Deploy Applications by AWS CDK](docs/05-deploy-applications-by-cdk/README.md) 
<!---
- [05 - Domain Driven Design Tactical design pattern guidance](05-ddd-tactical-design-pattern)
- [06 - Actual Implementation](06-actual-implementation)
- [07 - Infrastructure as Code by CDK](07-iaac-cdk)
- [08 - Deploy Serverless application](08-deploy-serverless-app)
- [09 - Deploy Containerized application](09-deploy-containerized-app)
- [10 - Build up CI/CD pipeline](10-build-up-cicd-pipeline)
--->

# Event Storming
![image](docs/img/problemsolving.png)

## What is Event Storming? Event Stomrming là gì
Event Storming is a **rapid**, **lightweight**, and often under-appreciated group modeling technique invented by Alberto Brandolini, that is **intense**, **fun**, and **useful** to **accelerate** project teams. It is typically offered as an interactive **workshop** and it is a synthesis of facilitated group learning practices from Gamestorming, leveraging on the principles of Domain Driven Design (DDD).

Event Storming là một mô hình kỹ thuật nhanh, ligthweight, và thường group các under-appreciated (không được đánh giá đúng mức) được phát minh bở Alberto Brandolini, nó instence, fun và hữu ích để tăng tốc các project team. Nó thường được đề nghị như một interactive workshop  và nó là sự tổng hợp của facilicated group learning practices from Gamestorming, tận dụng các nguyên tắc của Domain Driven Design (DDD)

You can apply it practically on any technical or business domain, especially those that are large, complex, or both.

## Whom is it for? Nó dành cho ai
Event Storming isn't limited to just for the software development team. In fact, it is recommend to invite all the stakeholders, such as developers, domain experts, business decision makers etc to join the Event Storming workshop to collect viewpoints from each participants.

Event Storming không giới hạn chỉ cho team phát triển phần mềm. Thực tế nó được gợi ý để mời tất cả stakeholders, như là developers, domain experts, business decision makers .... để tham gia Event Storming workshop để thu thập các quan điểm của các đối tượng tham gia.
## Event Storming Terms

![Event Storming](https://storage.googleapis.com/xebia-blog/1/2018/10/From-EventStorming-to-CoDDDing-New-frame-3.jpg)

> Reference from Kenny Bass - https://storage.googleapis.com/xebia-blog/1/2018/10/From-EventStorming-to-CoDDDing-New-frame-3.jpg

Take a look on this diagram, there are a few colored sticky notes with different intention:
Hãy nhìn vào sơ đồ này, có một vài ghi chú với mục đích khác nhau:

* **Domain Events** (Orange sticky note) - Describes *what happened*. Represent facts that happened in a specific business context, written in past tense
 ( Domain Events - màu cam : Mô tả "việc gì đã xảy ra". Biển diễn lại thực tế những gì đã diễn ra của một specific business context, được viết ở thì qk)
 
* **Actions** aka Command (Blue sticky note) - Describes an *action* that caused the domain event. It is a request or intention, raised by a role or time or external system
(Action hay Command - màu blue - Mô tả lại "hành động" mà nguyên nhân của domain event . Đó là 1 yêu cầu hoặc ý định, được nêu ra bởi 1 vai trò hoặc thời gian hoặc hệ thống bên ngoài.

* **Information** (Green sticky note) - Describes the *supporting information* required to help make a decision to raise a command
( Information - green : Mô tả "thông tin hỗ trợ" được yêu cầu để hỗ trợ đưa ra một quyết định để đưa ra 1 command.)

* **Consistent Business Rules** aka Aggregate (Yellow sticky note) "" 
* "Các Quy định của Business không được thay đổi" hay Aggregate 
    * Groups of Events or Actions that represent a specific business capability
    (Nhóm các Events hoặc Actions mà đại diện cho một tình huống kinh doanh cụ thể)
    * Has the responsibility to accept or fulfill the intention of command
    (Có trách nhiệm chấp nhận hoặc thực hiện ý định của command)
    * Should be in small scope
    (Phải trong 1 phạm vi hẹp)
    * And communicated by eventual consistency
    (Và được truyền đạt/ trao đổi một cách nhât quán)
    
* **Eventual Consistent Business rules** aka Policy (Lilac sticky note) "Các quy tắc kinh doanh nhất quán" - Chính sách
    * Represents a process or business rules. Can come from external regulation and restrictions e.g. account login success/fail process logic.
     Trình bày một process hoặc quy tắc kinh doanh. Có thể đến từ các quy định bên ngoài và không được cho phép/hạn chế thực hiện ví dụ như logic của việc đăng nhập  seccess/fail.
## Event Storming Benefits - Lợi ích của Event Storming 

Business requirements can be very complex. It is often hard to find a fluent way to help the Product Owner and Development teams to collaborate effectively. Event storming is designed to be **efficient** and **fun**. By bringing key stakeholder into the same room, the process becomes:
Các yêu cầu của business có thể rất phức tạp. Nó thường khó có thể tìm được 1 fluent way để giúp Product Owner và Development teams phối hợp hiệu quả. Event Storming được thiết kế để tạo tính hiệu quả và vui vẻ. Bằng việc đua các stakeholder quan trọng vào cùng 1 phòng và việc thực hiện ES sẽ diễn ra :

- **Efficient:** Everyone coming together in the same room can make decisions and sort out differences quickly. To create a comprehensive business domain model, what used to take many weeks of email, phone call or meeting exchanges can be reduced to a single workshop.
Hiệu quả : mọi người cùng nhau ở trong cùng 1 phòng để đưa ra quyết định và phân loại nhanh chóng các điểm khác nhau. Để tạo một mô hình business domain 1 ách toàn diện hơn là việc mất hàng tuần để email, phone call và các cuộc họp trao đổi có thể được rút ngắn chỉ trong 1 workshop.

- **Simple:** Event Storming encourages the use of "Ubiquitous language" that both the technical and non-technical stakeholders can understand.

- **Fun:** Domain modeling is fun! Stakeholders get hands-on experience to domain modeling which everyone can participate and interact with each other. It also provides more opportunities to exchange ideas and improve mindsharing, from various perspective across multiple roles.

- **Effective:** Stakeholders are encouraged not to think about the data model, but about the business domain. This puts customers first and working backwards from there, achieves an outcome that is more relevant.

- **Insightful:** Event Storming generate conversations. This helps stakeholders to understand the entire business process better and help to have a more holistic view from various perspective.

## Event Storming Applications

There are many useful applications of Event Storming. The most obvious time to use event storming is at a project's inception, so the team can start with a common understanding of the domain model. Some other reasons include:
* Discovering complexity early on, finding missing concepts, understanding the business process;
* Modelling or solving a specific problem in detail;
* Learning how to model and think about modelling.

Event Storming can also help to identify key views for your user interface, which can jump start Site Mapping or Wireframing.

Let's get started with a quick example to demonstrate how to run a simple Event Storming.

[Next: 01 Hands-on Events Exploring >](docs/01-hands-on-events-exploring/README.md)


## License

This library is licensed under the MIT-0 License. See the LICENSE file.
