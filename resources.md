Throughout this course we’ll link a lot of different websites and resources to check out. This way, no matter how you learn, you’ll always have resources for digging deeper on a specific subject.

Below we have compiled a list of some of our favorites.

## Resources:
* **People**
  * Kelsey Hightower [@kelseyhightower](https://twitter.com/kelseyhightower)
  * Carter Morgan [@_askcarter](https://twitter.com/_askcarter)
  * Adrian Cockcroft [@adrianco](https://twitter.com/adrianco)
  * Gundega Dekena [@pytonc](https://twitter.com/pytonc)

* **Books**
  * [Kubernetes: Up and Running, Kelsey Hightower](http://shop.oreilly.com/product/0636920043874.do) The definitive book on Kubernetes. This has been a great resource while making this course.
  * [Building Microservices: Defining Fine-Grained Systems](http://shop.oreilly.com/product/0636920033158.do) This is the book Kelsey reads before giving talks about microservices. It’s that good.

* **Articles**
  * Martin Fowler on the [Pros](http://martinfowler.com/articles/microservices.html) and [Cons](http://martinfowler.com/articles/microservice-trade-offs.html) of Microservices
  * [12-Fractured Apps](https://medium.com/@kelseyhightower/12-fractured-apps-1080c73d481c) - One of Carters favorites articles where Kelsey breaks down problems with many modern apps and how 12-factor app methodology solves those technical woes, Find out more about the 12-factor manifesto here: http://12factor.net/
  * Tim O’Reilly, ["Open Data: Small Pieces Loosely Joined"](http://radar.oreilly.com/2006/09/open-data-small-pieces-loosely.html) For the history nerds: Quite possibly the first article about Microservices Architecture (before it even had a name).

* **Videos**
  * Adrian Cockroft ["The Evolution of Microservices"](http://www.ustream.tv/recorded/86151804)
  * Adrian Cockroft ["The State of the Art in Microservices"](https://www.youtube.com/watch?v=pwpxq9-uw_0) (docker specific)
  * Martin Fowler ["Microservices"](https://www.youtube.com/watch?v=wgdBVIX9ifA) at goto
  * Craig McLuckie ["The Next Chapter in Native Cloud Computing"](https://www.youtube.com/watch?v=mPhjFYXoAD0) on cloud-native computing as being: container-packaged, dynamically-scheduled, and microservices-oriented
  
## Tools we use in the course
* **The Go Programming Language**
  * Our app is written in Go. If you’re not already using Go, you owe it to yourself to try it out.
  * https://golang.org/

* **Google Cloud Shell**
  * A free temp VM preloaded with the tools need to manage our clusters.
  * https://cloud.google.com/shell/docs/
  
* **Docker**
  * We use Docker to package, distribute, and run our application.
  * https://www.docker.com/
  * Understand how Docker works and how you can use it - https://www.docker.com/what-docker
  * Introduction to Docker - https://opensource.com/resources/what-docker
  * Here are two articles with a good write-ups about containers, if you want to go more in-depth about container technology:
    * [Containers bring a skinny new world of virtualization to Linux](http://www.itworld.com/article/2698646/virtualization/containers-bring-a-skinny-new-world-of-virtualization-to-linux.html)
    * [What are containers and why do you need them?](http://www.cio.com/article/2924995/enterprise-software/what-are-containers-and-why-do-you-need-them.html)
  * Find out more here: [Alpine container image on DockerHub](https://hub.docker.com/_/alpine/)
  * Article - [Docker gets minimalist with plan to migrate images to Alpine Linux](http://siliconangle.com/blog/2016/02/09/docker-gets-minimalist-with-plan-to-migrate-images-to-alpine-linux/)
  * Comment on HackerNews by [Solomon Hykes, founder and CTO of Docker (on the move)](https://news.ycombinator.com/item?id=11000827)
  
* **Kubernetes**
  * Once we have an application, we use Kubernetes to handle the heavy lifting of managing, deploying, and scaling our application.
  * http://kubernetes.io/
  * How does Kubernetes schedule Pods onto machines (called Nodes in Kubernetes)?
  * Well, Kelsey says it’s a lot like tetris: https://www.youtube.com/watch?v=Po_MEdnUVDE
  * Also, it’s probably a good idea to read up on Pods, my friend: http://kubernetes.io/docs/user-guide/pods/
  * User guide - http://kubernetes.io/docs/user-guide/liveness/
  
* **Google Container Engine (GKE)**
  * GKE is a hosted Kubernetes service
  * https://cloud.google.com/container-engine/
  
## Public vs Private Registries

A lot of the power of container images stems from the ability to host and download them from cloud registries. This makes it easy to share containers without having to use complex pipelines for distributing them.

When it comes to registries there are a few options. Below are a few different registries.

### Docker Hub
Docker Hub is the registry we’re using in this class. Go ahead and sign up for docker hub and create a repository so that you can follow along with the remaining lessons: https://hub.docker.com/

### Quay
Quay is another popular registry because of it’s rich automated workflow for building containers from github. https://quay.io/

### Google Cloud Registry
Finally, Google Cloud Registry (GCR) is a strong options for large enterprises. https://cloud.google.com/container-registry/docs/

### Comparison Of 4 Registries
A write up comparing some of the different registries: http://rancher.com/comparing-four-hosted-docker-registries/
