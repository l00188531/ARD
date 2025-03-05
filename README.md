### Context

- Our microservices application requires an automated deployment process that is easy to scale and adaptable. VM-based deployment has a lot of limitations in regards to workload automation through service scaling, fault tolerance and service discovery. Moreover, there is a shift to containerized applications in the cloud-native environment.

### Decision

- We have opted to adopt Kubernetes for container orchestration of our services.

- Services will be exposed as Pods that reside within the Kubernetes Namespaces.

- Scalability will be done through Horizontal Pod Autoscaling (HPA).

- Routing of external traffic will be performed by the Ingress Controller.

- Management of Kubernetes manifests will be via Helm as charts.

### Architecture Design
- Nginx Reverse Proxy
  - Listens on port 80 for HTTP traffic.
  - Routes frontend requests to the React service.
  - Routes backend API requests to the Flask service.
- Frontend (React)
  - Runs inside a Docker container.
  - Communicates with the backend via the Nginx proxy.
- Backend (Flask)
  - Runs inside a separate Docker container.
  - Listens on port 5000.
  - Handles API requests and business logic.
  - Communicates with the PostgreSQL database.
- Database (PostgreSQL)
  - Listens on port 5432.
  - Stores and retrieves application data.

###  Alternatives Considered

- Docker Swarm
  - More straightforward to configure
  - Cannot support scalability and ecosystem as well

- AWS ECS (Elastic Container Service)
  - Eases operations due to lower service management requirements
  - Tied to a AWS while having lesser mobility freedom than Kubernetes

- VM traditional deployment
  - No additional training for users
  - More manual supervision is needed for scaling
- Using Apache Instead of Nginx
  - Well-supported
  - Less performant for handling high concurrent requests

### Consequences

- Positive Outcomes

  - Allow self-healing and auto-scaling of resources

  - More economical and efficient use of resources

  - Standardized through definition of deployment manifests in Kubernetes

### Challenges

- Concepts of Kubernetes, in particular its architecture

- Difficulty of the initial configuration
